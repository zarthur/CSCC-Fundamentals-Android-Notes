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

- **100-199** - Informational.  These statuses simply indicate that a request
  was received.
- **200-299** - Success.  These statuses indicate that the request was received
  and processed successfully.
- **300-399** - Redirection. These statuses indicate that a client must do
  something to complete the request; often these are used to indicate that a
  resource has moved and the client must make a new request with the new
  location.
- **400-499** - Client Error. These statuses indicate that the client has made
  an error. The most common example is a *404 Not Found* error which occurs
  when a client requests a resource that cannot be found on the server.
- **500-599** - Server Error. These statuses indicate that the server failed to
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

```java
package com.myname.week_15;


import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.URL;



public class Main {
    public static void main(String[] args) {
        try {
            URL url = new URL("http://www.example.com");
            HttpURLConnection connection = (HttpURLConnection) url.openConnection();
            InputStreamReader inputStreamReader
                    = new InputStreamReader(connection.getInputStream());
            BufferedReader input = new BufferedReader(inputStreamReader);

            System.out.println("RESPONSE STATUS: "
                    + connection.getResponseCode()
                    + " " + connection.getResponseMessage());

            String line;
            System.out.println("RESPONSE BODY");
            do {
                line = input.readLine();
                System.out.println(line);
            } while (line != null);
            System.out.println("END OF RESPONSE BODY");

        } catch (IOException e) {
            e.printStackTrace();
        }


    }
}
```

In this example, we begin by creating an instance of the URL class to store the
URL we'd like to retrieve data from.  Once we have a URL object, we can open a
connection to the URL using the *URL.openConnection()* method.  Once we open
the connection, we'd like to begin reading data.  In order to do this, we have
to make use of an input stream.  

An **input stream** is a data abstraction that
allows use to read data from a source. A stream is usually used in situations
where there is some flow of data - from a file to our program or from a web
server to a program.  Unlike an array, a stream has no concept of an index - we
can consume data but we can't move forward and backward as we please with a
stream.  A reader is a special type of input stream; while an input stream
can be used with character data or raw data in the form of bytes, a reader is
only used with character data.  Normally with input streams and readers, we
consume the data from the stream one byte or character at a time; a buffered
input stream or reader allow us to consume larger chunks of data at a time -
this usually improves the performance of our programs.  In our example, we
rely on a *BufferedReader* to work with character data in chunks.  

Notice that we've also cast the return value of *URL.openConnection()* to a
*HttpURLConnection* object.  The *HttpURLConnection* class gives us to specify
the HTTP request method (GET, POST, etc) and gives us access to the response
status code and message.  GET is used as the default request method.

To access the information in the response, we read data from the
*BufferedReader* one line at a time until there are no lines left to read.  

Many of these methods can throw checked exceptions that inherit from
*IOException*; in practice, our try block wouldn't contain all our code and
we'd handle each exception in the appropriate way.

### Interacting with a REST API
Interacting with REST APIs require us to do more than just make GET requests.
For the following examples, we'll make use of the RESTful Todo App accessible at
[http://todo.eastus.cloudapp.azure.com/todo-android](http://todo.eastus.cloudapp.azure.com/todo-android)
which is based on the code described above.  

Looking at the [documentation](https://github.com/zarthur/restful-todo/blob/master/README.md)
we'll need to make GET, POST, PUT, and DELETE requests.  While we could write
code the relies entirely on the *HttpURLConnection* class, it can be tedious.  
Instead, we will make use of the Apache HttpClient library.  To add the library
to our project, we can add

```
compile 'org.apache.httpcomponents:httpclient:4.5.2'
```

to the *dependencies* section of our project's *build.gradle* file.

Our code to make a GET request and display the response can be rewritten like
this:

```Java
package com.myname.week_15;


import org.apache.http.HttpResponse;
import org.apache.http.StatusLine;
import org.apache.http.client.methods.CloseableHttpResponse;
import org.apache.http.client.methods.HttpGet;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClientBuilder;
import org.apache.http.impl.client.HttpClients;
import org.apache.http.util.EntityUtils;

import java.io.IOException;

class HttpException extends IOException {
    public HttpException(HttpResponse response) {
        super(response.getStatusLine().getStatusCode() + ": "
                + response.getStatusLine().getReasonPhrase());
    }
}

class HttpRequests {
    private CloseableHttpClient client = HttpClientBuilder.create().build();

    // return true if status is between 200 and 300
    private static boolean isSuccess(HttpResponse response) throws IOException {
        StatusLine status = response.getStatusLine();
        return (status.getStatusCode() >= 200 && status.getStatusCode() < 300);
    }

    public String get(String url) throws IOException {
        HttpGet request = new HttpGet(url);
        CloseableHttpResponse response = client.execute(request);
        try {
            if (!isSuccess(response)) {
                throw new HttpException(response);
            }

            return EntityUtils.toString(response.getEntity());
        }
        finally {
            response.close();
        }
    }
}

public class Main {
    public static void main(String[] args) {
        HttpRequests requests = new HttpRequests();
        try {
            System.out.println(requests.get("http://www.example.com"));
        } catch (IOException e) {
            System.out.println("Unable to open URL");
        }
    }
}
```

Here, we create a new class to handle our HTTP requests that relies on a
*CloseableHttpClient* to make our HTTP requests.  We create the client using
the *HttpClientBuilder* class that allows us to create a client and specify
certain client configuration options if necessary. In the *get()* method,
we use create an instance of the *HttpGet* class using a string-based URL,
execute the request, and check the status of the response.  If the response
doesn't indicate success, we throw an exception.  If the response does indicate
success, we convert the response to a string using a static method on the
*EntityUtils* class. Comparing this code to the previous example, we see that
working with the Apache HttpClient library simplifies our code.  

The REST API we're using requires that users log in by providing a username
and password.  The Apache HttpClient library makes this relatively simple.  
The following code includes the additions necessary for authentication.  The
example logs in as a user and prints the result of requesting
`http://todo.eastus.cloudapp.azure.com/todo-android/todos/api/v1.0/todos` with
the username `test` and password `test`.

```Java
package com.myname.week_15;


import org.apache.http.HttpResponse;
import org.apache.http.StatusLine;
import org.apache.http.auth.AuthScope;
import org.apache.http.auth.UsernamePasswordCredentials;
import org.apache.http.client.CredentialsProvider;
import org.apache.http.client.methods.CloseableHttpResponse;
import org.apache.http.client.methods.HttpGet;
import org.apache.http.impl.client.BasicCredentialsProvider;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClientBuilder;
import org.apache.http.util.EntityUtils;

import java.io.IOException;

class HttpException extends IOException {
    public HttpException(HttpResponse response) {
        super(response.getStatusLine().getStatusCode() + ": "
                + response.getStatusLine().getReasonPhrase());
    }
}

class HttpRequests {
    private CloseableHttpClient client;

    public HttpRequests(String username, String password) {
        CredentialsProvider credentials = new BasicCredentialsProvider()
        credentials.setCredentials(AuthScope.ANY,
                new UsernamePasswordCredentials(username, password));
        client = HttpClientBuilder.create()
                .setDefaultCredentialsProvider(credentials).build();
    }

    // return true if status is between 200 and 300
    private static boolean isSuccess(HttpResponse response) throws IOException {
        StatusLine status = response.getStatusLine();
        return (status.getStatusCode() >= 200 && status.getStatusCode() < 300);
    }

    public String get(String url) throws IOException {
        HttpGet request = new HttpGet(url);
        CloseableHttpResponse response = client.execute(request);
        try {
            if (!isSuccess(response)) {
                throw new HttpException(response);
            }

            return EntityUtils.toString(response.getEntity());
        }
        finally {
            response.close();
        }
    }
}

public class Main {
    public static void main(String[] args) {
        HttpRequests requests = new HttpRequests("test", "test");
        try {
            System.out.println(
                    requests.get("http://todo.eastus.cloudapp.azure.com/todo-android/todos/api/v1.0/todos"));
        } catch (IOException e) {
            System.out.println("Unable to open URL");
        }
    }
}
```

The output might include something like this:

```
{
  "todos": [
    {
      "body": "mow grass",
      "done": false,
      "id": 0,
      "priority": 1,
      "title": "grass"
    }
  ]
}
```

Notice that we modified the *HttpRequests* class to to build the
*CloseableHttpClient* in the constructor using the username and password
credentials provided.  Subsequent requests made using the client will make use
of these credentials.  

To fully support the API, we'll need to support POST, PUT, and DELETE methods
in addition to the GET method we already support.  To effectively work with
the API, we'll also need to be able to serialize and deserialize JSON from/to
the appropriate JSON objects.  Lets' add methods to our *HttpRequests* class
to support the remaining HTTP request methods then work on implementing JSON
support.

```java
package com.myname.week_15;


import org.apache.http.HttpResponse;
import org.apache.http.StatusLine;
import org.apache.http.auth.AuthScope;
import org.apache.http.auth.UsernamePasswordCredentials;
import org.apache.http.client.CredentialsProvider;
import org.apache.http.client.methods.*;
import org.apache.http.entity.StringEntity;
import org.apache.http.impl.client.BasicCredentialsProvider;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClientBuilder;
import org.apache.http.util.EntityUtils;

import java.io.IOException;
import java.io.UnsupportedEncodingException;

class HttpException extends IOException {
    public HttpException(HttpResponse response) {
        super(response.getStatusLine().getStatusCode() + ": "
                + response.getStatusLine().getReasonPhrase());
    }
}

class HttpRequests {
    private CloseableHttpClient client = HttpClientBuilder.create().build();

    public HttpRequests (String username, String password){
        CredentialsProvider credentialsProvider = new BasicCredentialsProvider();
        credentialsProvider.setCredentials(AuthScope.ANY,
                new UsernamePasswordCredentials(username, password));
        client = HttpClientBuilder.create()
                .setDefaultCredentialsProvider(credentialsProvider).build();
    }

    // return true if status is between 200 and 300
    private static boolean isSuccess(HttpResponse response) throws IOException {
        StatusLine status = response.getStatusLine();
        return (status.getStatusCode() >= 200 && status.getStatusCode() < 300);
    }

    // send specified request and return response as a string
    private String makeRequest(HttpRequestBase request) throws IOException {
        CloseableHttpResponse response = client.execute(request);
        try {
            if (!isSuccess(response)) {
                throw new HttpException(response);
            }

            return EntityUtils.toString(response.getEntity());
        }
        finally {
            response.close();
        }
    }

    private void addData(HttpEntityEnclosingRequestBase request, String contentType, String data)
            throws UnsupportedEncodingException {
        request.setHeader("Content-type", contentType);

        StringEntity requestData = new StringEntity(data);
        request.setEntity(requestData);
    }


    // send a GET request to the specified URL
    public String get(String url) throws IOException {
        HttpGet request = new HttpGet(url);
        return makeRequest(request);

    }

    // send a DELETE request to the specified URL
    public String delete(String url) throws IOException {
        HttpDelete request = new HttpDelete(url);
        return makeRequest(request);
    }

    // send a POST request to the specified URL with the specified data
    // and the specified content-type
    public String post(String url, String contentType, String data) throws IOException{
        HttpPost request = new HttpPost(url);
        addData(request, contentType, data);
        return makeRequest(request);
    }

    // send a PUT request to the specified URL with the specified data
    // and the specified content-type
    public String put(String url, String contentType, String data) throws IOException{
        HttpPut request = new HttpPut(url);
        addData(request, contentType, data);
        return makeRequest(request);
    }
}

public class Main {
    public static void main(String[] args) {
        // these examples may not work in the future as the RESTful service may not be running
        HttpRequests requests = new HttpRequests("test", "test");
        String contentType = "application/json";
        String data = "{\"title\":\"dinner\", \"body\":\"prepare dinner\", \"priority\": 3}";

        // post example
        String url = "http://todo.eastus.cloudapp.azure.com/todo-android/todos/api/v1.0/todo/create";
        System.out.println("POST Example - add a todo");
        try {
            String response = requests.post(url, contentType, data);
            System.out.println("POST RESPONSE: " + response);
        }
        catch (IOException e) {
            e.printStackTrace();
        }

        // put example
        data = "{\"title\":\"dinner\", \"body\":\"eat dinner\", \"priority\": 1}";
        url = "http://todo.eastus.cloudapp.azure.com/todo-android/todos/api/v1.0/todo/update/0";
        System.out.println("PUT Example - update a todo");
        try {
            String response = requests.put(url, contentType, data);
            System.out.println("PUT RESPONSE: " + response);
        }
        catch (IOException e) {
            e.printStackTrace();
        }

        // get example
        url = "http://todo.eastus.cloudapp.azure.com/todo-android/todos/api/v1.0/todos";
        System.out.println("GET Example - list all todos");
        try {
            String response = requests.get(url);
            System.out.println("GET RESPONSE: " + response);
        }
        catch (IOException e) {
            e.printStackTrace();
        }

        // delete example
        url = "http://todo.eastus.cloudapp.azure.com/todo-android/todos/api/v1.0/todo/delete/0";
        System.out.println("Delete Example - delete a todo");
        try {
            String response = requests.delete(url);
            System.out.println("DELETE RESPONSE: " + response);
        }
        catch (IOException e) {
            e.printStackTrace();
        }

    }
}
```

In this code, we've created a *HttpRequests* class to handle *GET*, *POST*,
*PUT*, and *DELETE* requests.  Compare the *HttpRequests.get()* method to the
code we wrote previously.  Notice that we were able to reuse a large portion of
the original code to support the other request methods and place it in the
*HttpRequests.makeRequest()* method.  Similarly, the *POST* and *PUT* methods
rely on the same *HttpRequests.addData()* method to attach data to the request.

In *Main.main()* we demonstrate the functionality of the class using the Todo
REST API.  A next step would be to incorporate the *HttpRequests* class with
code we've written before to work with todo objects and their JSON
representations.

```java
package com.myname.week_15;


import com.google.gson.Gson;
import org.apache.http.HttpResponse;
import org.apache.http.StatusLine;
import org.apache.http.auth.AuthScope;
import org.apache.http.auth.UsernamePasswordCredentials;
import org.apache.http.client.CredentialsProvider;
import org.apache.http.client.methods.*;
import org.apache.http.entity.StringEntity;
import org.apache.http.impl.client.BasicCredentialsProvider;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClientBuilder;
import org.apache.http.util.EntityUtils;

import java.io.IOException;
import java.io.UnsupportedEncodingException;
import java.util.Iterator;
import java.util.List;

class HttpException extends IOException {
    public HttpException(HttpResponse response) {
        super(response.getStatusLine().getStatusCode() + ": "
                + response.getStatusLine().getReasonPhrase());
    }
}

class HttpRequests {
    private CloseableHttpClient client = HttpClientBuilder.create().build();

    public HttpRequests (String username, String password){
        CredentialsProvider credentialsProvider = new BasicCredentialsProvider();
        credentialsProvider.setCredentials(AuthScope.ANY,
                new UsernamePasswordCredentials(username, password));
        client = HttpClientBuilder.create()
                .setDefaultCredentialsProvider(credentialsProvider).build();
    }

    // return true if status is between 200 and 300
    private static boolean isSuccess(HttpResponse response) throws IOException {
        StatusLine status = response.getStatusLine();
        return (status.getStatusCode() >= 200 && status.getStatusCode() < 300);
    }

    // send specified request and return response as a string
    private String makeRequest(HttpRequestBase request) throws IOException {
        CloseableHttpResponse response = client.execute(request);
        try {
            if (!isSuccess(response)) {
                throw new HttpException(response);
            }

            return EntityUtils.toString(response.getEntity());
        }
        finally {
            response.close();
        }
    }

    private void addData(HttpEntityEnclosingRequestBase request, String contentType, String data)
            throws UnsupportedEncodingException {
        request.setHeader("Content-type", contentType);

        StringEntity requestData = new StringEntity(data);
        request.setEntity(requestData);
    }


    // send a GET request to the specified URL
    public String get(String url) throws IOException {
        HttpGet request = new HttpGet(url);
        return makeRequest(request);

    }

    // send a DELETE request to the specified URL
    public String delete(String url) throws IOException {
        HttpDelete request = new HttpDelete(url);
        return makeRequest(request);
    }

    // send a POST request to the specified URL with the specified data
    // and the specified content-type
    public String post(String url, String contentType, String data) throws IOException{
        HttpPost request = new HttpPost(url);
        addData(request, contentType, data);
        return makeRequest(request);
    }

    // send a PUT request to the specified URL with the specified data
    // and the specified content-type
    public String put(String url, String contentType, String data) throws IOException{
        HttpPut request = new HttpPut(url);
        addData(request, contentType, data);
        return makeRequest(request);
    }
}

// a To-do class with a basic constructor, getters, and setters
class Todo {
    private String title;
    private String body;
    private int priority;
    private Integer id = null; //only set an int when the server assigns an ID

    public Todo(String title, String body, int priority) {
        this.title = title;
        this.body = body;
        this.priority = priority;
    }

    public String getTitle() {
        return title;
    }

    public void setTitle(String title) {
        this.title = title;
    }

    public String getBody() {
        return body;
    }

    public void setBody(String body) {
        this.body = body;
    }

    public int getPriority() {
        return priority;
    }

    public void setPriority(int priority) {
        this.priority = priority;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    @Override
    public String toString() {
        return "TODO ID: " + id + ", Title: " + title + ", Body: " + body
                + ", Priority: " + priority;
    }
}

// a collection of Todos; only support iteration
class TodoCollection implements Iterable<Todo> {
    private List<Todo> todos;


    @Override
    public Iterator<Todo> iterator() {
        return todos.iterator();
    }
}

class TodoAPIWrapper {
    Gson gson = new Gson();
    private HttpRequests requests;
    private String hostURL;

    TodoAPIWrapper(String username, String password, String hostURL) {
        requests = new HttpRequests(username, password);
        this.hostURL = hostURL;
    }

    // get all the todos
    public TodoCollection getTodos() {
        String url = hostURL + "/todos/api/v1.0/todos";
        try {
            String response = requests.get(url);
            return gson.fromJson(response, TodoCollection.class);
        } catch (IOException e) {
            System.out.println("Unable to get todos");
        }
        return null;
    }

    // create a new todo
    public Todo createTodo(String title, String body, int priority) {
        Todo newTodo = new Todo(title, body, priority);
        String url = hostURL + "/todos/api/v1.0/todo/create";
        String contentType  = "application/json";
        String postData = gson.toJson(newTodo);
        try {
            requests.post(url, contentType, postData);
            return newTodo;
        } catch (IOException e) {
            System.out.println("Unable to create new task: " + title);
        }
        return null;
    }

    // get a todo by id
    public Todo getTodo(int id) {
        String url = hostURL + "/todos/api/v1.0/todo/" + id;
        try {
            String response = requests.get(url);
            return gson.fromJson(response, Todo.class);
        } catch (IOException e) {
            System.out.println("Unable to get todo with id" + id);
        }
        return null;
    }

    // get first todo that matches title
    public Todo getTodo(String title) {
        TodoCollection todos = getTodos();
        for (Todo todo: todos) {
            if (todo.getTitle().equals(title)) {
                return todo;
            }
        }
        return null;
    }

    // delete a todo by id
    public boolean removeTodo(int id) {
        String url = hostURL + "/todos/api/v1.0/todo/delete/" + id;
        try {
            requests.delete(url);
            return true;
        }
        catch (IOException e) {
            System.out.println("Unable to delete todo with ID " + id);
        }
        return false;
    }

}

public class Main {
    public static void main(String[] args) {
        TodoAPIWrapper todoAPI = new TodoAPIWrapper("test", "test",
                "http://todo.eastus.cloudapp.azure.com/todo-android");

        // create two todos
        System.out.println("Adding todos");
        todoAPI.createTodo("Study", "Study for exam", 2);
        todoAPI.createTodo("Dinner", "Prepare dinner", 3);

        // get todos and list them
        System.out.println("Getting todos");
        TodoCollection todos = todoAPI.getTodos();
        for (Todo todo: todos) {
            System.out.println(todo);
        }

        // remove todo with id 0
        System.out.println("Removing todo");
        todoAPI.removeTodo(0);

        //get remaining todos and list them
        System.out.println("Getting remaining todos");
        todos = todoAPI.getTodos();
        for (Todo todo: todos) {
            System.out.println(todo);
        }

    }
}
```

In the preceding code, we've added a class representing each todo item, a
class to represent a collection of todo items, and a class that includes
methods for creating, removing, and getting todos using the *HttpRequests*
class to communicate with the server.  

## Exercise
Add an *update()* method that takes a *Todo* instance as a parameter.  The
method should check if the *Todo* instance has an ID.  If there is an ID, the
method should use the *HttpRequests.put()* method and the
`/todos/api/v1.0/todo/update/<ID>` endpoint to send the update to the server.  

Once the method is written, demonstrate it's functionality but creating a new
todo item, getting it from the server using it's title, update the *Todo*
object in your code, then send it to the server using the newly written method.
