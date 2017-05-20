## Snapshots

This features makes testing presentational components a lot easier. With a single line, you can test all your presentational components (their render method only). No need to write test cases for each component returned by render method.

### What are Snapshots:

A snapshot is nothing but a configuration file defining your component style, UI and props. The test case will look somthing like this:

`__tests__/someComponent.component.test.js`

```js
import React from 'react';
import renderer from 'react-test-renderer';
import SomeComponent from '../SomeComponent.component';

describe('Some component', () => {
  it('renders correctly', () => {
    const tree = renderer.create(
      <SomeComponent/>
    ).toJSON();
    expect(tree).toMatchSnapshot();
  });
});
```

Whenever Jest sees this line `expect(tree).toMatchSnapshot();`, it is going to generate a snapshot and compare it with stored snapshot. If the snapshot is not present, Jest will store the generated snap.

The generated snap file will look something like this:

`__tests__/snaphots/someComponent.component.js.snap`
```
exports[`SomeComponent Component: SomeComponent renders correctly 1`] = `
<View
  style={
    Object {
      "flex": 1,
    }
  }
>
  <Text
    accessible={true}
    allowFontScaling={true}
    ellipsizeMode="tail"
    style={
      Object {
        "color": "#000000",
        "fontFamily": "Roboto",
        "fontSize": 24,
        "fontWeight": "500",
        "paddingVertical": 20,
        "textAlign": "center",
      }
    }
  >
    SomeText
  </Text>
</View>
`;
```

As you can see above, the snap contains every possible property of the UI which is being returned by the render method.


### Should I push generated snaps to git?

Yes you should. Snaps should be there in each dev's machine so that if one of the devs changes some other component unknowingly, the snap test for that component will fail and he/she would know before pushing it.
Even the Jest official documentation says this:

>It is expected that all snapshots are part of the code that is run on CI and since new snapshots automatically pass, they should not pass a test run on a CI system. It is recommended to always commit all snapshots and to keep them in version control.

### What to do when snap test fails?

Consider this scenario. You worked on a component, generated a snap and pushed it. Later another dev named John made some change in the component. Now the test of the snap will fail since snap still contains the code which you wrote. John will just need to update the snapshot to make the test pass. No need to update the test case, just one command: `jest --updateSnapshot` and you are done.

*We recommend creating an __npm script__ for updating snaps. As you can see in the package.json of our boiler plate,  it conatains a command called "test:update". This command go through all the test cases and will update the snap whenever it is required.*

__More information can be found here:__ [https://facebook.github.io/jest/docs/en/snapshot-testing.html#content](https://facebook.github.io/jest/docs/en/snapshot-testing.html#content)
