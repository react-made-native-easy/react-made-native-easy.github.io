## Eslint : The guardian of code conventions ⚔️

Every team composes of developers who follow different conventions. Hence, often we find that code written by a developer is not easily understood by another developer in the same team. This creates dependencies on individuals and tight dependencies in a software development project can affect the velocity of the team.

The best way to solve this is by deciding and following code conventions.

> We strongly believe that NO convention is bad.
>
> As long as there is some convention and it is followed relegiously,  its good.

Code conventions can be as easy as following 4 spaces instead of tabs, or ending a statement always with semicolon, etc. It could also be something more complex like not allowing `setState()`to be invoked on `componentWillMount`of a React Component.

**Eslint** is a tool that allows to maintain code quality and enforce code conventions. Eslint is a static code evaluator. Basically, it means that eslint will not actually run the code but will instead read through the source code to see if all the pre - configured code conventions are followed by the developers.

Eslint allows to maintain consistent code style throughout the project. Thus, any developer of the team can easily understand the code written by another developer. This can exponentially increase the teams velocity and avoid dependencies.

Apart from code conventions , eslint also spots common mistakes  made by developers. For example,

```js
var a = 1, b = 2, c = 3, e = 4;

var test = function() {
console.log(a, b, c, d, e);
};
```

The above code will compile fine. But as soon as u run it , it will throw a runtime exception`ReferenceError`

```js
test()

VM206:4 Uncaught ReferenceError: d is not defined
    at test (<anonymous>:4:22)
    at <anonymous>:1:1
test @ VM206:4
(anonymous) @ VM222:1
```

These mistakes can be easily detected by eslint.

It is pretty easy to setup eslint for a project.

```
yarn add --dev eslint
```

This would install eslint as your dev dependency.

And then in your `package.json` add a `npm script` as below.

So now your `package.json` should have

```js
{
  "name": "testapp",
  "version": "0.0.1",
  "scripts": {
    "start": "node node_modules/react-native/local-cli/cli.js start",
    ....
    ....
    ....
     "lint": "eslint app/",
     "lint:fix": "eslint app/ --fix"
  },
  "dependencies": {
  ....
  ....
```

Now you can simply run

```
npm run lint
or 
npm run lint:fix
```

`npm run lint` will just run the eslint and show a list of errors that need to be fixed.`npm run lint:fix` will run eslint and attempt to correct the errors it is able to fix automatically.

Now for the icing on the cake. Most modern editors have support for **eslint **via plugins.  The benefit of a text editor eslint plugin is that these plugins suggest correction while we write code itself thus saving a lot of time for the developer.

A editor configured with eslint would look something like this.

![](/assets/images/eslint-error-editor.png)

Also, some of these plugins also support features like **lint on save.**  Thus, eslint attempts to run`eslint --fix <current_file>` This fixes all auto fixable lint errors such as incorrect indentation spaces, etc the moment you  hit save \(cmd+s\) on a file.

**Eslint** can be configured via a configuration file `.eslintrc` which should be placed in the root directory of the project.

A typical `.eslintrc` file looks like this :

```js
# "off" or 0 - turn the rule off
# "warn" or 1 - turn the rule on as a warning(doesn’ t affect exit code)
# "error" or 2 - turn the rule on as an error(exit code is 1 when triggered)
{
  "parser": "babel-eslint",
  "env": {
    "browser": true
  },
  "plugins": [
    "react",
    "react-native"
  ],
  "ecmaFeatures": {
    "jsx": true
  },
  "extends": ["eslint:recommended", "plugin:react/recommended"],
  "rules": {
    "react/no-did-mount-set-state": 2,
    "react/no-direct-mutation-state": 2,
    "react/jsx-uses-vars": 2,
    "no-undef": 2,
    "semi": 2,
    "react/prop-types": 2,
    "react/jsx-no-bind": 2,
    "react/jsx-no-duplicate-props": 2,
    ....
    ....
    ....
  },
  "globals": {
    "GLOBAL": false,
    "it": false,
    "expect": false,
    "describe": false,
    ....
    ....
    ....
   }
}
```

You can find all the rules [here](http://eslint.org/docs/rules/).

Now you have to use this carefully. If you try to use everything or if you extend a plugin and use all the rules, you might end up spending hell lot of time fixing lint than writing code. So we suggest you to use rules which are auto fixable.

We wrote our ESLINT config file ourself and you can also write it yourself or refer to the file that we created. We recommend everyone should have at least these rules. It contains all the minimalistic things required. You can of course change it as per your needs. The best part is that all of our rules are auto fixable.

Some of my favorite rules are:

* `react/no-did-mount-set-state`
* `react/no-direct-mutation-state`
* `react/prop-types`
* `react/jsx-no-bind`
* `react/jsx-no-duplicate-props`

We also suggest using eslint for spacing/tabs instead of `.editorconfig`. This way, all the configuration level stuff would be coming from a single config file. and we can use the power of eslint auto fix to lint/indent file on save.

