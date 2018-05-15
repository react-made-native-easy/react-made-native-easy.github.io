# Testing stateful components using Enzyme

We talked about testing presentational components using our beloved feature Snapshot testing in Jest. But it only tests the UI of the component \(the render method\).

_What if your component contains some class methods? What if your component contains state?_

That's where we use **Enzyme**.

## What's Enzyme?

Enzyme is a JavaScript testing utility for React. You will mostly be using the `shallow` utility from Enzyme. Shallow utility helps us in rendering a component and allowing us access to the class methods/state of the component.

## Integrating Enzyme in your current Jest Framework

The default react-native boilerplate comes with Jest. Integrating Enzyme with Jest is just a two-step process.

* Install enzyme, jest-enzyme, enzyme-adapters `yarn add enzyme jest-enzyme enzyme-adapter-react-16 enzyme-react-16-adapter-setup --dev`
* Setup enzyme-react-adaptor and jest-enzyme. Your new package.json should look something like this:

  > package.json

  ```javascript
    {
      ...,
      "jest": {
        ...,
        "setupTestFrameworkScriptFile": "./node_modules/jest-enzyme/lib/index.js",
        "setupFiles": ["enzyme-react-16-adapter-setup"]
      }
    }
  ```

That's it. You can start using Enzyme utilities now.

## Using Shallow renderer from Enzyme:

First, we need to shallow render our component.

```javascript
import {shallow} from 'enzyme';
describe('SomeComponent component', () => {
  it('Shallow rendering', () => {
    const wrapper = shallow(<SomeComponent {..props}/>);
  });
});
```

Now, our component is rendered and we can access props/state/methods using `wrapper`. Here is how you can access them:

```javascript
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

As you can see, you can access everything a component possesses using shallow utitity. You can also have a look at the example test case in our boilerplate code [here](testing-stateful-components-using-enzyme.md).

## Example

Let's take an example of a component with state and class methods. We will write test cases for the methods including a snapshot test. The example includes testing class methods, state, and props.

```javascript
import React from 'react';
import renderer from 'react-test-renderer';
import {shallow} from 'enzyme';
import Counter from '../Counter.component';

describe('Counter component', () => {
  it('Counter: renders correctly', () => {
    const tree = renderer.create(<Counter />).toJSON();
    expect(tree).toMatchSnapshot();
  });
  it('componentWillMount: should set the passed initialCountValue to state', () => {
    const wrapper = shallow(<Counter initialCountValue={2}/>);
    expect(wrapper.instance().state.count).toBe(2);
  });
  it('incrementCounter: should increment state.count by 1', () => {
    const wrapper = shallow(<Counter initialCountValue={0}/>);
    const instance = wrapper.instance();
    expect(instance.state.count).toBe(0);
    instance.incrementCounter();
    expect(instance.state.count).toBe(1);
  });
  it('decrementCounter: should decrement state.count by 1', () => {
    const wrapper = shallow(<Counter initialCountValue={1}/>);
    const instance = wrapper.instance();
    expect(instance.state.count).toBe(1);
    instance.decrementCounter();
    expect(instance.state.count).toBe(0);
  });
  it('should call props on increment/decrement', () => {
    const incrementSpy = jest.fn();
    const decrementSpy = jest.fn();
    const wrapper = shallow(<Counter initialCountValue={1} onIncrement={incrementSpy} onDecrement={decrementSpy}/>);
    const instance = wrapper.instance();
    instance.incrementCounter();
    expect(incrementSpy).toBeCalledWith(2);
    instance.decrementCounter();
    expect(decrementSpy).toBeCalledWith(1);
  });
});
```

