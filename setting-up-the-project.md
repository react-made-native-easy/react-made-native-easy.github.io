# **Setting up the Project**

This section will cover the following.

* **Getting started** - This section will 
* This section covers the basic boilerplate setup of a sample project that we will be using to learn. I personally believe that one should setup his/her project from scratch instead of using a ready made boilerplate. The reason being , when a developer/team sets up its own boiler plate , they know what exactly is going into the app. A lot of boiler plates have features/libraries which the team will be unaware of and would contribute to the clutter

### create-react-native-app OR Expo OR react-native init 

React-native has a bunch of options to setup the project. create-react-native-app, react-native init and EXPO are among most popular ones and interestingly all three of them are mentioned in the official website of react-native. It might create a lot of confusion for someone who is starting to setup his/her own boilerplate.

Here is short desciption for each of them:

* _create-react-native-app:_ It is similar to the creat-react-app. It has all the necessary tasks to run your application. All the necessary setup has been taken care and you can start hacking around react-native. This is very useful if you are starting to learn react-native and you don't want to worry about anything else. It uses Exponent's open source tools to debug the application. To run the app, all you need to do is install expo client app and scan a QR code. Although its very quick to setup, everything seems like a black-box and it is not recommended to use it for production apps.

* _Expo_: Similar to 

* _react-native init:_ This provides you with the very basic setup of the application including native code\(IOS and Android folders\). You can change native code if you wish or not change it if not required. You would use native android/ios simulators or devices to run your application.You have 2 separate files which defines entry points for android and ios. You can run the dev version of the application by `react-native run-ios` \(It will open the ios emulator and run your app on it\). If you want to create a production level app\(which is probably why you are reading this book\) you would need to know much more than just react-native, which is exactly why we recommend using _react-native init._



