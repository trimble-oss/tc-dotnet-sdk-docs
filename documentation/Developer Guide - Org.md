# Trimble.Connect.Org.Client.NET SDK Developer Guide

## Content

1. [Overview](#overview)
2. [Authentication Flow](#flow)
3. [Code Snippets](#snippets)
   - [Initializing OrgClient](#initialize-orgclient)
   - [Paging Mechanism](#paging)
   - [Delegating Handlers](#delegating-handlers)
   - [Get for Org](#get-org)
   - [CRUD Operations for Node](#crud-node)
   - [CRUD Operations for Tree](#crud-tree)

### Acronyms

- **TID** - Trimble Identity

---

## <a name="overview">Overview</a>

The Trimble Connect Organization Service Client (Org Client) provides .NET SDK wrappers for interacting with the organization-level functionalities of the Trimble Connect ecosystem. It allows for seamless management of organizations, members, and roles.

**Key Features:**

- **Organization Management:** Create, update, and manage organization details within Trimble Connect.
- **User Role Management:** Assign and update user roles within an organization.
- **Authentication Support:** Utilizes Trimble Identity (TID) for secure authentication.
- **Environment Options:**
  - **STAGE:** `https://org-api.stage.connect.trimble.com/v1`
  - **PROD:** `https://org-api.connect.trimble.com/v1`
- **Service API Wrappers:** Simplifies integration with Trimble Connect's organization-level services.

---

## <a name="flow">Authentication Flow</a>

The OrgClient follows a secure authentication flow using Trimble Identity (TID). This ensures authorized access to organization-related services in Trimble Connect.

### Steps in the Authentication Flow:

1. **Client Configuration Initialization**
   - Create an `OrgClientConfig` object containing service configuration details.

2. **Credentials Provider Initialization**
   - Use an `ICredentialsProvider` to manage authentication tokens.

3. **Client Initialization**
   - Initialize `OrgClient` with `OrgClientConfig` and `ICredentialsProvider`.

4. **Token Inclusion in Requests**
   - The `TrimbleConnectHttpClient` ensures that all requests include valid authentication tokens.

5. **Handling Requests**
   - Tokens are fetched, refreshed if expired, and included in headers for API calls.

```c#
Authenticate with TID

CredentialsProvider = new PasswordCredentialsProvider(
                new Uri(Config.AuthorityUrl),
                new ClientCredentials(Config.ClientId, Config.ClientKey, new Uri(Config.RedirectUrl), Config.ClientName),
                Config.ParseCredentials(Config.UserAndPassword));
OrgClient = new OrgClient(
                new OrgClientConfig { ServiceURI = new Uri(Config.ServiceUrl) },
                CredentialsProvider);

RegionsConfig.RegionsUri = new Uri(Config.TrimbleConnectServiceUrl + "regions");
```
---

## <a name="snippets">Code Snippets</a>

### <a name="initialize-orgclient">Initializing OrgClient</a>

```c#
public OrgClient(IClientConfig config, ICredentialsProvider credentialsProvider)
```

---

### <a name="paging">Paging Mechanism</a>

```c#
var currentPage = await orgClient.ListNodesAsync(request, cancellationToken).ConfigureAwait(false);

            while (currentPage != null)
            {
                // Provide a chance to bail out before attempting to process the current page or fetch the next page
                cancellationToken.ThrowIfCancellationRequested();

                Task<NodesPage> nextPageTask = null;

                if (currentPage.Next != null)
                {
                    // Fetch the next page asynchronously
                    nextPageTask = orgClient.ListNodesAsync(currentPage.Next, cancellationToken);
                }

                // Process the current page while the next page is already being fetched
                resultPageProcessor.Invoke(currentPage);

                currentPage = nextPageTask != null ? await nextPageTask.ConfigureAwait(false) : null;
            }
---

### <a name="delegating-handlers">Delegating Handlers</a>

```c#

var handler = new Handler();
var config = new OrgClientConfig { ServiceURI = new Uri(Config.ServiceUrl) };
config.HttpHandlers.Add(handler);
using (var client = new OrgClient(config, Global.CredentialsProvider))
            {
                await this.MakeAPICall(client).ConfigureAwait(false);
                Assert.AreEqual(1, handler.Called);
            }
}
```

---

### <a name="get-org">Get for Org</a>

```c#

var BatchGetRequest = new BatchGetAsync{
    Fields = $"Attributes",
}

var BatchGetResponse = await OrgClient.BatchGetAsync(BatchGetRequest).ConfigureAwait(false);

```

---

### <a name="crud-node">CRUD Operations for Node</a>

```c#

//Get Node
var GetNodeRequest = new GetNodeAsync{
    NodeId = $"Nodeid",
    ForestId =$"Forestid",
    TreeId = $"Treeid",
    Fields = $"Attributes"
    
}

var GetNodeResponse = await OrgClient.GetNodeAsync(GetNodeRequest).ConfigureAwait(false);

//List Node
var ListNodesRequest = new ListNodesAsync{
    ForestId =$"Forestid",
    TreeId = $"Treeid",
    Fields = $"Attributes"
}

var ListNodesResponse = await OrgClient.ListNodesAsync(ListNodesRequest).ConfigureAwait(false);

//Create Node
var CreateNodeRequest = new CreateNodeAsync{
    ForestId =$"Forestid",
    TreeId = $"Treeid",
    Fields = $"Attributes"

}

var CreateNodeResponse = await OrgClient.CreateNodeAsync(CreateNodeRequest).ConfigureAwait(false);

//Search Node
var SearchNodesRequest = new SearchNodesAsync{
    ForestId =$"Forestid",
    TreeId = $"Treeid",
    Fields = $"Attributes"

}

var SearchNodeResponse = await OrgClient.SearchNodesAsync(SearchNodesRequest).ConfigureAwait(false);

//Update Node
var UpsertNodeRequest = new UpdateNodeAsync{
    NodeId = $"Nodeid",
    ForestId =$"Forestid",
    TreeId = $"Treeid",
    Fields = $"Attributes"
}

var UpdateNodeResponse = await OrgClient.UpdateNodeAsync(UpsertNodeRequest).ConfigureAwait(false);

//Delete Node
var DeleteNodeRequest = new DeleteNodeAsync{
    NodeId = $"Nodeid",
    ForestId =$"Forestid",
    TreeId = $"Treeid",
    Fields = $"Attributes"
}

var DeleteNodeResponse = await OrgClient.DeleteNodeAsync(DeleteNodeRequest).ConfigureAwait(false);

```

---

### <a name="crud-tree">CRUD Operations for Tree</a>

```c#

//Get Tree
var GetTreeRequest = new GetTreeAsync{
    ForestId =$"Forestid",
    TreeId = $"Treeid",
    Fields = $"Attributes"
}

var GetTreeResponse = await OrgClient.GetTreeAsync(GetTreeRequest).ConfigureAwait(false);

//List Tree
var ListTreesRequest = new ListTreesAsync{
    ForestId =$"Forestid",
    Fields = $"Attributes"
}

var ListTreesResponse = await OrgClient.ListTreesAsync(ListTreesRequest).ConfigureAwait(false);

//Create Tree
var CreateTreeRequest = new CreateTreeAsync{
    ForestId =$"Forestid",
    Name = $"NameOfTree",
    Type = $"TypeOfTree",
    Fields = $"Attributes"
}

var CreateTreeResponse = await OrgClient.CreateTreeAsync(CreateTreeRequest).ConfigureAwait(false);

//Update Tree
var UpsertTreeRequest = new UpdateTreeAsync{
    ForestId =$"Forestid",
    Name = $"NameOfTree",
    Type = $"TypeOfTree",
    Fields = $"Attributes"
}

var UpdateTreeResponse = await OrgClient.UpdateTreeAsync(UpsertTreeRequest).ConfigureAwait(false);

//Delete Node
var DeleteTreeRequest = new DeleteTreeAsync{
   ForestId =$"Forestid",
   TreeId = $"Treeid"
}

var DeleteTreeResponse = await OrgClient.DeleteTreeAsync(DeleteTreeRequest).ConfigureAwait(false);



```



