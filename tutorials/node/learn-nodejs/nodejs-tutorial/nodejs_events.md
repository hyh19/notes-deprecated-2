# [Node.js Events](https://www.w3schools.com/nodejs/nodejs_events.asp)

## Events in Node.js

```javascript
var fs = require('fs');

var readStream = fs.createReadStream('./sample.txt');
readStream.on('open', function () {
    console.log('The file is open');
});
```

## The EventEmitter Object

```javascript
var events = require('events');
var eventEmitter = new events.EventEmitter();

var myEventHandler = function () {
    console.log('I hear a scream!');
}

eventEmitter.on('scream', myEventHandler);

eventEmitter.emit('scream');
```