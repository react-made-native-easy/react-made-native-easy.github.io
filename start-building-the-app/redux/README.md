# Redux 🗄

We assume that the reader is already aware of Redux. But just to recap, according to redux.js.org:

> Redux is a predictable state container for JavaScript apps. It helps you write applications that behave consistently, run in different environments \(client, server, and native\), and are easy to test. On top of that, it provides a great developer experience, such as live code editing combined with a time traveling debugger.

In a nutshell, Redux is a state container. That means, it will contain all our runtime application state or data.

Redux is essentially made up of three parts:

* Store - The store contains a global state for the entire app. It is basically the manager of the application state.
* Actions - These are the commands you pass to the store along with some data to modify the stored state.
* Reducers - Reducers are functions that the store calls whenever an action arrives. The reducers determine what the new state will be based on the action and the action payload they receive.

## But we already have React's state

Both Redux and React's state are used to manage the state in the application. But, both Redux and React's state are very different in the way they work and it is good to know when to use what.

React state is stored locally within a component. When it needs to be shared with other components, it is passed down through the props. This means that all the components which need the state data need to be children of the component holding the value. But in case of Redux, the state is stored globally in the Redux store. Components subscribe to the store to get access to the value. This centralizes all data and makes it very easy for a component to get the state it needs, without surrounding components being affected.

So, does this means that Redux is great and we should use it for all our app state management?

**NO!**

While Redux is helpful in some cases, it will create unnecessary indirections for simpler and trivial use cases.

Consider that we have a text input. And since we are using Redux, we decide to use Redux to store all the changes in the text field in the Redux store. In Redux, for changing the state on text input, we will need to create an Action, write a reducer and then subscribe our component to the store so that it re-renders on every state change. This is bad! Why do we need to complicate things so much?

_Dan Abramov_ - The creator of Redux says you might actually not need Redux unless you have a plan to benefit from this additional indirection. In his blog at [https://medium.com/@dan\_abramov/you-might-not-need-redux-be46360cf367](https://medium.com/@dan_abramov/you-might-not-need-redux-be46360cf367), he clearly states that since Redux introduces an additional indirection to managing the state of the application, we should use it only if it benefits us.

Now, let's see when we should use Redux and when React's state is good enough.

One way to do this is based on the duration the data has to be stored. As _Tyler Hoffman_ explains in his blog post: [https://spin.atomicobject.com/2017/06/07/react-state-vs-redux-state/](https://spin.atomicobject.com/2017/06/07/react-state-vs-redux-state/),

Different pieces of state are persisted for different amounts of time. We can categorize data into:

* **Short term**: data that will change rapidly in your app
* **Medium term**: data that will likely persist for a while in your app
* **Long term**: data that should last between multiple app launches.

### Short term data

Short term data is data that changes very quickly and is needed to be stored only for small amount of time. For example, this includes the characters that a user types in a text field. This data is not needed once the user submits the form. Also, we will most likely not need to transfer this type of data to any other independent component. Such type of data clearly fits the use case for React's state.

### Medium term data

Medium term data needs to stick around while the user navigates through the app. This could be data loaded from an API or any changes that need to be persisted until a page refresh. This can also contain data that needs to be used by a component that is completely unrelated to the component that produced the data. As an example, consider that one of the components makes an API call on a button click to update the user's profile details. The data returned from the server needs to be stored and will be used by a completely unrelated profile screen. Now, if the data is stored in some global location, it will be far easier to access this data. Such type of use cases clearly fits Redux.

### Long term data

This is the data that should be persisted between page refreshes or when the user closes and reopens the app. Since the Redux store is created on app launch, this type of data should be stored somewhere else, for example Async Storage in the case of React Native.

