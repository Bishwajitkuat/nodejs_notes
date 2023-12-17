# Server with HTTP Basic

## Core Modules

Modules comes with nodejs installation, we need to just require it to use it. some core modules are

- http
- https
- fs
- path
- os

## Require a module

```js
const http = require("http");
```

## http object from "http" module

It has several methods

### createServer((req, res) => {})

It returns server object which takes a call back function as parameter which takes request and respons objecs as parameter. When a request is made, this call back function will be called and return response.

```js
const server = http.createServer((req, res) => {
  console.log(req);
});
```

### server.listen(port, callBackFunc)

When the `server` object is created from createServer() method, we have access to listen(port, callBackFunc) method which takes a port number as first parameter and a callBackFunc as second parameter. It's keep on listening to requests.

```js
server.listen("3000", () => console.log("listening to port 3000"));
```

### Request object
