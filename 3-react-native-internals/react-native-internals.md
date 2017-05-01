# React Native Internals

React Native is a framework which allows developers to build native apps using Javascript. Wait! Cordova already does that
and has been around for quite a while. Why will anyone want to use RN ?

The primary difference between RN and Cordova based apps is that Cordova based apps run inside a webview while RN apps render using native views. RN apps have direct access to all the Native APIs and views offered by the underlying mobile OS. Thus, RN apps have the same feel and performance of the native application.

At first, its easy to assume that react native might be compiling JS code into the respective native code directly. But this would be really hard to achieve since Java and Objective C are strongly typed languages while Javascript is not ! Instead RN does something much much clever. React Native essentially can be considered as a set of React components, where each component represents the corresponding native views and components. For example, a native TextInput will have a corresponding RN component which can be directly imported onto the JS code and used like any other react component. Hence, the developer will be writing the code just like for any other React web app but the output will be a native app.

Ok ! This all looks black magic 🙄.

To understand this, lets take a look at the architecture and how react native works internally.

### Architecture 🤖

Both IOS and Android have a similar architecture with subtle differences.

If we consider the big picture, there are three parts to the RN platform:

1. **Native Code/Modules** :
  Most of the native code in case of IOS is written in Object C or Swift. While in case of android it is Java.
  But for most cases we will not need to write any native code while writing our react native app.

2. **Javascript VM** :
  The JS Virtual Machine that runs all our JavaScript code.
  On iOS/Android simulators and devices React Native uses JavaScriptCore, which is the JavaScript engine that powers Safari.
  JavaScriptCore is an open source JavaScript engine originally built for WebKit.
  Incase of IOS, React Native uses the IOS platform - provided JavaScriptCore.
  It was first introduced in IOS 7 along with OSX Mavericks.<br/>
  https://developer.apple.com/reference/javascriptcore.


  In case of Android, React Native bundles the JavaScriptCore along with the application. This increases the app size. Hence the hellow world application of RN would take around 3 to 4 megabytes.

  In case of Chrome debugging mode, the JavaScript code runs within Chrome itself (instead of the JavaScriptCore on the device) and communicates with native code via WebSocket. So, here it will use the V8 engine. This allows us to see a lot of information on the chrome debugging tools like network requests, console logs ,etc. 😎

3. **React Native Bridge** :
  React Native bridge is a C++/Java bridge which is responsible for communication between the native and Javascript thread.
  A custom protocol is used for message passing.

![](/assets/images/rn-architecture.png)

In most cases, a developer would write the entire react native application in Javascript. To run the application one of the following commands are issued via the cli - `react-native run-ios` or `react-native run-android`. At this point React native cli would spawn a node packager/bundler that would bundle the js code into a single `main.bundle.js` file. The packager can be considered something similar to webpack. Now, whenever the react native app is launched, at first the native entry point view is loaded. The Native thread spawns the JS VM thread onto which the bundled JS code is run. The JS code has all the business logic of the application. The Native thread now sends messages via the RN Bridge to start the JS application. Now the spawned Javascript thread starts issuing instructions to the native thread via the RN Bridge. The instructions include what views to load, what information to be retrieved from the hardware, etc. For example, if the js thread wants a view and text to be created it will batch the request onto a single message and send it across to native to render them.

`[ [2,3,[2,'Text',{...}]] [2,3,[3,'View',{...}]] ]`

The native thread will perform these operations and send the result back to the JS assuring that the operations have been performed.

*__Note__: To see the bridge messages on the console, just put the following snippet onto the `index.<platform>.js` file*
```
import MessageQueue from 'react-native/Libraries/BatchedBridge/MessageQueue';
MessageQueue.spy(true);
```

### Threading Model 🚧

When a react native application is launched , it spawns up the following threading queues.

1. **Main thread** _(Native Queue)_ - This is the main thread which gets spawned as soon as the application launches.
It loads the app and starts the JS thread to execute the Javascript code. The native thread also listens to the UI events like 'press' , 'touch', etc. These events are then passed onto the JS thread via the RN Bridge. Once the Javascript loads, the JS thread sends the information on what needs to be rendered onto the screen. This information is used by a **shadow node thread** to compute the layouts. The shadow thread is basically like a mathematical engine which finally decides on how to compute the view positions. These instructions are then passed back to the main thread to render the view.

2. **Javascript thread** _(JS Queue)_ - The Javascript Queue is the thread queue where main bundled JS thread runs.
The JS thread runs all the business logic - the code we write in React.

3. **Custom Native Modules** - Apart from the threads spawned by React Native, we can also spawn threads on the custom native modules we build to speed up the performance of the application.
For example - Animations are handled in React Native by a separate native thread to offload the work from the JS thread.

Links: https://www.youtube.com/watch?v=0MlT74erp60


### View Managers 👓

View Manager is a native module that maps JSX Views onto Native Views.
For example:

```
import React, { Component } from 'react';
import { AppRegistry, Text } from 'react-native';

class HelloWorldApp extends Component {
  render() {
    return (
      <View>
        <Text>Hello world!</Text>
      <View>
    );
  }
}

AppRegistry.registerComponent('HelloWorldApp', () => HelloWorldApp);
```

Here when we write `<Text />`, the Text View manager will invoke `new TextView(getContext())` in case of android.

View Managers are basically classes extended from `ViewManager` class in Android and subclasses of `RCTViewManager` in IOS.


### Development mode 🔨

When the app is run on `DEV` mode ,the Javascript thread is spawned on the development machine. Even though the JS code is running on a more powerful machine as compared to a phone, you will soon  notice that the performance is considerably low as compared to when run in bundled mode or production mode. This is unavoidable because a lot more work is done in DEV mode at runtime to provide good warnings and error messages, such as validating propTypes and various other assertions. Furthermore, latency of communication between the device and the JS thread also comes to play here.


Link : https://www.youtube.com/watch?v=8N4f4h6SThc - RN android architecture
