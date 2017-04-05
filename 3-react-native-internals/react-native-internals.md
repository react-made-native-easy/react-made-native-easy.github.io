# React Native Internals

<Introduction>

React Native lets us build native applications using Javascript. We get complete access to native views and native APIs
in both android and ios. Unlike cordova , it doesnt run your application inside a webview. React native apps are using native
views, fields ,etc to render onto the screen which allows it to provide native look and feel and also the performance of the native views.

At first, it all looks black magic ! Lets see how react native really works inside.

### Architecture

Both IOS and Android have a similar architecture with subtle differences.

If we consider the big picture, there are three parts to the RN platform:

1. **Native Code**
Most of the native code in case of IOS is in Object C or Swift. While in case of android its Java.
But for most cases we will not need to write any native code while writing our react native app.

2. **Javascript VM**
The JS Virtual Machine that runs all our JavaScript code.
On iOS/Android simulators and devices React Native uses JavaScriptCore, which is the JavaScript engine that powers Safari.
JavaScriptCore is the built-in open source JavaScript engine for WebKit.
Incase of IOS, React Native uses the IOS platform provided JavaScriptCore.
It was first introduced in IOS 7 along with OSX Mavericks.
Link : https://developer.apple.com/reference/javascriptcore.

In case of Android, React Native bundles the JavaScriptCore along with the application.

_Note_
In case of Chrome debugging, the JavaScript code runs within Chrome itself (instead of the JavaScriptCore on the device) and communicates with native code via WebSocket. So, here its using the V8 engine.

3. RN Bridge
React Native bridge is a C++/Java bridge which is responsible for communication between the native and the Javascript environments.
The RN Bridge can be considered as a link between both worlds.

Lets see the architecture wrt to Android we will go through subtle difference in IOS.


In both the platforms,the code we write in Javascript is bundled into a main.bundle js file using a packager.
The bundled code runs inside a Javascript VM.


1. Source -- https://www.youtube.com/watch?v=8N4f4h6SThc

Android
Java/c++     ----------------->        JS VM (ios --- in system)
(Native)      Bridge(Java/C++)         (android --- package with the app) --so app size is bigger (3.5mb for hello world)
             <-----------------

ios
c
JavascriptCore



Bridge (c++) --- follows custom protocol for message passing

Messages from the Native are send first to start the application via an entry point.
then
Messages from JS are sent in batches to the Native environment to perform some operations.
[[2,3,[2,'Text',{...}]]
[2,3,[3,'View',{...}]]]

For example if the js environment wants two textviews to be created it will batch the request onto a single message and
send it across to native to render them.


```
import MessageQueue from 'react-native/Libraries/BatchedBridge/MessageQueue';
MessageQueue.spy(true);
```
To see the bridge Messages



Threading

Three threading queues
UI Events(Native Queue)  ------ React Native Modules(Queue) -------  JS Event Queue

1. (UI Queue) UI Touch -> JS Event handler (Dispatched changes required) --> Native Modules Queue (Recomputes the layout using css layouting) ---> Update the UI (UI Queue)



Alternate threading (https://www.youtube.com/watch?v=0MlT74erp60)

1.Main Thread - The Native app thread -- this loads the app and starts up JS thread to load js code

2.JS Thread - Starts up , all the logic in JS also performs React rendering

3.Shadow Thread - Computes the layout in shadow nodes (does math where to render the component,etc) -- finally gives instruction to main thread to render the View

4.Custom Native Modules - can have their own threads



Native Modules

View Managers map JS/JSX Views  to Native Views

for example <Text /> ---> new TextView(getContext())



Development mode (dev=true)

JavaScript thread performance suffers greatly when running in dev mode. This is unavoidable: a lot more work needs to be done at runtime to provide you with good warnings and error messages, such as validating propTypes and various other assertions.





DaveKerr blog article to be incorporated for managing build files
http://www.dwmkerr.com/tips-and-tricks-for-beautifully-simple-mobile-app-ci/



Performance -
1.Keys in list
2.Should component Update
3.
