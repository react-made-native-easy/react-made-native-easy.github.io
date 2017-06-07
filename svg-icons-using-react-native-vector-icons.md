# Icons in react-native

Those who come from web development background knows the power of svg icons. We always prefer svg\(for icons, images\) and what not. Benefits of SVGs are:

* They are resolution independent\(scalable\)
* They provide you more flexibility in changing color etc.
* They are lesser in size as compared to an image.
* They are written in XML syntax so you can put directly inside your html without worrying about bundling your assets.

### That was about web, does react-native support SVGs?

Unfortunately, rendering SVGs in native is not as simple as it is in HTML/WEB, where you can just use SVG as image IMG or copy paste svg content inside your HTML. Unlike web, react-native doesn't support SVGs out of the box. Though there are some plugins which lets you render SVGs, but they do not support all SVG elements and it is not very easy to use.

### How do you get the power of SVG in native environment?

Let me introduce you to this amazing library called \[react-native-vector-icons\]\([https://github.com/oblador/react-native-vector-icons](https://github.com/oblador/react-native-vector-icons%29%29%29\). It comes bundled with a bunch of icon sets\(default is [`FontAwesome`](http://fortawesome.github.io/Font-Awesome/icons/)\).

* All the fonts are scalable and you can style them just like SVGs.
* It returns a react component which accepts name, etc. as prop. After integration, the usage will be as simple as `<Icon name="rocket" size={30} color="#900" />`
* You can pass custom style.
* It also supports a couple of other components which might be useful. Ex, Button with an icon \(`<Icon.Button />`\)
* You can also create your own iconset if you want to use custom icons.

### Creating custom iconset

`react-native-vector-icons` support using custom icon sets if you do not want to use the icons which come bundled or if you want to add your own icons. It supports Fontello and IcoMoon to create custom fonts. We used icomoon to convert our svgs to a config which is readable by the library.  The steps for the same are as follows:

* Open the [iconmoon application.](https://icomoon.io/app/#/select)
* Remove the current set and creaye a new empty set.

![](/assets/icomoon-1.png)

* Drag and drop your svg files on the tool.

![](/assets/icomoon2.png)

* Select the files which you want to export. Select all if you want to export all the icons. Click Download JSON.

![](/assets/icomoon3.png)

* Put the JSON file in your app and create a file called CustomIcon.js and import the config.json which you exported in the previous step.

```js
import {createIconSetFromIcoMoon} from 'react-native-vector-icons';
import icoMoonConfig from './config.json';
export default createIconSetFromIcoMoon(icoMoonConfig);
```

* To use a font, Simply import the file as a react component and pass the icon name and size\(optional\)

```js
import CustomIcon from './components/CustomIcon.js'

<CustomIcon name='android' /> //To use the icon
<CustomIcon name='android' size={25} /> // To pass size
<CustomIcon name='android' style={styles.androidIcon} /> // To pass custom tyle


```



