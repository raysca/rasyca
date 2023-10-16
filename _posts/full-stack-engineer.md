# Full Stack Engineering Prep Handbook

## JavaScript & NodeJS

### Event Loop

### Multi Threading in NodeJS

Have a cursory understanding of how the NodeJS event loop works. Each nodejs process has 7 threads, 4 threads for `I/O` , 2 threads for garbage collection and a main thread for running user code. 

```bash

```

References
 - [how-to-use-multithreading-in-node-js](https://www.digitalocean.com/community/tutorials/how-to-use-multithreading-in-node-js)
 - [understanding-the-event-loop-callbacks-promises-and-async-await-in-javascript](https://www.digitalocean.com/community/tutorials/understanding-the-event-loop-callbacks-promises-and-async-await-in-javascript)

 ## Web Security

 Authentication 


 ## Caching

Have a basic understanding of how caching works. Caching is a way to store data in a way that makes it faster to access. Caching is used to speed up access to data that is used frequently.

 ### E-Tags

 E-Tags are a way for the server to tell the client that the resource has changed. The server generates a hash of the resource and sends it to the client. The client then sends the hash back to the server on subsequent requests. If the hash matches, the server sends a `304` response code and the client uses the cached version of the resource. If the hash does not match, the server sends a `200` response code and the client uses the new version of the resource.

 ### HTTP Cache-Control Headers

HTTP cache headers are a way for the server to tell the client how to cache the resource. The server sends the cache headers with the resource. The client then uses the cache headers to determine how to cache the resource.

The `Cache-Control` header tells the client how long to cache the resource. The `max-age` directive tells the client how long to cache the resource in seconds. In the example above, the client will cache the resource for 1 hour.

```javascript
Bun.serve({
    fetch(req: Request) {
        return new Response('Hello World', {
            headers: {
                'Cache-Control': 'max-age=3600'
            }
        })
    },
})
```





## HTTP

### Methods

- GET - Used to get a resource
- POST - Used to create a resource
- PUT - Used to update a resource
- DELETE - Used to delete a resource
- HEAD - Used to get the headers of a resource
- OPTIONS - Used to get the options of a resource


### HTTP v1.1 vs HTTP 2

- HTTP 2 is a binary protocol. 
- HTTP 2 is faster than HTTP 1.1 because it uses binary encoding instead of text encoding. 
- HTTP 2 also supports multiplexing, which means that multiple requests can be sent over a single connection. 
- HTTP 2 also supports server push, which means that the server can send resources to the client before the client requests them.
    

### Web Sockets

Web Sockets are a way for the client and server to communicate over a single connection. It is perfect for real-time applications like chat or multiplayer games. Web Sockets are supported by all modern browsers and are easy to use.
    

### Brush up on HTTP Codes

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

```javascript
const https = require('https');



