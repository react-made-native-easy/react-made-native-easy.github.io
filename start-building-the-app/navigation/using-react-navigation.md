# Using React-navigation

Using react-navigation is pretty simple once you understand how it works. Unfortunately, their official documentation lacks some explanations. But there is no need to worry. We will try to fill in the gaps in the official documentation in this chapter. The official documentation is available at [https://reactnavigation.org/docs/getting-started](https://reactnavigation.org/docs/getting-started)

### How does navigation work in native applications?

When it comes to native apps, the routing works on the concept of stacks, screens, push/pop, etc.

### What is a stack in navigation?

We all have heard of stacks in our college programming classes. Well, this is exactly the same stack. Whenever you navigate from one screen to another, a new screen comes up on top. The old screen is still there \(unless you reset your stack\). This mechanism of caching the old pages helps in improving performance and making the transitions smoother. Have a look at the image below for an example of stack in navigation.

![](../../.gitbook/assets/stack.svg)

### Types of navigators offered by react-navigation

React-navigation offers a bunch of navigators predefined for our use. An app might contain all the available navigators. Let's try to understand all three of them one-by-one.

* StackNavigator: This is the most common navigator of all. You will use it for most react-native applications. We just explained how stacks work in navigation. You define a StackNavigator by passing the routes object inside the StackNavigator function from react-navigation. Each of the screens gets mounted only when you navigate to that particular screen and gets un-mounted only when you go back or manually reset the navigation state.
* TabNavigator: This is also quite a common user interface where we have a bunch of tabs and can swipe between them. We have seen this in Facebook, Twitter, and many other popular apps. React-navigation supports this navigator out of the box. The way of defining a TabNavigator is pretty similar to a StackNavigator. The only difference is that the routes you define in TabNavigator get mounted all at once. So you need to be a little careful when managing navigation to/from a different stack.
* DrawerNavigator: This navigator gives us the side-menu which we see in most mobile applications nowadays. You can obviously customize everything in your drawer since it is all written in JavaScript. You can set up the DrawerNavigator by writing only 4 lines of code. The DrawerNavigator comes with a default UI which supports an icon and a title per list item, which looks quite elegant. You can also define your own component to show up as a drawer if you want to have a custom UI and actions for your side-menu. More info can be found [here](https://reactnavigation.org/docs/drawer-based-navigation).

## Creating a router

Install `react-navigation` using npm or Yarn.

`yarn add react-navigation` or `npm install react-navigation --save`

Creating a router is pretty easy. You just define a page component \(which will be a container component\) and then import it in your router.js file.

Each of the navigators accepts an object during initialization whose syntax is as follows:

```javascript
const Router = StackNavigator({
  <routeName, string>: {screen: <screenObject, enum(React component, navigator instance)>, navigationOptions: <Screen level nav options>}
}, {
  navigationOptions: <Router level default nav options>
})
```

`routeName` is the name associated with the current screen. It will be used for navigation/analytics tracking.

`screenObject` The screen key in the router object can contain:

* A React Native component.
* A react-navigation router instance if you want routers to be nested.

> Example routes/index.js

```javascript
const AboutRoutes = TabNavigator({
  aboutApp: {
    screen: AboutApp,
  },
  aboutDevs: {
    screen: AboutDevs,
  }
});

const Router = StackNavigator({
  home: {screen: HomePage,
    navigationOptions: {
      title: 'Start taking notes',
    }
  },
  about: {
    screen: AboutRoutes
  }
}, {
  mode: 'card'
});

export default Router;
```

Each of the navigators returns a React component which is supposed to be added to the root level of the app.

> Example App.container.js

```javascript
import React, {Component} from 'react';
import Router from './routes';
import {connect} from 'react-redux';

class App extends Component {
  render () {
    return (
      <Router />
    );
  }
}

export default App;
```

When we define a React component in our router file, it adds a few properties to the component, which are:

* static [navigationOptions](https://reactnavigation.org/docs/headers.html#setting-the-header-title): We can use this to define our headers, title, etc. However, we recommend defining this in router.js because we will be importing pages to our router and defining header UI/title in the container component is not a good idea.
* this.props.navigation: react-navigation also adds a navigation function to the screen. This function can be used to navigate to a route and pass parameters. We do not recommend this way as we will be handling routing via [redux-actions](https://reactnavigation.org/docs/redux-integration.html)

The code till here can be found on the **branch** [chapter/10/10.1](https://github.com/react-made-native-easy/note-taker/tree/chapter/10/10.1)

