Icons in react-native

Those who come from web development background knows the power of svg icons. We always prefer svg\(for icons, images\) and what not. Benefits of SVGs are:

* They are resolution independent\(scalable\)
* They provide you more flexibility in changing color etc.
* They are lesser in size
* They are written in XML syntax so you can put directly inside your html without worrying about bundling your assets.

_That was about web, does react-native support SVGs?_

Unfortunately, rendering SVGs in native is not as simple as it is in HTML/WEB, where you can just use svg as image url or copy paste svg content inside your HTML. Unlike web, react-native doesn't support SVGs out of the box. Though there are some plugins which lets you render SVGs, but they do not support all SVG elements and it is not very easy to use.

_Now how do you get the power of SVG in native environment?_

Let me introduce you to this amazing library called \[react-native-vector-icons\]\(https://github.com/oblador/react-native-vector-icons\)

