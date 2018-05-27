# [Node.js HTTP Module](https://www.w3schools.com/nodejs/nodejs_http.asp)

```javascript
var http = require('http');

http.createServer(function (req, res) {
    res.write('Hello World!');
    res.end();
}).listen(8080);
```

添加请求头

```bash
var http = require('http');
http.createServer(function (req, res) {
    res.writeHead(200, { 'Content-Type': 'text/html' });
    res.write('Hello World!');
    res.end();
}).listen(8080);
```