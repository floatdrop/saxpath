SaXPath [![Build Status](https://secure.travis-ci.org/StevenLooman/saxpath.png)](http://travis-ci.org/StevenLooman/saxpath)
=======
Simple XPath evaluator which runs against a SAX stream. 

Supported XPath construct as of writing are:
  - '/'-axis (child)
  - '//'-axis (self-or-descendant)
  - node name tests, including namespaces
  - all nodes selector: '\*'
  - predicate test (@<attribute_name> = "<literal>")

Usage
-----

```javascript
var sax = require('sax');
var saxpath = require('saxpath');
var recorder = new (saxpath.TapeRecorder)();
var saxParser = new sax.createStream(true);
var streamer = new saxpath.SaXPath(saxParser, '/image/url', recorder);
saxParser.on('end', function() {
    var tape = recorder.box[0];
    console.log(tape); 
});
saxParser.write("<image><url>http://www.google.com/image.png</url></image>");
saxParser.end();
```

Inner workings
--------------
A state machine is built which the SAX-nodes are tested against. If a node matches, the state machine progresses.

For self-or-descendant-nodes, the state machine is forked and earch fork (including the parent) is tested against the SAX-nodes. This ensures all nodes are matched. See test/saxpath.js and test/inception.xml for an example.
