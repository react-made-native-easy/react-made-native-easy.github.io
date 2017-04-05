1. Source -- https://www.youtube.com/watch?v=8N4f4h6SThc

Java/c++   ------>  JS VM (ios --- in system)
(Native)   Bridge         (android --- package with the app) --so app size is bigger (3.5mb for hello world)
          <-------

Bridge (c++) --- follows custom protocol for message passing

Messages from the Native are send first to start the application via an entry point.
then
Messages from JS are sent in batches to the Native environment to perform some operations.
[[2,3,[2,'Text',{...}]]
[2,3,[3,'View',{...}]]]

For example if the js environment wants two textviews to be created it will batch the request onto a single message and
send it across to native to render them.


```import MessageQueue from 'react-native/Libraries/BatchedBridge/MessageQueue';
MessageQueue.spy(true);```
To see the bridge Messages



Threading

Three threading queues
UI Events(Native Queue)  ------ React Native Modules(Queue) -------  JS Event Queue

1. (UI Queue) UI Touch -> JS Event handler (Dispatched changes required) --> Native Modules Queue (Recomputes the layout using css layouting) ---> Update the UI (UI Queue)



Alternate threading

1.Main Thread

Native Modules

View Managers map JS/JSX Views  to Native Views

for example <Text /> ---> new TextView(getContext())



Development mode (dev=true)

JavaScript thread performance suffers greatly when running in dev mode. This is unavoidable: a lot more work needs to be done at runtime to provide you with good warnings and error messages, such as validating propTypes and various other assertions.





DaveKerr blog article to be incorporated for managing build files
http://www.dwmkerr.com/tips-and-tricks-for-beautifully-simple-mobile-app-ci/
