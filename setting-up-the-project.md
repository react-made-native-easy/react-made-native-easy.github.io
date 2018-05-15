# Setting up the project 🌈

This section covers the basic boilerplate setup of a sample project that we will be using to learn React Native. We believe that one should set up his/her project from scratch or use a minimal boilerplate instead of using a ready-made, feature rich boilerplate. The reason behind this is that when a developer/team sets up its own boilerplate, they know exactly what is going into the app. A lot of boilerplates have features/libraries which the team will be unaware of and it would only contribute to code clutter.

## Popular boilerplates' review

* create-react-native-app `OR`
* Expo `OR`
* react-native init

React-native has a bunch of options to set up a project. `create-react-native-app`, `react-native init` and `Expo` are among the most popular ones and interestingly, all three of them are mentioned on the official website of react-native. It might create a lot of confusion for someone who is starting with RN to decide which one to use.

Here is a short description of each of them:

* **create-react-native-app:** It is similar to `create-react-app`. It has all the necessary tasks to run your application. All the necessary setup has been taken care of and you can start hacking around react-native. This is very useful if you are starting with react-native and don't want to worry about anything else. It uses Exponent's open source tools to debug the application. To run the app, all you need to do is install the Expo Client app and scan a QR code. Although it is very quick to setup, everything seems like a black-box. Due to this, it can get pretty messy when you want to change a lot of configurations. Hence we do not recommend it for a long-term production application.
* **Expo**: It is a third-party framework which provides you with a lot of cool features like sharing your app with anyone while you are developing it and showing live code changes on a real device by just scanning a QR code. We do not recommend using this if your app uses a lot of third party native modules or you wish to hack around the native code since you don't get access to native code out of the box. Refer to this page to know more: [Why not Expo?](https://docs.expo.io/versions/latest/introduction/why-not-expo.html) Also, it supports only **Android 4.4 and above**, which can be a big turn off if your user base has a large number of **Android 4.1 - 4.3** users.
* **react-native init:** This provides you with a very basic setup of the application including native `ios` and `android` folders. This allows you to change the native code if required. You would use native Android/iOS simulators or devices to run your application. You can run the dev version of the application with `react-native run-ios` \(it will open the iOS simulator and run your app on it\). Here we will need to setup everything from scratch. But on the contrary, we get full access to native code and have control over almost everything that is going on in our application. Also, it is easier to upgrade `react-native` and other core libraries using this boilerplate as compared to others. Hence any critical updates can be integrated much more easily. **Thus, we recommend this boilerplate for any long term production application**.

  Setup instructions for **react-native init** can be found at [https://facebook.github.io/react-native/docs/getting-started.html](https://facebook.github.io/react-native/docs/getting-started.html). You can find them at the **Building Projects with Native Code** section.

