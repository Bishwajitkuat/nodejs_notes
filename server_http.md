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

request object is sent by the browser and it contains lots of valuable information. Some important keies are.

- headers
- url
- method

```js
const server = http.createServer((req, res) => {
  // URL
  console.log("URL");
  console.log(req.url);
  // METHOD
  console.log("METHOD");
  console.log(req.method);
  // HEADERS
  console.log("HEADERS");
  console.log(req.headers);
  res.end();
});
```

output

```shell
URL
/
METHOD
GET
HEADERS
{
  host: 'localhost:3000',
  connection: 'keep-alive',
  'cache-control': 'max-age=0',
  'sec-ch-ua': '"Not_A Brand";v="8", "Chromium";v="120", "Google Chrome";v="120"',
  'sec-ch-ua-mobile': '?0',
  'sec-ch-ua-platform': '"Linux"',
  dnt: '1',
  'upgrade-insecure-requests': '1',
  'user-agent': 'Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36',
  accept: 'text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7',
  'sec-fetch-site': 'none',
  'sec-fetch-mode': 'navigate',
  'sec-fetch-user': '?1',
  'sec-fetch-dest': 'document',
  'accept-encoding': 'gzip, deflate, br',
  'accept-language': 'en-US,en;q=0.9,fi-FI;q=0.8,fi;q=0.7,bn-BD;q=0.6,bn;q=0.5'
}
```

### Response object

it has several important method

-res.setHeader(name, value)
Set header of the response object, first parameter (string) is name of the header and second parameter is the value (anything). examples

- "Content-Type", "text/html"
- "Content-Type", "text/plain"
- "Content-Type", "application/json"
- 'Set-Cookie', ['type=ninja', 'language=javascript']

- res.write(chunk, encoding, callback)

  This method is used to send back data with response.
  chunk: data as string. It could be plain text, json data in string which can be read in browser, encoding: optional, defaul is utf8, callback: optional.

-res.end()

This method is called at the end of the response, we should not write any respons code after end() method. We must use end method because, if we do not end the response, browser will keeps reloading to get more response.

```js
const server = http.createServer((req, res) => {
  res.setHeader("Content-Type", "text/html");
  res.write("<h1>Hello from server!!!</h1>");
  res.end();
});
```

### Different response based on routes

```js
const server = http.createServer(async (req, res) => {
  const url = req.url;
  if (url === "/") {
    res.setHeader("Content-Type", "text/html");
    res.write("<html>");
    res.write("<head><title>Sent Message</title></head>");
    // we are sinding a POST request to /message route with the form data;
    res.write(
      '<body> <form action="/message" method="POST"><input name="message" type="text"> <button type="submit" >Submit</button></form></body>'
    );
    res.write("</html>");
    // we must use return because we do not want end the execution here
    return res.end();
  } else if (url === "/message" && req.method === "POST") {
    // creating message.txt file with content Message
    fs.writeFileSync("message.txt", "Message");
    // Sending status code with response
    res.statusCode = 302;
    // Rederecting to root route by passing "Location" as header name and "/" as value
    res.setHeader("Location", "/");
    return res.end();
  }
  // if no route is matched, this response will be sent
  res.setHeader("Content-Type", "text/html");
  res.write("<html>");
  res.write("<body> <h1>Page not found</h1> </body>");
  res.write("</html>");
  res.end();
});
```
