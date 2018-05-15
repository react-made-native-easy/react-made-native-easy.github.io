# Navigation 🚪

Navigation in react-native is pretty different, especially if you are coming from a web background. It follows native standards for navigation. For example, it uses the concept of screens, stacks, push/pop, which most web developers have not used for routing.

> It is not as simple as doing `dispatch(push('<routeName>')`. You have to manage screens, reset your stack, nest them properly. If not configured well, it can cause huge performance loss and multiple components to be mounted at the same time.

React-native has a bunch of options for routing. The different options have been mentioned here: [https://facebook.github.io/react-native/docs/navigation.html](https://facebook.github.io/react-native/docs/navigation.html). We have found react-navigation to be the most stable among all of them.

## Why react-navigation?

* The official react-native documentation recommends using react-navigation. It says, `"You will probably want to use React Navigation."`.
* It supports native transitions for both Android and iOS.
* Both Android and iOS routes can be handled by one single configuration file without any platform specific configuration.
* Provides multiple types of navigators out of the box. E.g.: StackNavigator, TabNavigator, DrawerNavigator.
* It is highly configurable. You can configure almost everything, be it tabs, header or footer, using a single config file.
* Redux integration is available. It's very easy to integrate with the Redux store. The benefit of this is that you can navigate by just dispatching an action and passing the route name. It makes route management much easier. After integration, all you need to do is: `dispatch(NavigationActions.navigate({routeName}))`.

