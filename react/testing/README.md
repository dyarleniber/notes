[Back](../README.md)

## Testing

- Tools
  - Jest
    - Jest is a Testing Framework (usually used as a test runner).
    - [Documentation](https://jestjs.io/en/)
  - Enzyme
    - Enzyme is a Testing utility for React to test React Components' output.
    - [Documentation](https://enzymejs.github.io/enzyme/)

Create React App uses Jest as its test runner.

[This is the Create React App Testing documentation.](https://create-react-app.dev/docs/running-tests/)

> Snapshot Testing is a feature of Jest that automatically generates text snapshots of your components and saves them on the disk so if the UI output changes, you get notified without manually writing any assertions on the component output. [Read more about snapshot testing.](https://jestjs.io/docs/en/snapshot-testing)

When you run npm test, Jest will launch in watch mode. Every time you save a file, it will re-run the tests, like how npm start recompiles the code.You can disable this behavior by passing in the --watchAll=false flag.

> Jest has an integrated coverage reporter that works well with ES6 and requires no configuration.

- Run the tests using the following command
```shell
npm test --watchAll=false
```

- Run the tests including a coverage report using the following command
```shell
npm test -- --coverage --watchAll=false
```

### Testing with React/Redux

Because most of the Redux code you write are functions, and many of them are pure, they are easy to test without mocking.

- Examples of testing with Redux
  - https://redux.js.org/recipes/writing-tests

- Best practices for testing with Redux
  - https://willowtreeapps.com/ideas/best-practices-for-unit-testing-with-a-react-redux-approach
  - https://jsramblings.com/3-ways-to-test-mapstatetoprops-and-mapdispatchtoprops/

### Testing with Hooks

#### Custom Hooks

[react-hooks-testing-library](https://github.com/testing-library/react-hooks-testing-library) is a very used React hooks testing utilities.

- When to use this library
  - You're writing a library with one or more custom hooks that are not directly tied a component
  - You have a complex hook that is difficult to test through component interactions
- When not to use this library
  - Your hook is defined alongside a component and is only used there
  - Your hook is easy to test by just testing the components using it

[Installation and Code samples](https://react-hooks-testing-library.com/)

#### Function component

react-hooks-testing-library is useful for testing custom hooks, in order to testing hooks inside a function component, a useful approach would be test the side effects simulating events like clicks.

Example:

```jsx
it('should set the password value on change event with trim', () => {
    container.find('input[type="password"]').simulate('change', {
      target: {
        value: 'somenewpassword  ',
      },
    });
    expect(container.find('input[type="password"]').prop('value')).toEqual(
      'somenewpassword',
    );
  });
```

An alternative to simulating events using simulate method is to execute the props by calling them as functions by passing in the necessary params. It is useful when we have a custom component with custom methods as props, in these cases the simulate method wouldn't work.

```jsx
container.find('input[type="password"]').prop('onChange')({
  target: {
    value: 'somenewpassword',
  }
});
```

> Alternatively, we could also mock the Hooks using Jest.

##### Lifecycle hooks

Lifecycle hooks such as useEffect aren't yet supported in shallow render (those hooks don't get called) so we need to use mount instead of shallow to test those components for now. Like with the useState hook we check for updates to props to test these hooks by simulating events or executing props as functions.

##### Methods that don't update state

The methods that don't manipulate the state can be refactored out of the component into a separate utils file and tested in it instead of having them inside the component. If the methods are pretty specific to the component and aren't shared outside the component we could have it inside the component file but outside the main function component.

#### Hooks from Redux

There are 2 kinds of hooks you will encounter.

- Separated custom hooks with / without jsx
- Component with hooks inside

And the essential concepts are: the first one is a unit test method, the 2nd solution is an integration test.

##### How to test separated custom hooks

Let's say you have a custom hook function:

```jsx
import { useSelector, useDispatch } from "react-redux";
import { Selectors } from "./selectors";
import { Actions } from "./actions";

export const useReset = () => {
  const totalCost = useSelector(Selectors.totalCost);
  const dispatch = useDispatch();

  return () => {
    if (totalCost > 0) {
      dispatch(Actions.reset());
    }
  };
};
```

It is simple, we will dispatch Actions.reset() when totalCost is greater than 0.

For the testing this, you can simply monkeypatch all the methods that you are using here in terms of changing the behavior when testing.

```jsx
import { useReset } from "./useReset";
import { Selectors } from "./selectors";
import { Actions } from "./actions";

jest.mock("react-redux", () => ({
  useSelector: jest.fn(fn => fn()),
  useDispatch: () => jest.fn()
}));

const setup = ({ totalCost }) => {
  jest.spyOn(Selectors, "totalCost").mockReturnValue(totalCost);
  jest.spyOn(Actions, "reset");
};

describe("useReset", () => {
  afterEach(() => {
    jest.clearAllMocks();
  });

  afterAll(() => {
    jest.restoreAllMocks();
  });

  test("Success Case", () => {
    setup({ totalCost: 1 });

    const resetFunc = useReset();
    resetFunc();

    expect(Actions.reset).toHaveBeenCalledTimes(1);
  });

  test("Failure Case", () => {
    setup({ totalCost: 0 });

    const resetFunc = useReset();
    resetFunc();

    expect(Actions.reset).toHaveBeenCalledTimes(0);
  });
});
```

##### How to test component with hooks inside

In this case, you can simply create a fake store <Provider> from the lib redux-mock-store, and use it to wrap your component, so every test is more like an integration test involved not only the Components but the selectors as well.

[redux-mock-store](https://github.com/reduxjs/redux-mock-store)

```jsx
import React from 'react';
import { mount } from 'enzyme';
import configureStore from 'redux-mock-store';
import { Provider } from 'react-redux';

import Header from '../../components/Header';
import { addTodo } from '../../store/actions/todos';

const middlewares = [];
const mockStore = configureStore(middlewares);
const initialState = { };
const store = mockStore(initialState);
const wrapper = mount(
  <Provider store={store}>
    <Header />
  </Provider>,
);

describe('Header component', () => {
  it('should render without crashing', () => {
    expect(wrapper).toMatchSnapshot();
  });

  it('should change the state', () => {
    const input = 'Input example';
    wrapper.find('[id="textInput"]').first().simulate('change', { target: { value: input } });
    expect(wrapper.find('[id="textInput"]').first().prop('value')).toEqual(input);
  });

  it('should add a new todo', () => {
    const input = 'Input example';
    wrapper.find('[id="textInput"]').first().simulate('change', { target: { value: input } });
    wrapper.find('[id="addForm"]').first().simulate('submit', {
      preventDefault: jest.fn(),
      target: { value: input },
    });
    const actions = store.getActions();
    const text = input.trim();
    const expectedPayload = addTodo(text);
    expect(actions).toEqual([expectedPayload]);
    store.clearActions();
    expect(wrapper.find('[id="textInput"]').first().prop('value')).toEqual('');
  });
});
```

#### References

- https://react-hooks-testing-library.com/
- https://medium.com/@acesmndr/testing-react-functional-components-with-hooks-using-enzyme-f732124d320a
- https://dev.to/theactualgivens/testing-react-hook-state-changes-2oga
- https://medium.com/@pylnata/testing-react-functional-component-using-hooks-useeffect-usedispatch-and-useselector-in-shallow-9cfbc74f62fb
- https://www.albertgao.xyz/2019/11/05/how-to-test-react-redux-hooks-via-jest/
- https://github.com/reduxjs/redux-mock-store
