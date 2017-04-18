#### Utilizing the Power of ESLINT for consistent code

You have to trust us on this, ESLINT can increase your speed of development exponentially by making sure that the coding style is consistent throughout the project.

Most of the editors support ESLINT plugin and the plugin has a feature called `lint on save`. It fixes the auto fixable lint errors the moment you save (cmd+s) a file. You can find all the rules [here](http://eslint.org/docs/rules/).

Now you have to use this carefully. If you try to use everything or if you extend a plugin and use all the rules, you might end up spending hell lot of time fixing lint than writing code. So we suggest you to use rules which are auto fixable.

We wrote our ESLINT config file ourself and you can also write it yourself or refer to the file that we created. We recommend everyone should have at least these rules. It contains all the minimalistic things required. You can of course change it as per your needs. The best part is that all of our rules are auto fixable.

Some of my favorite rules are:

- `react/no-did-mount-set-state`
- `react/no-direct-mutation-state`
- `react/prop-types`
- `react/jsx-no-bind`
- `react/jsx-no-duplicate-props`

We also suggest using eslint for spacing/tabs instead of `.editorconfig`. This way, all the configuration level stuff would be coming from a single config file. and we can use the power of eslint auto fix to lint/indent file on save.
