# [ExpressJS - Hello World](https://www.tutorialspoint.com/expressjs/expressjs_hello_world.htm)

```javascript
const express = require('express')
const app = express()

app.get('/', function (req, res) {
    res.send('Hello World');
})

app.listen(3000)
```