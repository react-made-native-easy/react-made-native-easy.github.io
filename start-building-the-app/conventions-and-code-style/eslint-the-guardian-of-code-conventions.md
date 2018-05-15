# ESLint: The guardian of code conventions ⚔️

**ESLint** is a tool that allows us to maintain code quality and enforce code conventions. ESLint is a static code evaluator. Basically, it means that ESLint will not actually execute the code but will instead read through the source code to see if all the preconfigured code conventions are followed by the developers.

> ESLint allows us to maintain consistent code style throughout the project. Thus, any developer on the team can easily understand the code written by another developer. This can exponentially increase the team's velocity and avoid dependencies. It can reduce human errors and can act as a guardian that maintains code conventions.

### Examples

Apart from code conventions, ESLint also spots common mistakes made by developers. For example,

```javascript
var a = 1, b = 2, c = 3, e = 4;

var test = function() {
console.log(a, b, c, d, e);
};
```

The above code will compile fine. But as soon as you execute it, it will throw a runtime exception `ReferenceError`

```javascript
test()

VM206:4 Uncaught ReferenceError: d is not defined
    at test (<anonymous>:4:22)
    at <anonymous>:1:1
test @ VM206:4
(anonymous) @ VM222:1
```

These mistakes can be easily detected by ESLint.

### Installation

It is pretty easy to setup ESLint for a project.

```text
yarn add --dev eslint babel-eslint eslint eslint-plugin-react eslint-plugin-react-native
```

This would install ESLint and other useful plugins as your dev dependencies.

After this, add an `npm script` in your `package.json` as given below.

Your `package.json` should now have

```javascript
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

You can simply run

```text
npm run lint
or
npm run lint:fix
```

`npm run lint` will run the ESLint and show a list of errors that need to be fixed. `npm run lint:fix` will run ESLint and attempt to correct the errors it is able to fix automatically.

## The icing on the cake

Most modern editors have support for **ESLint** via plugins. The benefit of a text editor ESLint plugin is that these plugins suggest corrections while we write code, thus saving a lot of time for developers.

An editor configured with ESLint would look something like this.

![](../../.gitbook/assets/eslint-error-editor.png)

Some of these plugins also support features like **lint on save.** Thus, ESLint attempts to run `eslint --fix <current_file>` the moment you save `(cmd+s)` a file. This fixes all auto-fixable lint errors such as incorrect indentation spaces, adding semicolons at the end of a line, etc.

![](../../.gitbook/assets/eslint-atom.png)

![](../../.gitbook/assets/atom-eslint-config.png)

We **strongly** recommend enabling this feature.

## The `.eslintrc` file

**ESLint** rules can be configured via a configuration file `.eslintrc` which should be placed in the root directory of the project.

A sample `.eslintrc` file looks like this:

```javascript
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

The important area in the above configuration is the rules section. This section controls all the code conventions followed in the project.

The complete list of all the available rules is present here: [http://eslint.org/docs/rules/](http://eslint.org/docs/rules/)

It can be pretty overwhelming at first to decide which rules should go in. Hence we can start with

```javascript
"extends": ["eslint:recommended", "plugin:react/recommended"]
```

These should cover all the basic rules needed to start your project.

But we recommend adding these as well

```javascript
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
```

Rest of the rules can be added based on what conventions the team decides to follow.

## Recommendations

We would suggest adding more of auto-fixable rules as the corrections suggested by these rules can be fixed by the editor with an ESLint plugin on file save. This would reduce the time that a developer would spend fixing lint errors than writing actual code.

We also recommend using ESLint for spacing/tabs instead of other methods like `.editorconfig`. This way, all the code conventions can be configured via a single utility **\(eslint\)** and the fixes can be made by the editor itself on save.

The code till here can be found on the **branch** [chapter/6/6.1](https://github.com/react-made-native-easy/note-taker/tree/chapter/6/6.1)

