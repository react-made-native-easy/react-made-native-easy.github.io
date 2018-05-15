# SVG Icons using react-native-vector-icons 🐾

Those who come from a web development background know the power of SVG icons. We prefer using SVGs \(for icons, images\) because of the following reasons:

* They are resolution independent \(scalable\).
* They provide us more flexibility in changing color.
* They are smaller in size as compared to images.
* They are written in XML syntax so they can be inserted directly into HTML without worrying about bundling assets.

## That was on the web. Does react-native support SVGs?

Unfortunately, rendering SVGs in native is not as simple as it is in HTML/Web, where you can use SVG as an image or copy paste SVG content inside your HTML. Unlike the web, react-native doesn't support SVG out of the box. Though there are some plugins which let you render SVG, they do not support all SVG elements and are not very easy to use.

## How do you get the power of SVG in a native environment?

Let me introduce you to this amazing library called [`react-native-vector-icons`](https://github.com/oblador/react-native-vector-icons). It comes bundled with a bunch of icon sets \(default is [`FontAwesome`](https://fontawesome.com/icons)\).

* All the fonts are scalable and you can style them just like SVG.
* It returns a React component which accepts name, etc. as prop. After integration, the usage will be as simple as `<Icon name="rocket" size={30} color="#900" />`
* You can pass custom style.
* It also supports a couple of other components which might be useful. Example: button with an icon \(`<Icon.Button />`\)
* You can also create your own icon set if you want to use custom icons.
* Installation is a 2 step process if you wish to use the icons provided by FontAwesome or other similar font libraries. \(`yarn add` and `react-native link`\)
* If you wish to use custom fonts \(made in SVG\), please read the next chapter.

