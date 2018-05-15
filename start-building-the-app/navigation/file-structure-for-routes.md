# File Structure for routes

As we add more and more routes, our router file tends to become huge and unmanageable. The `index.js` file will be containing styles defining the spacing of tabs, fontSize, etc and at the same time defining types of nested stacks and their screens.

This file can become unmanageable in a very short time. Hence we split the files into multiple files, each defining its own stack and importing styles from a separate file.

## Splitting your index.js file to multiple router files

The current index.js looks something like this:

> routes/index.js

```javascript
const AboutRoutes = TabNavigator({
  aboutApp: {
    screen: AboutApp,
    navigationOptions: {
      title: 'About the App'
    }
  },
  aboutDevs: {
    screen: AboutDevs,
    navigationOptions: {
      title: 'About the Creators'
    }
  }
}, {
  tabBarOptions: {
    upperCaseLabel: false,
    showIcon: false
  },
  swipeEnabled: true,
  ...,
  animationEnabled: true
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
});

export default Router;
```

As we can see, the current file defines two stacks, AboutRoutes, and the main Router.

Let's create a new file which will contain _just_ the screens required for About page and import it in the `routes/index.js` file.

> routes/index.js

```javascript
import AboutRoutes from './about.routes.js';

const Router = StackNavigator({
  home: {screen: HomePage,
    navigationOptions: {
      title: 'Start taking notes',
    }
  },
  about: {
    screen: AboutRoutes
  }
});

export default Router;
```

> routes/about.routes.js

```javascript
export default TabNavigator({
  aboutApp: {
    screen: AboutApp,
    navigationOptions: {
      title: 'About the App'
    }
  },
  aboutDevs: {
    screen: AboutDevs,
    navigationOptions: {
      title: 'About the Creators'
    }
  }
}, {
  tabBarOptions: {
    upperCaseLabel: false,
    showIcon: false
  },
  swipeEnabled: true,
  ...,
  animationEnabled: true
});
```

Pretty neat huh?

We can further modularize it such that all the navigation config data comes from a different file. This way, there will be a single file containing **navigation config** for all the routes. We can even reuse some of the configs.

## Taking out config data from routes file

Let's take out the configuration file from AboutRoutes.

> config/routes.config.js

```javascript
export const aboutRoutesConfig = {
  tabBarOptions: {
    upperCaseLabel: false,
    showIcon: false
  },
  swipeEnabled: true,
  animationEnabled: true
};
```

> routes/about.routes.js

```javascript
import {aboutRoutesConfig} from '../config/router.config';

export default TabNavigator({
  aboutApp: {
    screen: AboutApp,
    navigationOptions: {
      title: 'About the App'
    }
  },
  aboutDevs: {
    screen: AboutDevs,
    navigationOptions: {
      title: 'About the Creators'
    }
  }
}, aboutRoutesConfig);
```

The router file looks much cleaner now, defining just the routes and their title. In future, if we need to change the config, we need not go inside each route file and search for its config.

The code till here can be found on the **branch** [chapter/10/10.3](https://github.com/react-made-native-easy/note-taker/tree/chapter/10/10.3)

