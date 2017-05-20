## Testing stateful components using Enzyme

We talked about testing presentational components using our beloved feature called Snaphot testing in Jest. But It just tests the UI of the component(just the render method of your component).

_What if your component contains some class methods? What if your component contains state?_

Thats where we use __enzyme__.

### What's enzyme?

Enzyme is a JavaScript Testing utility for React. You will mostly be using shallow utility from enzyme. Shallow utility helps us rendering a component and allows us accessing the class methods/state of the component.

### Integrating Enzyme in your current Jest Framework

The default react-native boilerplate comes with Jest. Integrating enzyme with Jest is just a two step process.

- Install enzyme and jest-enzyme `yarn add enzyme jest-enzyme --dev`

- Add one line in package.json inside jest config:
    `"setupTestFrameworkScriptFile": "./node_modules/jest-enzyme/lib/index.js",`

Thats it. You can start using Enzyme utilities now.

### Using Shallow renderer from enzyme:

- First we need to shallow render our component.

```js
import {shallow} from 'enzyme';
describe('SomeComponent component', () => {
  it('Shallow rendering', () => {
    const wrapper = shallow(<SomeComponent {..props}/>);
  });
});
```

Now Our component is rendered and we can access props/state/methods using `wrapper`. Here is how you access them:


```js
import {shallow} from 'enzyme';
describe('SomeComponent component', () => {
  it('Shallow rendering', () => {
    const wrapper = shallow(<SomeComponent someProp={1}/>);
    const componentInstance = wrapper.instance();
    //Accessing react lifecyle methods
    componentInstance.componentDidMount();
    componentInstance.componentWillMount();
    //Accessing component state
    expect(wrapper.state('someStateKey')).toBe(true);
    //Accessing component props
    expect(wrapper.props.someProp).toEqual(1);
    //Accessing class methods
    expect(componentInstance.counter(1)).toEqual(2);
  });
});
```

As you saw, you can access everything a component possess using shallow utitity. You can also have a look at the example test case in our boilerplate code [here]().
