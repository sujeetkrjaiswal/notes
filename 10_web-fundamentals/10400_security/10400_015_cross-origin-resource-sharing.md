# Cross Origin Resource Sharing

## Steps Involved

1. When the browser is making a cross-origin request, the browser adds an `Origin` header with the current origin \(scheme, host, and port\).
2. On the server side, when a server sees this header, and wants to allow access, it needs to add an `Access-Control-Allow-Origin` header to the response specifying the requesting origin \(or \* to allow any origin.\)
3. When the browser sees this response with an appropriate `Access-Control-Allow-Origin` header, the browser allows the response data to be shared with the client site.

For privacy and security reasons, CORS is normally used for anonymous requests. If you want to send cookies, set additional headers to request and response.

On Browser, use below

```javascript
fetch('https://example.com', {
  mode: 'cors',
  credentials: 'include'
})
```

On Server, it should add below headers. `Access-Control-Allow-Origin` must be set to a specific origin \(no wildcard using `*`\)

```yaml
HTTP/1.1 200 OK
Access-Control-Allow-Origin: https://example.com
Access-Control-Allow-Credentials: true
```

## Preflight Requests for complex HTTP calls

If a web app needs a complex HTTP request, the browser adds a preflight request to the front of the request chain. It's an `OPTIONS` request like below and is sent before the actual request message.

```yaml
OPTIONS /data HTTP/1.1
Origin: https://example.com
Access-Control-Request-Method: DELETE
```

The server should respond with methods the applications accepts from this origin. The server response can also include an `Access-Control-Max-Age` header to specify the duration to cache preflight results so the client does not need to make a preflight request every time it sends a complex request.

```yaml
HTTP/1.1 200 OK
Access-Control-Allow-Origin: https://example.com
Access-Control-Allow-Methods: GET, DELETE, HEAD, OPTIONS
```

The CORS specification defines a complex request as

* A request that uses methods other than `GET`, `POST`, or `HEAD`
* A request that includes headers other than `Accept`, `Accept-Language` or `Content-Language`
* A request that has a Content-Type header other than `application/x-www-form-urlencoded`, `multipart/form-data`, or `text/plain`

## Exmaple

On server side, use the below sample code

```javascript
const express = require('express');
const app = express();

// No CORS header set
app.get('/', function(request, response) {
  response.sendFile(__dirname + '/message.json');
});

// CORS header `Access-Control-Allow-Origin` set to accept all
app.get('/allow-cors', function(request, response) {
  response.set('Access-Control-Allow-Origin', '*');
  response.sendFile(__dirname + '/message.json');
});

// Listen for requests :)
const listener = app.listen(process.env.PORT, function() {
  console.log('Your app is listening on port ' + listener.address().port);
});
```

On the browser end

```javascript
fetch('https://example.com/', {mode:'cors'})
// request has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header
//is present on the requested resource.


fetch('https://example.com/allow-cors', {mode:'cors'})
```

## References

* Blog on [web.dev](https://web.dev) titled [Share cross-origin resources safely](https://web.dev/cross-origin-resource-sharing/)

