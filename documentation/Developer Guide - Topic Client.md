# Trimble.Connect.Topic.Client .NET SDK Developer Guide

### Content

1. [Overview](#overview)
2. [Authentication/Initialization](#authentication)
   + [Example: Sign-in TC Platform Services](#sign-in-example)
3. [Class Diagram](#class-diagram)
4. [Extensibility mechanisms](#extensibility-mechanisms)
5. [Search API](#search)
6. [Code snippets](#tc_snippets)
   + [Example: Access TC Services](#example-access-tc)
   + [Example: How to enumerate paged results](#example-paging)
   + [Example: Using delegating handlers to implement network activity indicator](#example-delegating-handlers)

### Acronyms

TID - Trimble Identity, identity provider

TC - Trimble Connect

## <a name="overview">Overview</a>

The Trimble.Connect.Topic.Client component represents a set of TC Topic Services REST API wrappers. Wrappers provide full create, read, update, delete (CRUD) functionality against TC topics entities. Wrappers work on the same abstraction level as the web API and mostly map RESTful API requests one-to-one into .NET HTTP requests.

This layer's responsibilities include:

* Consistent error handling and convenient ways to deal with varying network performance (like async mechanisms and progress reporting).
* Providing an easy to use abstraction for your targeted platforms and programming language.
* Supporting middleware message handlers
* Leveraging strongly typed entity recognition for data modeling
* Graceful error handling
* Adherence to HTTP conventions and standards

The TC REST API specification can be found here: 

* Staging: [https://app.swaggerhub.com/apis/Trimble-Connect/topic-stage/0.1#](https://app.swaggerhub.com/apis/Trimble-Connect/topic-stage/0.1#)

The video tutorial: [SDK Authentication using the Identity library](https://youtu.be/XaQXK4Y3TpE?t=2m20s).

## <a name="authentication">Authentication/Initialization</a>

The clients can use Trimble.Identity.OAuth.AuthCode for interactive workflow authentication or Trimble.Identity.OAuth.Password for headless applications (like from test code). The clients can also provide any custom implementation of ICredentialsProvider.

### <a name="example-sign-in">Example: Topic Service Client Initialization </a>

Authenticate with TID and initialize the Topic service client. 

Topic Service URI - Stage : https://open11.stage.connect.trimble.com/bcf/2.1/

Topic Service URI - Prod : https://open11.connect.trimble.com/bcf/2.1/

    var clientCreds = new ClientCredential(ClientId, ClientKey)
            {
                RedirectUri = new Uri(RedirectUrl),
            };
    
    var authCtx = new AuthenticationContext(clientCreds)
            {
                AuthorityUri = new Uri(AuthorityUrl),
            };
    
    var provider = new AuthCodeCredentialsProvider(authCtx)
            {
                AuthenticationRequest = new InteractiveAuthenticationRequest();
            };
    
    // If client already knows the access token
    var provider = new TokenCredentialsProvider(accessToken);
    
    var config = new TopicServiceClientConfig { ServiceURI = TopicServiceUri};
    config.RetryConfig = new RetryConfig { MaxErrorRetry = 1 };
    
    var client = new TopicServiceClient(config, provider);

## <a name="class-diagram">Class Diagram</a>

Below is a class diagram that describes the API programming models used in the SDK component to access the TC Topics service.

![Class diagram](images/TopicServiceClient_ClassDiagram.png)

## <a name="extensibility-mechanisms">Extensibility mechanisms</a>

The SDK client component is designed to be backward and forward compatible. This means that applications released with older versions of the SDK should be still functional when new TC Topic Service APIs release. Also if by some reason app developers don't want to migrate to newer versions of the SDK but need access to new TC Topic Service APIs this can be done as well by using extension mechanisms.

The following mechanisms are used to achieve this flexibility:

1. Each entity has a `Properties` attribute implemented as a generic dictionary. This is a collection of "unknown" or "open" properties. E.g. if a new property is added to the entity after SDK has been released app can access such property from this dictionary.
2. We prefer string literals to enums. This makes it possible to deserialize new values even if they were not known at the time of SDK compilation.
3. All updates to entities are done using PATCH requests instead of PUT.
4. There is a generic mechanism to pass additional query parameters to the `GetAllAsync` method.
5. Application developer can provide custom `HttpMessageHandler` and `DelegatingHandlers` when creating `TrimbleConnectClient`. This is very powerfull mechanism to implement custom processing in the http pipeline. One example can be found [below](#example-delegating-handlers).

### <a name="tc_snippets">Code Snippets</a>

Below are some examples on using the Trimble Connect Topic Client component.
Full sample applications can be found on [github](https://github.com/Trimble-Connect/samples)

### <a name="example-access-tc">Example: Access TC Topic API endpoints</a>

    / Get connect project
    var project = this.TopicServiceClient.Projects.GetAsync(project.Identifier);
    
    // get project specific wrapper
    using (var projectClient = this.TopicServiceClient.GetProjectClient(this.Project))
    {
        await projectClient.CreateExtensionsAsync(extension);
        var extn = await projectClient.GetExtensionsAsync();
    }
    
    // working with topics
    using (var projectClient = this.TopicServiceClient.GetProjectClient(this.Project))
    {
        var response = await projectClient.Topics.CreateAsync(topic);
        var getResult = await projectClient.Topics.GetAsync(response.Identifier);
    }
    
    // working with comments
    using (var projectClient = this.Client.GetProjectClient(this.Project))
    {
        var response = await projectClient.Topics.CreateAsync(topic);
    
        using (var topicClient = projectClient.GetTopicClient(response))
        {
             var localComment = new Comment
             {
                 CommentString = "Comment description",
             };
             var comment = await topicClient.Comments.CreateAsync(localComment);
        }
    }

### <a name="example-paging">Example: How to enumerate paged results</a>

Most methods that return a list of entities (like `GetAllAsync` or `ListAsync` method) accept a `pageSize`  paramter and return an instance of `IQueryResult<T>`. Entities should be read from the `IQueryResult<T>` page by page. Note that `IQueryResult<T>` can be enumerated only once.

    var page = await this.TopicServiceClient.Projects.GetAllAsync(...);
    do
    {
        foreach(var entity in page)
        {
            // ... process each entity
        }
    
        if (page.HasMore)
        {
            page = await page.GetNextPageAsync(cancellationToken);
        }
        else
        {
            break;
        }
    }
    while (true);

There is a helper extension method `ReceiveAll` that encapsulates the enumeration logic for all pages and accepts a callback to be called for each page.

    List<Project> projects = new List<Project>();
    await this.TopicServiceClient.Projects.GetAllAsync(pageSize: 20).ReceiveAllAsync((page) =>
    {
         projects.AddRange(page);
    });
	
	using (var client = this.TopicServiceClient.GetProjectClient(this.Project))
    {
	    List<Topic> allTopics = new List<Topic>();
        await client.Topics.ListAsync(pageSize: 2).ReceiveAllAsync((page) =>
        {
            allTopics.AddRange(page);
        });
    }

### <a name="example-delegating-handlers">Example: Using delegating handlers to implement network activity indicator</a>

The code snippet below demonstrates how to use http delegating handlers to implement network activity indicator in the app.

    public class BusyHandler : DelegatingHandler
    {
        private int _callCount;
        private readonly Action<bool> _busyIndicator;
    
        public BusyHandler(Action<bool> busyIndicator)
        {
            _busyIndicator = busyIndicator;
        }
    
        protected override async Task<HttpResponseMessage> SendAsync(
          HttpRequestMessage request, CancellationToken cancellationToken)
        {
             // update the count by one in a single atomic operation. 
             // If we get a 1 back, we know we just went 'busy'
             var outgoingCount = Interlocked.Increment(ref _callCount);
             if (outgoingCount == 1)
             {
                 _busyIndicator(true);
             }
    
             var response = await base.SendAsync(request, cancellationToken);
    
             // decrement the count by one in a single atomic operation.
             // If we get a 0 back, we know we just went 'idle'
             var incomingCount = Interlocked.Decrement(ref _callCount);
             if (incomingCount == 0)
             {
                 _busyIndicator(false);
             }
    
             return response;
        }
    }
