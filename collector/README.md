# WebGuard's Forensics Engine

## Server
Directory ```server``` contains required files to start the server which serves htmls, listens for REST APIs and also handles socket connections from client.


To initiate the server, run the following command:
```
node index.js
```

In order to log browser events in files, you can set ```OUTPUT_FORMAT``` to ```FILE``` environment variable. Otherwise, the events are logged in console stdout.
```
OUTPUT_FORMAT=FILE node index.js
```
 If this env is set, the events log files will be availbe in ```logs/{uuid}.log``` where ```uuid``` is the id assinged to each user upon their first connection and saved in the browser cookie afterwards.

## Client
The tracking module is developed using [Evtrack](https://github.com/luileito/evtrack) project with improvements such as usage of a socket connection for sending the data. In order to enable tracking for an html file, you need to add the following scripts:

```html
<script type="text/javascript" src="/js/tracker.js"></script>
<script>
  (function(){
    TrackUI.record({
      postServer: "http://localhost:8080",
      regularEvents: "*",
      pollingEvents: '*',
      pollingMs: 1000,
      debug: true,
      postInterval: 1,
    });
  })();
</script>
```
The sample is available in ```tracker/index.html``` file.
A complete list of available options for the tracker is available below:
```js
{
  // The server where logs will be stored.
  postServer: "http://my.server",
  // The interval (in seconds) to post data to the server.
  postInterval: 30,
  // Events to be tracked whenever the browser fires them. Default:
  //      mouse-related: "mousedown mouseup mousemove mouseover mouseout mousewheel click dblclick"
  //      touch-related: "touchstart touchend touchmove"
  //   keyboard-related: "keydown keyup keypress"
  //     window-related: "load unload beforeunload blur focus resize error online offline"
  //             others: "scroll change select submit reset contextmenu cut copy paste"
  // If this property is empty, no events will be tracked.
  // Use space-separated values to indicate multiple events, e.g. "click mousemove touchmove".
  // The "*" wildcard can be used to specify all events.
  regularEvents: "*",
  // Events to be polled, because some events are not always needed (e.g. mousemove).
  // If this property is empty (default value), no events will be polled.
  // Use space-separated values to indicate multiple events, e.g. "mousemove touchmove".
  // The "*" wildcard can be used to specify all events.
  // Events in pollingEvents will override those specified in regularEvents.
  // You can leave regularEvents empty and use only pollingEvents, if need be.
  pollingEvents: "",
  // Sampling frequency (in ms) to register events.
  // If set to 0, every single event will be recorded.
  pollingMs: 150,
  // A name that identifies the current task.
  // Useful to filter logs by e.g. tracking campaign ID.
  taskName: "evtrack",
  // A custom function to execute on each recording tick.
  callback: null,
  // Whether to dump element attributes together with each recorded event.
  saveAttributes: true,
  // Enable this to display some debug information
  debug: false
})
```

## Sample Traces
The following shows a few sample of recorded events saved as objects in the MongoDB.
```js
{ "userId" : "dc235397-45f9-4322-bef5-c402a8a7e3ff", "eventName" : "load", "timestamp" : "1677529004106", "pos" : { "x" : 0, "y" : 0 }, "cursor" : 0, "element" : "/", "attrs" : "", "extra" : "{\"trusted\":true}", "url" : "http://localhost:9010/"}
{ "userId" : "dc235397-45f9-4322-bef5-c402a8a7e3ff", "eventName" : "pageshow", "timestamp" : "1677529004106", "pos" : { "x" : 0, "y" : 0 }, "cursor" : 0, "element" : "/", "attrs" : "", "extra" : "{\"trusted\":true}", "url" : "http://localhost:9010/"}
{ "userId" : "dc235397-45f9-4322-bef5-c402a8a7e3ff", "eventName" : "scroll", "timestamp" : "1677529004209", "pos" : { "x" : 0, "y" : 0 }, "cursor" : 0, "element" : "/", "attrs" : "", "extra" : "{\"trusted\":true}", "url" : "http://localhost:9010/"}
{ "userId" : "dc235397-45f9-4322-bef5-c402a8a7e3ff", "eventName" : "scroll", "timestamp" : "1677529004770", "pos" : { "x" : 0, "y" : 0 }, "cursor" : 0, "element" : "/", "attrs" : "", "extra" : "{\"trusted\":true}", "url" : "http://localhost:9010/"}
{ "userId" : "dc235397-45f9-4322-bef5-c402a8a7e3ff", "eventName" : "mouseover", "timestamp" : "1677529006111", "pos" : { "x" : 3, "y" : 117 }, "cursor" : 0, "element" : "/html[null]", "attrs" : "\"HTML\":{", "extra" : "{\"trusted\":true}", "url" : "http://localhost:9010/"}
{ "userId" : "dc235397-45f9-4322-bef5-c402a8a7e3ff", "eventName" : "mousemove", "timestamp" : "1677529006113", "pos" : { "x" : 3, "y" : 117 }, "cursor" : 0, "element" : "/html[null]", "attrs" : "\"HTML\":{", "extra" : "{\"trusted\":true}", "url" : "http://localhost:9010/"}

```