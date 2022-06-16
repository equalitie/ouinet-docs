# Client Library

The Ouinet client library is a software library that implements the Ouinet client, as described in this document. The Ouinet client library is structured as an HTTP proxy server that application software can connect to, and which can perform HTTP requests sent to it by the application software using the mechanisms of the Ouinet project. Applications using the Ouinet client library can invoke a library operation to start this proxy server in the background, and then configure usage of this proxy server for HTTP requests performed by the application.

When the Ouinet client library gets initialized, it will attempt to establish a connection to one of the configured injector servers, using any of the mechanisms described in the [Injector server connections](#injector-server-connections) section. Depending on the configuration, it may also begin participating in the Ouinet distributed cache, operating as a bridge node, or both.

Once the Ouinet client library proxy server is operational, it will start accepting HTTP proxy requests. The client will attempt to satisfy these requests using any of the mechanisms specified in the [Content Access Systems](#content-access-systems) section, and reply with a response when it has succeeded.

When an application functioning as an HTTP client is configured to use an HTTP proxy server, it will generally not send HTTPS requests to this proxy server, to avoid a breach of confidentiality. Instead, such applications will usually send a CONNECT request to the proxy server instead, establishing a tunneled connection to the origin server that the proxy server cannot eavesdrop on or manipulate. While this is generally a sensible decision when applied to most proxy servers, this behavior would make it impossible for the Ouinet client to perform any of the Ouinet functionality to HTTPS requests.

In order to be able to apply the Ouinet mechanisms to HTTPS requests despite this complication, the Ouinet client proxy terminates all TLS sessions that would otherwise be tunneled through the client proxy. This functionality makes the Ouinet client library behave as a type of man-in-the-middle, which applications will typically rightly reject as a security breach. If an application wishes to use the Ouinet library to perform HTTPS requests, it must therefore configure the TLS certificate used by the Ouinet client proxy as being authorized for this purpose. The Ouinet client library generates such a certificate, private to the specific device running the Ouinet client, when starting the client proxy; this certificate functions as a certificate authority for connections made to the Ouinet client proxy, and should be configured as such in the application.

When an end-user application sends an HTTP request to the Ouinet client proxy, it can include certain configuration options to adjust the behavior of the Ouinet client; in particular, it can include instructions as to how the Ouinet client should and should not attempt to resolve individual HTTP requests. These configuration settings are included in the form of additional HTTP headers specifically to the Ouinet project. Likewise, the Ouinet client can communicate additional details regarding resources it has fetched for the benefit of the application; these details, too, are communicated in the form of additional HTTP headers included with the HTTP response. The exact list of such headers used in the communications between the end-user application and the Ouinet client proxy are described below.


## Request headers

The application sending HTTP requests to the client library can include the following HTTP headers to customize the Ouinet client behavior:

* `X-Ouinet-Private`: If equal to `true`, indicates that this request is ineligible for caching. The Ouinet client may not use the distributed cache lookup mechanism, nor attempt to create a cache entry based on this request.
* `X-Ouinet-Group`: If set, specifies the resource group to which the requested resource belongs. The Ouinet client should search the distributed cache using a distributed hash table location based on this group, both when attempting to fetch the resource from the distributed cache, and when sharing a cache entry for this resource using the distributed cache.

## Response headers

When sending a response to an HTTP request, the Ouinet client may append the following HTTP headers to the response:

* `X-Ouinet-Version`: Contains the version of the Ouinet protocol used by the Ouinet client.
* `X-Ouinet-Error`: Added by the Ouinet client to any responses indicating that the Ouinet client could not resolve the assorted request. If set, the value of this header consists of a numeric error code, followed by an error description in plain text.
* `X-Ouinet-Warning`: Added by the Ouinet client when a resource request has not failed, but did encounter a situation that the user should be made aware of. If set, the value of this header is a description in plain text.
* `X-Ouinet-Source`: Contains the content access mechanism used to satisfy the assorted request. Possible values include:
  * `front-end`: The request was not a proxy request, but rather an HTTP request to the Ouinet client control panel. The Ouinet client control panel is not described further in this document.
  * `origin`: The request was satisfied using a direct HTTP request to the responsible origin server.
  * `proxy`: The request was satisfied using a request to the injector server that is not eligible for caching.
  * `injector`: The request was satisfied using a request to the injector server that is eligible for caching, and that may have been stored in the distributed cache.
  * `local-cache`: The request was satisfied using a cache entry stored in device local storage.
  * `dist-cache`: The request was satisfied by retrieving a cache entry using the distributed cache.
* `X-Ouinet-Injection`: Added by the Ouinet client to any response that was satisfied using the distributed cache, either by retrieving a cache entry from the local storage or a peer-to-peer connection, or by requesting a cache entry created by an injector server. If set, this header contains the unique ID representing the cache entry containing the requested resource.
