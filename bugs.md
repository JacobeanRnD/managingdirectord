If transition is missing, infinite alert notification.
We should use something other than alert. 
Does not support empty script.
save -w reattaches event listeners after going from error to success and back. too many saves!

Unable to recover from:

ERROR: Error saving statechart
{"name":"error.create","data":[["Document is not valid! Line: 250, Column: 0","Element '{http://www.w3.org/2005/07/scxml}transition', attribute 'event': [facet 'pattern'] The value '' is not accepted by the pattern '\\.?\\*|(\\i|\\d|\\-)+(\\.(\\i|\\d|\\-)+)*(\\.\\*)?(\\s(\\i|\\d|\\-)+(\\.(\\i|\\d|\\-)+)*(\\.\\*)?)*'.\n"],["Document is not valid! Line: 250, Column: 0","Element '{http://www.w3.org/2005/07/scxml}transition', attribute 'event': '' is not a valid value of the atomic type '{http://www.w3.org/2005/07/scxml}EventTypes.datatype'.\n"]]}

Visualization overlay. Stuff gets overlayed strangely on top of other stuff.


How to package up node_modules in the same directory? We need a project structure... scxml build should send tar of directory to server. 
  Maybe we need a builder/buildpack. 

node_modules need to be installed in the context embedding scion. this will either be the docker container image, or the simluation provider (for simple-simulation-provider). Need to consider a better way to do that. 


Error unregistering listener:

/home/jacob/.nvm/v0.12.1/lib/node_modules/scxmld/node_modules/SCXMLD-simple-simulation-provider/index.js:78
    instance.unregisterListener(instance.listener);
            ^
TypeError: Cannot read property 'unregisterListener' of undefined
    at Object.server.unregisterListener (/home/jacob/.nvm/v0.12.1/lib/node_modules/scxmld/node_modules/SCXMLD-simple-simulation-provider/index.js:78:13)
    at /home/jacob/.nvm/v0.12.1/lib/node_modules/scxmld/app/api.js:231:20
    at IncomingMessage.<anonymous> (/home/jacob/.nvm/v0.12.1/lib/node_modules/scxmld/app/sse.js:21:5)
    at IncomingMessage.emit (events.js:104:17)
    at abortIncoming (_http_server.js:280:11)
    at Socket.socketOnEnd (_http_server.js:393:7)
    at Socket.emit (events.js:129:20)
    at _stream_readable.js:908:16


Simple simulation provider and docker simulation provider return different responses:
```
jacob@jacob-ThinkPad-W520:~/workspace/jacobean/research-prototypes/managing-director$ scxml ls
Statechart list:
["managing-director.scxml","helloworld.scxml"]
jacob@jacob-ThinkPad-W520:~/workspace/jacobean/research-prototypes/managing-director$ scxml ls
Statechart list:
{"name":"success.get.charts","data":{"charts":["managing-director.scxml","helloworld.scxml"]}}
```


------------

prefer XML response on 
jacob@jacob-ThinkPad-W520:~/workspace/jacobean/research-prototypes/managing-director$ scxml cat helloworld.scxml

Rather than JSON:
{"name":"success.get.definition","data":{"scxml":"<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n<scxml xmlns=\"http://www.w3.org/2005/07/scxml\" name=\"helloworld\" datamodel=\"ecmascript\" version=\"1.0\">\n  <state id=\"a\">\n    <transition target=\"b\" event=\"t\"/>\n  </state>\n  <state id=\"b\">\n    <transition target=\"c\" event=\"t\"/>\n  </state>\n  <state id=\"c\">\n    <transition target=\"a\" event=\"t\"/>\n  </state>\n</scxml>"}}



