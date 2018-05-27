# [Node.js File System Module](https://www.w3schools.com/nodejs/nodejs_filesystem.asp)

## Read Files

`demofile1.html`

```html
<html>

<body>
    <h1>My Header</h1>
    <p>My paragraph.</p>
</body>

</html>
```

```javascript
var http = require('http');
var fs = require('fs');
http.createServer(function (req, res) {
    fs.readFile('demofile1.html', function (err, data) {
        res.writeHead(200, { 'Content-Type': 'text/html' });
        res.write(data);
        res.end();
    });
}).listen(8080);
```

打开 <http://localhost:8080>

输出结果

```html

<html>

<body>
    <h1>My Header</h1>
    <p>My paragraph.</p>
</body>

</html>
```
