# Downloading and uploading large files

When using the Connect SDK you use the **TrimbleConnectClient** to communicate with the service. The same client is being used to request both the entity data and the files. The ___Timeout___ property of **TrimbleConnectClient** is used to set the time limit for the entire entity request.

There are two suggested approaches for time limiting the file requests.

### Using per-request cancellation token

All SDK file upload/download methods accept a ***CancellationToken*** as a parameter. If you are tying that ___CancellationToken___ to a user action, you can still timebox the entire operation by combining the tokens and passing the linked token. Application can keep track of the connection speed, and based on that and the file size can estimate a good timeout value.

     // cancellationToken is a token bound to user interaction
     ...
     using (var timeoutCancellation = new CancellationTokenSource())
     using (var linkedCts = CancellationTokenSource.CreateLinkedTokenSource(cancellationToken, timeoutCancellation.Token))
     {
      ...
      var minimumSpeed = 10 * 1024; // our estimation of worst case request speed
	  var requestTimeout = Timespan.FromSeconds(file.Size / minimumSpeed);
	  timeoutCancellation.CancelAfter(requestTimeout);
      // Call SDK method and pass linkedCts.Token
      ...
     }

**N.B.** The __TrimbleConnectClient__ has property ___FileTransferTimeout___, which was previously used for this purpose. It has been deprecated and does not affect the file request timeout (in fact setting it will throw an exception). From **Trimble.Connect.Client** version **2.1.6.** onwards file transfers will never timeout. 

**N.B.** Thumbnails are specific type of files, and are assumed to be small in size. The API which SDK provides for thumbnail upload and download supports only the per-request cancellation token. Typically application would not bind the file upload/download action to user interaction and therefore it wouldn't need to link the user interaction token and a timeout token but would just create a timeout token with appropriate _CancelAfter_ value and use it in the method invocation. **If the application does not provide a cancellationToken, default token will be used and the request may never terminate.**

### Use timeout for a chunk of data transfer

Alternative approach would be to define a time limit for at least one byte of data to be transferred. This approach can work with arbitrarily large files and is the most flexible. To implement it, you would pass a ___CancellationToken___ to ___ReadAsync___ or ___WriteAsync___ methods on a ___Stream___ received from the SDK method. If you are using sync methods to operate on a stream, you can set stream's ___ReadTimeout___ or ___WriteTimeout___ instead. 

    var client = new TrimbleConnectClient...
    ...
    var timeoutCancellation = new CancellationTokenSource();
    using (var stream = await client.Files.DownloadAsync(fileVersionIdentifier))
    {
        var buffer = new byte[80 * 1024];
        var read = 0;
        do
        {
            timeoutCancellation.CancelAfter(TimeSpan.FromSeconds(5));
            read = await stream.ReadAsync(buffer, 0, buffer.Length, timeoutCancellation.Token);
            // put data where needed
        } while (read > 0);
    }

The **Trimble.Connect.Client** component provides convenience extension methods and overloads on the ___IFilesController___ for uploading from a ___Stream___ and downloading to a ___File___. These methods accept a ___timeout___ parameter which is used to limit the time spent waiting for at least one byte of data to be transferred.

    UploadAsync
    UploadFromFileAsync
    DownloadToFileAsync

Similarly, the **Trimble.Connect.Data** component has overload for file sync methods (_PushAsync_, _PullAsync_) on ___LocalFile___ and ___FileVersion___ which accept a ___timeout___ parameter.

