## Navigation

Navigation in react-native is pretty different, specially if you are coming from web background. It follows native standards for navigation. Example: Screens, stacks, push/pop, which most of us web developers have not used for routing.

> It is not as simple as doing `dispatch(push('/<routeName>)` , you have to manage screens, reset your stack, nest them properly. If not configured well, it can cause huge performance loss and muliple components being mounted at a single point of time.

React-native has a bunch of options for routing. \(They have mentioned it here: [https://facebook.github.io/react-native/docs/navigation.html](https://facebook.github.io/react-native/docs/navigation.html)\).

We found React-navigation the most stable among all the options.

### Why react-navigation?

* It is a wrapper over NavigationExperimental\(which supports the smooth native-thread animations provided by the Animated library\). Moreover, NavigationExperimental is removed from the latest react-native version because some libraries sprung up around it which made it much easier to use than the official implementation.

* The official react-native documentation suggests React-navigation. `You will probably want to use React Navigation.`: Says the documentation

* It supports native transitions for both android and IOS.

* Both android and IOS routes can be handled by one single configuration file without any platform specific configuration.

* Provides multiple type of navigators out of the box. eg: StackNavigator, TabNavigator, DrawerNavigator.

* Redux integration available: Its very easy to integrate with redux store. The benefit of this is that you can navigate by just dispatching action and passing routename. It makes route management much more easier. After integration, all you need to do is: `dispatch(NavigationActions.navigate({routeName}))`.



