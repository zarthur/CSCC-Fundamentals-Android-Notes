# Week 15 - HTTP and Networking

## Corresponding Text
*Learn Java for Android Development*, pp. 567-573, 621-632, 639-643, 665-670

## HTTP and REST
**Hypertext transfer protocol (HTTP)** is a means of transferring data
between a client and a server in based on requests and responses.  A client
submits a request to a server and the returns a response - usually with some
data related to the request including status information about the completion
of the request. Any time you load a web site in a browser, the data is
retrieved from the server by the browser using HTTP.

### Requests and Responses
The HTTP standard defines several different types of requests that a client can
make.  Each of the different types of request methods, also called HTTP verbs,
correspond to a different action that can be done to some resource on a server.
While we typically use HTTP to retrieve data from a server in the form of a web
page or submit data to a server using forms, we can also update data or delete
resources from a server using HTTP assuming we have the proper permissions.  

For the examples we'll be looking at, we'll work with four of the HTTP request
methods.

1. **GET** - requests a representation of the specified resource. GET should
   only retrieve data and should not alter any data on the server.
2. **POST** - requests that the server accept and store data contained in the
   request.  POST is often used for submitting form data or uploading files.
3. **PUT** - requests that the server accept data and store it with a specific
   resource.  PUT is often used to update an existing server-side resource.
4. **DELETE** - requests that the specified resource be removed from the server.

We can use these request methods to create, read, update, and delete data on a
server using a client.  We'll see examples of this later.

For each request made, the server should generate a response and send it to
the client.  The data included in the response is dependent on the request.  
For example, if the client makes a GET request for a web page, the server's
response might include HTML; if the client makes a DELETE request, the server's
response might simply be an indication whether the delete was successful or not.

In addition to data associated with the request, the server's response includes
a status code and text associated with the status code.  HTTP status codes are
typically three-digit codes grouped by meaning.

- 100-199 - Informational.  These statuses simply indicate that a request was
  received.
- 200-299 - Success.  These statuses indicate that the request was received and
  processed successfully.
- 300-399 - Redirection. These statuses indicate that a client must do
  something to complete the request; often these are used to indicate that a
  resource has moved and the client must make a new request with the new
  location.
- 400-499 - Client Error. These statuses indicate that the client has made an
  error. The most common example is a *404 Not Found* error which occurs when a
  client requests a resource that cannot be found on the server.
- 500-599 - Server Error. These statuses indicate that the server failed to
  fulfill an apparently valid request.

When we interact with a server using HTTP, it is important to rely on the
response status codes to determine the outcome of our requests.

### REST APIs
An **application programming interface (API)** is a means of interacting with
an application.  An API specifies which resources and protocols are
publicly-accessible and that can be used to interact with the application.  An
API can be in the form of a library than provides language-specific objects
such as classes, methods, and variables.  An API can also be in the form of a
web API, making resources accessible via a web server and often relying on
HTTP to facilitate communication.

**Representational state transfer (REST)** describes an software architectural
style.  Some of the properties of an *RESTful* application are the following:

1. Clear separation between client and server roles,
2. No information about client context is stored on the server; each request
   should contain all the information necessary to complete the request, and
3. The application presents a uniform interface in which individual resources
   can be identified in a request, the resources are able to be updated or
   deleted using the interface and information provided by it, and messages
   used with the interface include information describing how to process it.

When we begin working with HTTP in Java later in this class, we will make use
of a simple RESTful Todo application.  The source code can be found at
[https://github.com/zarthur/restful-todo](https://github.com/zarthur/restful-todo)
and is a modification of the code found at
[https://github.com/vahidR/restful-todo](https://github.com/vahidR/restful-todo);
it has been modified to better serve our needs.

## HTTP in Java
###The HttpURLConnection Class

### Interacting with a REST API


## Exercise
