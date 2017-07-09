## Environment variables

Since react-native is combines native platform code with JS, you might find it hard to accomplish small tasks like passing environment variables. But don't worry, we got you covered here as well 🙌

### How Environment variables work in react-native

Since the JS code is being executed by the native platform, we cannot directly pass env variables like we used to do in node/web pack projects. So we found an alternative way to accomplish this.

### How to use Environment variables in RN
We created an `env.config.js` file which exports an object. The env object can contain the API base url, environment, and even the app fixtures/mock api data, etc (we will discuss this in details in [API mocks](./api-mocks.md) section). And on the CI (travis in our case), we change the contents of the file. Sounds cool right? 🤓 

The app will also contain all the env configs inside `env/` folder. The CI will replace the contents of `env.config.js` with `env/prod.config.js` before starting the build process.
