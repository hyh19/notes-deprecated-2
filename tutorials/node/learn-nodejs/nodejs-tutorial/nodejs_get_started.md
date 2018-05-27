# [Node.js Get Started](https://www.w3schools.com/nodejs/nodejs_get_started.asp)

```javascript
var http = require('http');

http.createServer(function (req, res) {
    res.writeHead(200, { 'Content-Type': 'text/html' });
    res.end('Hello World!');
}).listen(8080);
```

执行脚本

```bash
node server.js
```

打开 <http://localhost:8080/>