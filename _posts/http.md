# What eveny full stack engineers should know about HTTP

As a full stack engineer, you will be dealing a lot with the ubiquitous HTTP protocol.  In this article I will cover various aspects of HTTP that you should know about. By the end of this article, you should have a good understanding of HTTP. This article will server as a reference for you to come back to when you need to brush up on HTTP. I hope you find this article useful and learn something new about HTTP.

If I missed anything, please let me know in the comments below. Happy reading!

## URL

URLs are addresses that are used to access resources on the internet and an important part of using HTTP. They are made up of a protocol, domain, path, query, fragment, port, username, password, and TLD.

These are the components of a URL:

| Name     | Description                                                                                                                   | Example             |
| -------- | ----------------------------------------------------------------------------------------------------------------------------- | ------------------- |
| Protocol | The protocol that will be used to access the resource. Most browsers now defaults to https unless explicitly stated otherwise | `https`             |
| Domain   | The domain that the resource is accessed from.                                                                                | `example.com`       |
| Path     | The path where the resource resides                                                                                           | `/path/to/resource` |
| Query    | Queries are extra information sent to the server to customize the request                                                     | `?query=string`     |
| Fragment | Fragments are used by browsers/clients to track progress in the document. It is never sent to the server as part of a request | `#fragment`         |
| Port     | The port where the server is running. It defaults to `80`  for `http` and `443` for `https`                                   | `:8080`             |
| Username | The username indicates to the server the identity of that who is accessing the resource. It defaults to `annoymous`           | `username`          |
| Password | Some resources are protected and the server can demand credentials. It defaults to empty                                      | `password`          |
| TLD      | The top-level domain is used by clients to know where to find the domain name in the URL                                      | `com`               |

Here is an example of a URL:

```text
https://username:password@example.com/path/to/resource?query=string#fragment
```

## HTTP Request Methods

HTTP requests are made up of a method, path, headers, and body. The method is the most important part of the request because it tells the server what to do with the request. There are 6 methods that are commonly used: `GET`, `POST`, `PUT`, `DELETE`, `HEAD`, and `OPTIONS`.  There is a seventh method called `TRACE` that is rarely used. but will be covered here for completeness.

A typical HTTP request looks like this:

```text
GET /path/to/resource HTTP/1.1
Host: example.com
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.15; rv:89.0) Gecko/20100101 Firefox/89.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Connection: keep-alive
```

[Request headers](##headers) will be discussed in more detail later in this article.

### Safe Methods

The `GET` and `HEAD` HTTP methods are idempotent, which means that they can be called multiple times without changing the state of the server.  They are often referred to as safe methods because they do not change the state of the server.

### Unsafe Methods

The `POST`, `PUT`, `DELETE`, and `OPTIONS` HTTP methods are not idempotent. they can potentially change the state of the server.


### GET

This is the most commonly used method  of the HTTP. It is the method used by browsers when you type in a URL in the address bar. It instructs the server to send the resource to the client. 

```text
GET /items/snickers HTTP/1.1 
Host: www.foo-bar.com
Accept: *
```

### POST

POST were meant to be used for sending input data to the server but in practice are commonly used by HTML forms. If following a strict interpretation of the HTTP specification, POST should be used for sending input data to the server. 

If you are designing a REST API, you should use POST for creating a new resource.

```text
POST /items HTTP/1.1
Host: www.foo-bar.com
Accept: *
Content-Type: application/json

{
    "name": "snickers",
    "price": 1.99
}
```

### PUT

PUT is used to update an existing resource on the server. If the resource does not exist, it will be created. If you are designing a REST API, you should use PUT for updating an existing resource.

```text

PUT /items/snickers HTTP/1.1
Host: www.foo-bar.com
Accept: *
Content-Type: application/json

{
    "name": "snickers",
    "price": 3.00
}
```

### DELETE

`DELETE` is used to delete an existing resource on the server. If you are designing a REST API, you should use `DELETE` for deleting an existing resource.

```text
DELETE /items/snickers HTTP/1.1
Host: www.foo-bar.com
Accept: *
``` 

### HEAD

The `HEAD` method can be referred to as lite `GET`. It behaves like a `GET` request but does not return the body of the response. It is used to get the headers of a resource on the server.

This is useful for checking if a resource exists without having to download the entire resource. Using `HEAD` can save bandwidth and time. Here are some use cases for `HEAD`:

- Checking if a resource exists by inspecting the status code of the response.
- Checking if a resource has been modified  e.g by inspecting the last modified headers.
- Inspect the type of a resouce by inspecting the content type headers.


```text
HEAD /items/snickers HTTP/1.1
Host: www.foo-bar.com
Accept: *
``` 

### OPTIONS

The `OPTIONS` method is used to query an HTTP server about what methods are allowed on a resource. The most common use case of this method is when browsers use this method when making a CORS request to check if the server allows the request. 

When a web page attempts to make a `CORS` request, the browser will first make an `OPTIONS` request to the server to check if the server allows the request. If the server allows the request, the browser will then make the actual request. If the server does not allow the request, the browser will not make the actual request.

HTTP Request
```text
OPTIONS /items/snickers HTTP/1.1
Host: www.foo-bar.com
Access-Control-Request-Method: DELETE
```

HTTP Response
```text
HTTP/1.1 200 OK
Access-Control-Allow-Methods: GET, POST, PUT, DELETE, HEAD, OPTIONS
```

### TRACE

The `TRACE` method is used to check if a request has been modified by a proxy server. It is rarely used in practice. It is good for debugging purposes. Lets look at an example of a request going through the internet. The figure below what most be happening when you make a request to a server.

![HTTP Request](/assets/http/request_no_proxy.png)

but in reality, the requests usually go through multiple proxies before reaching their destination and each proxy can potentially modify the request. The figure below shows what happens when a request goes through a proxy.

![HTTP Request](/assets/http/request_with_proxy.png)

This is where the `TRACE` method comes in handy. When web servers receive a `TRACE` request, they effectively just echo the request back to the client.

```text
TRACE /items/snickers HTTP/1.1
Host: www.foo-bar.com
Accept: *
```

```text
HTTP/1.1 200 OK 
Content-type: text/plain 
Content-length: 96
Via: 1.1 proxy1.server.com

TRACE /items/snickers HTTP/2 
Host: www.foo-bar.com 
Accept: *
Via: 1.1 proxy2.server.com
```

You can see that the request has been modified by the proxy server by upgrading the HTTP version from `HTTP/1.1` to `HTTP/2` and adding a `Via` header. This is useful for debugging purposes.


## HTTP Version 2

HTTP 2 is the latest version of the HTTP protocol. It is faster than HTTP 1.1 because it uses binary encoding instead of text encoding. It also supports multiplexing, which means that multiple requests can be sent over a single connection. It also supports server push, which means that the server can send resources to the client before the client requests them.

- HTTP 2 is a binary protocol. This means that it uses binary encoding instead of text encoding. This makes it faster than HTTP 1.1.
- HTTP 2 also supports multiplexing, which means that multiple requests can be sent over a single connection. This is super useful for reducing latency.
- HTTP 2 also supports server push, which means that the server can send resources to the client before the client requests them.


### Multiplexing

Multiplexing is a way for the server to send multiple requests over a single connection. This be

```javascript

fetch('https://foo-bar.com/items/snickers')
    .then(response => response.json())
    .then(data => console.log(data))
    .catch(error => console.error(error))

```

    

## Web Sockets

Web Sockets are a way for the client and server to communicate over a single connection. It is perfect for real-time applications like chat or multiplayer games. Web Sockets are supported by all modern browsers and are easy to use.
    

## Brush up on HTTP Codes

- 1xx - Informational
    - 100 - Continue
    - 101 - Switching Protocols
    - 102 - Processing

- 2xx - Success
    - 200 - OK
    - 201 - Created
    - 202 - Accepted
    - 203 - Non-Authoritative Information
    - 204 - No Content
    - 205 - Reset Content
    - 206 - Partial Content
    - 207 - Multi-Status
    - 208 - Already Reported
    - 226 - IM Used

- 3xx - Redirection
    - 301 - Moved Permanently
    - 302 - Found

- 4xx - Client Error
    - 400 - Bad Request
    - 401 - Unauthorized
    - 402 - Payment Required
    - 403 - Forbidden
    - 404 - Not Found
    - 405 - Method Not Allowed
    - 406 - Not Acceptable
    - 407 - Proxy Authentication Required
    - 408 - Request Timeout

- 5xx - Server Error
    - 500 - Internal Server Error
    - 501 - Not Implemented
    - 502 - Bad Gateway
    - 503 - Service Unavailable
    - 504 - Gateway Timeout
    - 505 - HTTP Version Not Supported
    - 506 - Variant Also Negotiates
    - 507 - Insufficient Storage
    - 508 - Loop Detected
    - 510 - Not Extended
    - 511 - Network Authentication Required
    - 599 - Network Connect Timeout Error
    
### CORS

Cross-Origin Resource Sharing (CORS) is a mechanism that allows resources on a web page to be requested from another domain outside the domain from which the resource originated. This is useful for loading resources from a CDN or loading resources from a different domain.

#### How CORS works

CORS works by adding an `Origin` header to the request. The `Origin` header contains the domain from which the request originated. The server then checks the `Origin` header and determines whether or not to allow the request.

#### CORS Preflight

CORS Preflight is a mechanism that allows the browser to check if the server allows the request before sending the actual request. This is useful for checking if the server allows the request before sending the actual request.

#### CORS Headers

- `Access-Control-Allow-Origin` - The domain from which the request originated.
- `Access-Control-Allow-Methods` - The methods that the server allows.
- `Access-Control-Allow-Headers` - The headers that the server allows.
- `Access-Control-Max-Age` - The amount of time that the server allows the request to be cached.
- `Access-Control-Allow-Credentials` - Whether or not the server allows credentials to be sent with the request.
- `Access-Control-Expose-Headers` - The headers that the server allows to be exposed to the client.

#### CORS Credentials

CORS Credentials are a way for the server to tell the client that it allows credentials to be sent with the request. This is useful for sending cookies with the request.


### Authentication

Authentication is a way for the server to tell the client that it is allowed to access a resource. This is useful for restricting access to resources that are only available to certain users.

#### Basic Authentication

Basic Authentication is a way for the server to tell the client that it is allowed to access a resource. This is useful for restricting access to resources that are only available to certain users.

#### Token Authentication

Token Authentication is a way for the server to tell the client that it is allowed to access a resource. This is useful for restricting access to resources that are only available to certain users.

#### OAuth

OAuth is a way for the server to tell the client that it is allowed to access a resource. This is useful for restricting access to resources that are only available to certain users.

#### JWT

JWT is a way for the server to tell the client that it is allowed to access a resource. This is useful for restricting access to resources that are only available to certain users.


### Cookies

Cookies are a way for the server to tell the client that it is allowed to access a resource. This is useful for restricting access to resources that are only available to certain users.

#### How Cookies Work

Cookies work by storing data on the client's computer. The server then sends the cookie to the client. The client then sends the cookie back to the server on subsequent requests.

#### Cookie Attributes

- `Domain` - The domain that the cookie is valid for.
- `Path` - The path that the cookie is valid for.
- `Expires` - The date that the cookie expires.
- `Max-Age` - The amount of time that the cookie is valid for.
- `Secure` - Whether or not the cookie is only sent over HTTPS.
- `HttpOnly` - Whether or not the cookie is only sent over HTTP.
- `SameSite` - Whether or not the cookie is only sent to the same site.

#### Cookie Security

Cookies are not secure by default. They can be stolen by a malicious user. To make cookies secure, they must be encrypted. This is done by using the `Secure` attribute.

## Security

### SQL Injection

SQL Injection is a way for the server to tell the client that it is allowed to access a resource. This is useful for restricting access to resources that are only available to certain users.

#### How SQL Injection Works

SQL Injection works by injecting SQL code into a query. This is done by using the `;` character. For example, the following query is vulnerable to SQL Injection:

```sql
SELECT * FROM users WHERE username = 'admin' AND password = 'password';
```

The following query is not vulnerable to SQL Injection:

```sql
SELECT * FROM users WHERE username = 'admin' AND password = 'password';
```

#### SQL Injection Prevention

SQL Injection can be prevented by using prepared statements. Prepared statements are a way to tell the database what data to expect. This is done by using the `?` character. For example, the following query is vulnerable to SQL Injection:

```sql
SELECT * FROM users WHERE username = ? AND password = ?;
```


### XSS

XSS is a way for the server to tell the client that it is allowed to access a resource. This is useful for restricting access to resources that are only available to certain users.

#### How XSS Works

XSS works by injecting JavaScript code into a web page. This is done by using the `<script>` tag. For example, the following code is vulnerable to XSS:

```html
<script>alert('XSS')</script>
```

The following code is not vulnerable to XSS:

```html
<script>alert('XSS')</script>
```

#### XSS Prevention

XSS can be prevented by using a Content Security Policy (CSP). A CSP is a way to tell the browser what JavaScript code to allow. This is done by using the `script-src` directive. For example, the following code is vulnerable to XSS:


### CSRF

CSRF is a way for the server to tell the client that it is allowed to access a resource. This is useful for restricting access to resources that are only available to certain users.

#### How CSRF Works

CSRF works by tricking the user into clicking a link or submitting a form. This is done by using the `GET` or `POST` method. For example, the following code is vulnerable to CSRF:

```html
<form action="https://example.com" method="POST">
    <input type="hidden" name="csrf_token" value="1234567890">
    <input type="submit" value="Submit">
</form>
```

The following code is not vulnerable to CSRF:

```html
<form action="https://example.com" method="POST">
    <input type="hidden" name="csrf_token" value="1234567890">
    <input type="submit" value="Submit">
</form>
```

### HTTPS

HTTPS is a way for the server to tell the client that it is allowed to access a resource. This is useful for restricting access to resources that are only available to certain users.

#### How HTTPS Works

HTTPS works by encrypting the data that is sent between the client and the server. This is done by using the `TLS` protocol. For example, the following code is vulnerable to HTTPS:
  

## Proxy Servers

Proxy Servers are a way for the client to tell the server that it is allowed to access a resource. This is useful for restricting access to resources that are only available to certain users.

### Reverse Proxy

Reverse Proxy is a way for the server to tell the client that it is allowed to access a resource. This is useful for restricting access to resources that are only available to certain users.

### Forward Proxy

Forward Proxy is a way for the client to tell the server that it is allowed to access a resource. This is useful for restricting access to resources that are only available to certain users.

### Load Balancer

Load Balancer is a way for the server to tell the client that it is allowed to access a resource. This is useful for restricting access to resources that are only available to certain users.

### Caching Server

Caching Server is a way for the client to tell the server that it is allowed to access a resource. This is useful for restricting access to resources that are only available to certain users.

### Web Application Firewall

Web Application Firewall is a way for the server to tell the client that it is allowed to access a resource. This is useful for restricting access to resources that are only available to certain users.

cccccbgvrirtfvbnrvvefivijrvfghkbfbdgujeeeffi
