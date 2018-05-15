# Custom Native Modules 🍮

## The Problem

Although React Native has extensive amounts of native module libraries available, thanks to the huge open source community around it, **sometimes there are specific requirements that arise during the course of a project which require native platform API access not provided by React Native.** As an example let's say you are working with a client and they have a custom SDK for authentication. The client has SDKs for iOS, Android, and Web. Although you are building a native app for Android/iOS on top of a Web technology \(React\), the irony is that this task suddenly becomes very difficult.

## Solution

The beauty of React Native is that it is designed to be a bridge between native code and web technologies. Thus, all you need to do is simply **integrate the native iOS and Android SDKs into the respective native projects and expose the methods and variables via a React Native module called: **`NativeModules`**.**

The module which talks to the native libs and can be accessed via JS is called Wrapper module. We will discuss how to create custom wrapper modules that can be used to invoke native code from Javascript in the following chapters.

On a side note, if the requirement was to integrate a native module that doesn't need to interact with the Javascript layer then we can safely integrate the native modules in the respective native projects for iOS and Android and not create a React Native bridge. One such use case can be crash reporters which need to send a crash report if an app crashes. Here you only integrate the crash reporter on the native code base of iOS and Android and not expose any method to the Javascript side. The initialization would also happen when the native app starts.

**Thus, if a functionality is technically supported by the native platform, it can be supported in React Native.**

