# [Node.js HTTP Module](https://www.w3schools.com/nodejs/nodejs_http.asp)

```javascript
var http = require('http');

http.createServer(function (req, res) {
    res.write('Hello World!');
    res.end();
}).listen(8080);
```

## Add an HTTP Header

```javascript
var http = require('http');
http.createServer(function (req, res) {
    res.writeHead(200, { 'Content-Type': 'text/html' });
    res.write('Hello World!');
    res.end();
}).listen(8080);
```

## Read the Query String

```javascript
var http = require('http');
http.createServer(function (req, res) {
    res.writeHead(200, { 'Content-Type': 'text/html' });
    res.write(req.url);
    res.end();
}).listen(8080);
```

在浏览器打开

- <http://localhost:8080/summer>

输出结果

```html
/summer
```

- <http://localhost:8080/winter>

输出结果

```html
/winter
```

## Split the Query String

```javascript
var http = require('http');
var url = require('url');

http.createServer(function (req, res) {
    res.writeHead(200, { 'Content-Type': 'text/html' });
    var q = url.parse(req.url, true).query;
    var txt = q.year + " " + q.month;
    res.end(txt);
}).listen(8080);
```

在浏览器打开 <http://localhost:8080/?year=2017&month=July>

输出结果

```html
2017 July
```