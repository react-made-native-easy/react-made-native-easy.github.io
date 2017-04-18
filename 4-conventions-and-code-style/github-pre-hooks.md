### Git Pre-push/Pre-commit Hooks

Pre-push/Pre-commit hooks are nothing but a couple of commands which you would want to run every time you push/commit something. Now it might not sound that interesting but using them with jest and eslint can protect the code quality of your code at a much, much higher level.

How you ask?

Pre hooks run a shell command which you specify and if the command fails with exit status 1, the process will get terminated. We store the shell commands inside `.git/hooks` directory of root of the project.  So imagine that you won't be able to push the code if your tests are failing or your js files are not properly linted. That sound's interesting right?

Now the next question might be, how do you make sure that everyone adds those commands to the pre-push hooks folder. Don't worry we got that covered as well. Let me introduce you to these awesome NPM modules called [`pre-push`]() [`pre-commit`]().

After installing these node-modules, all you need to do is specify which command you would like to execute before pushing/committing your code. And all this can be done within `package.json`. It will make sure to add the commands to your local git hooks folder while setting up (npm install) the machine.

We used `pre-push` hooks to make sure that every line of code which is being pushed is linted and all the tests pass.

If you look at the package.json file of our boilerplate, you would find this sinppet:
```
"pre-push": [
  "lint",
  "test"
]
```
As you can see, we are doing `npm run lint` and `npm run test` everytime the developer tries to push something. If any of the command fails, developer would not be allowed to push and you which greatly reduces the chances of build failure/PR check failure in CIs.
