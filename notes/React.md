# React

React is a JavaScript library for building user interfaces.

Example:

```jsx
class TodoApp extends React.Component {
  constructor(props) {
    super(props);
    this.state = { items: [], text: '' };
    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  render() {
    return (
      <div>
        <h3>TODO</h3>
        <TodoList items={this.state.items} />
        <form onSubmit={this.handleSubmit}>
          <label htmlFor="new-todo">
            What needs to be done?
          </label>
          <input
            id="new-todo"
            onChange={this.handleChange}
            value={this.state.text}
          />
          <button>
            Add #{this.state.items.length + 1}
          </button>
        </form>
      </div>
    );
  }

  handleChange(e) {
    this.setState({ text: e.target.value });
  }

  handleSubmit(e) {
    e.preventDefault();
    if (this.state.text.length === 0) {
      return;
    }
    const newItem = {
      text: this.state.text,
      id: Date.now()
    };
    this.setState(state => ({
      items: state.items.concat(newItem),
      text: ''
    }));
  }
}

class TodoList extends React.Component {
  render() {
    return (
      <ul>
        {this.props.items.map(item => (
          <li key={item.id}>{item.text}</li>
        ))}
      </ul>
    );
  }
}

ReactDOM.render(
  <TodoApp />,
  document.getElementById('todos-example')
);
```

## [The Component Lifecycle](https://reactjs.org/docs/react-component.html)

Each component has several lifecycle methods that you can override to run code at particular times in the process.

- Mounting
	- constructor()
		- The constructor for a React component is called before it is mounted.
	- render()
		- The render() method is the only required method in a class component.
	- componentDidMount()
		- It is invoked immediately after a component is mounted (inserted into the tree).
- Updating
	- shouldComponentUpdate()
		- Use it to let React know if a component's output is not affected by the current change in state or props.
	- render()
	- componentDidUpdate()
		- It is invoked immediately after updating occurs. This method is not called for the initial render.
- Unmounting
	- componentWillUnmount()
		- It is invoked immediately before a component is unmounted and destroyed.


## [Create React App](https://create-react-app.dev/)

Create React App is a comfortable environment for learning React, and is the best way to start building a new single-page application in React.


## [Hooks](https://reactjs.org/docs/hooks-intro.html)

Hooks are a new addition in React 16.8. They let you use state and other React features without writing a class.

### Motivation

Problems that hooks solve:

- It's hard to reuse stateful logic between components
	- Patterns like render props and higher-order components that try to solve this. But these patterns require you to restructure your components when you use them, which can be cumbersome and make code harder to follow.
	- With Hooks, you can extract stateful logic from a component so it can be tested independently and reused.
	- Hooks allow you to reuse stateful logic without changing your component hierarchy.
- Complex components become hard to understand
	- We have often had to maintain components that started out simple but grew into an unmanageable mess of stateful logic and side effects. Each lifecycle method often contains a mix of unrelated logic.
	- To solve this, Hooks let you split one component into smaller functions based on what pieces are related (such as setting up a subscription or fetching data).
- Classes confuse both people and machines
	- Classes make code reuse and code organization more difficult.
	- To solve these problems, Hooks let you use more of React's features without classes.

### Main Hooks

#### useState

Returns a stateful value, and a function to update it.

```jsx
const [state, setState] = useState(initialState);
```

Example:
```jsx
function Counter({initialCount}) {
  const [count, setCount] = useState(initialCount);
  return (
    <>
      Count: {count}
      <button onClick={() => setCount(initialCount)}>Reset</button>
      <button onClick={() => setCount(prevCount => prevCount - 1)}>-</button>
      <button onClick={() => setCount(prevCount => prevCount + 1)}>+</button>
    </>
  );
}
```

> Unlike the setState method found in class components, useState does not automatically merge update objects. You can replicate this behavior by combining the function updater form with object spread syntax:
```jsx
setState(prevState => {
  // Object.assign would also work
  return {...prevState, ...updatedValues};
});
```
> Another option is useReducer, which is more suited for managing state objects that contain multiple sub-values.

##### Lazy initial state

```jsx
const [state, setState] = useState(() => {
  const initialState = someExpensiveComputation(props);
  return initialState;
});
```

#### useEffect

Accepts a function that contains imperative, possibly effectful code.

```jsx
useEffect(didUpdate);
```

The function passed to useEffect will run after the render is committed to the screen. 
By default, effects run after every completed render, but you can choose to fire them only when certain values have changed.

##### Cleaning up an effect

Often, effects create resources that need to be cleaned up before the component leaves the screen, such as a subscription or timer ID. To do this, the function passed to useEffect may return a clean-up function. For example, to create a subscription:

```jsx
useEffect(() => {
  const subscription = props.source.subscribe();
  return () => {
    // Clean up the subscription
    subscription.unsubscribe();
  };
});
```

The clean-up function runs before the component is removed from the UI to prevent memory leaks.
Additionally, if a component renders multiple times (as they typically do), the previous effect is cleaned up before executing the next effect.

##### Timing of effects

Unlike componentDidMount and componentDidUpdate, the function passed to useEffect fires after layout and paint, during a deferred event. (Most types of work shouldn't block the browser from updating the screen).

However, not all effects can be deferred. For example, a DOM mutation that is visible to the user must fire synchronously before the next paint so that the user does not perceive a visual inconsistency.

For these types of effects, React provides one additional Hook called useLayoutEffect. It has the same signature as useEffect, and only differs in when it is fired.

##### Conditionally firing an effect

The default behavior for effects is to fire the effect after every completed render. That way an effect is always recreated if one of its dependencies changes.

However, this may be overkill in some cases, like the subscription example from the previous section. We don't need to create a new subscription on every update, only if the source prop has changed.

To implement this, pass a second argument to useEffect that is the array of values that the effect depends on. Our updated example now looks like this:

```jsx
useEffect(
  () => {
    const subscription = props.source.subscribe();
    return () => {
      subscription.unsubscribe();
    };
  },
  [props.source],
);
```

Now the subscription will only be recreated when props.source changes.

> If you use this optimization, make sure the array includes all values from the component scope (such as props and state) that change over time and that are used by the effect.

> If you want to run an effect and clean it up only once (on mount and unmount), you can pass an empty array ([]) as a second argument. 

> Passing [] as the second argument is closer to the familiar componentDidMount and componentWillUnmount mental model.

> We recommend using the exhaustive-deps rule as part of our eslint-plugin-react-hooks package. It warns when dependencies are specified incorrectly and suggests a fix.

The array of dependencies is not passed as arguments to the effect function. Conceptually, though, that's what they represent: every value referenced inside the effect function should also appear in the dependencies array.

#### useReducer

```jsx
const [state, dispatch] = useReducer(reducer, initialArg, init);
```

- An alternative to useState. Accepts a reducer of type (state, action) => newState, and returns the current state paired with a dispatch method.

Here's the counter example from the useState section, rewritten to use a reducer:

```jsx
const initialState = {count: 0};

function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return {count: state.count + 1};
    case 'decrement':
      return {count: state.count - 1};
    default:
      throw new Error();
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, initialState);
  return (
    <>
      Count: {state.count}
      <button onClick={() => dispatch({type: 'decrement'})}>-</button>
      <button onClick={() => dispatch({type: 'increment'})}>+</button>
    </>
  );
}
```

> React doesn't use the state = initialState argument convention popularized by Redux. The initial value sometimes needs to depend on props and so is specified from the Hook call instead. If you feel strongly about this, you can call useReducer(reducer, undefined, reducer) to emulate the Redux behavior, but it's not encouraged.

### Building Your Own Hooks

Building your own Hooks lets you extract component logic into reusable functions.

When we want to share logic between two JavaScript functions, we extract it to a third function. Both components and Hooks are functions, so this works for them too!

```jsx
import { useState, useEffect } from 'react';

function useFriendStatus(friendID) {
  const [isOnline, setIsOnline] = useState(null);

  useEffect(() => {
    function handleStatusChange(status) {
      setIsOnline(status.isOnline);
    }

    ChatAPI.subscribeToFriendStatus(friendID, handleStatusChange);
    return () => {
      ChatAPI.unsubscribeFromFriendStatus(friendID, handleStatusChange);
    };
  });

  return isOnline;
}
```

#### Using a Custom Hook

```jsx
function FriendStatus(props) {
  const isOnline = useFriendStatus(props.friend.id);

  if (isOnline === null) {
    return 'Loading...';
  }
  return isOnline ? 'Online' : 'Offline';
}
```

#### Pass Information Between Hooks

Since Hooks are functions, we can pass information between them.

```jsx
const friendList = [
  { id: 1, name: 'Phoebe' },
  { id: 2, name: 'Rachel' },
  { id: 3, name: 'Ross' },
];

function ChatRecipientPicker() {
  const [recipientID, setRecipientID] = useState(1);
  const isRecipientOnline = useFriendStatus(recipientID);

  return (
    <>
      <Circle color={isRecipientOnline ? 'green' : 'red'} />
      <select
        value={recipientID}
        onChange={e => setRecipientID(Number(e.target.value))}
      >
        {friendList.map(friend => (
          <option key={friend.id} value={friend.id}>
            {friend.name}
          </option>
        ))}
      </select>
    </>
  );
}
```

We keep the currently chosen friend ID in the recipientID state variable, and update it if the user chooses a different friend in the select picker.

Because the useState Hook call gives us the latest value of the recipientID state variable, we can pass it to our custom useFriendStatus Hook as an argument:

### Using Hooks with Redux

React Redux now offers a set of hook APIs as an alternative to the existing connect() Higher Order Component. These APIs allow you to subscribe to the Redux store and dispatch actions, without having to wrap your components in connect().

As with connect(), you should start by wrapping your entire application in a <Provider> component to make the store available throughout the component tree:

```jsx
const store = createStore(rootReducer)

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
)
```

React Redux now includes its own useSelector and useDispatch Hooks that can be used instead of connect.

- useSelector is analogous to connect's mapStateToProps. You pass it a function that takes the Redux store state and returns the pieces of state you're interested in.

- useDispatch replaces connect's mapDispatchToProps but is lighter weight. All it does is return your store's dispatch method so you can manually dispatch actions.

Example using connect:

```jsx
import React from "react";
import { connect } from "react-redux";
import { addCount } from "./store/counter/actions";

export const Count = ({ count, addCount }) => {
  return (
    <main>
      <div>Count: {count}</div>
      <button onClick={addCount}>Add to count</button>
    </main>
  );
};

const mapStateToProps = state => ({
  count: state.counter.count
});

const mapDispatchToProps = { addCount };

export default connect(mapStateToProps, mapDispatchToProps)(Count);
```

Now, with the new React Redux Hooks instead of connect:

```jsx
import React from "react";
import { useDispatch, useSelector } from "react-redux";
import { addCount } from "./store/counter/actions";

export const Count = () => {
  const count = useSelector(state => state.counter.count);
  const dispatch = useDispatch();

  return (
    <main>
      <div>Count: {count}</div>
      <button onClick={() => dispatch(addCount())}>Add to count</button>
    </main>
  );
};
```

#### useSelector gotchas

useSelector diverges from mapStateToProps in one fairly big way: it uses strict object reference equality (===) to determine if components should re-render instead of shallow object comparison.

For example, in this snippet:

```jsx
const { count, user } = useSelector(state => ({
  count: state.counter.count,
  user: state.user,
}));
```

useSelector is returning a different object literal each time it's called. When the store is updated, React Redux will run this selector, and since a new object was returned, always determine that the component needs to re-render, which isn't what we want.

The simple rule to avoid this is to either call useSelector once for each value of your state that you need:

```jsx
const count = useSelector(state => state.counter.count);
const user = useSelector(state => state.user);
```

or, when returning an object containing several values from the store, explicitly tell useSelector to use a shallow equality comparison by passing the comparison method as the second argument:

```jsx
import { shallowEqual, useSelector } from 'react-redux';

const { count, user } = useSelector(state => ({
  count: state.counter.count,
  user: state.user,
}), shallowEqual);
```

#### References

- https://thoughtbot.com/blog/using-redux-with-react-hooks
- https://react-redux.js.org/api/hooks


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

#### References

- https://react-hooks-testing-library.com/
- https://medium.com/@acesmndr/testing-react-functional-components-with-hooks-using-enzyme-f732124d320a
- https://dev.to/theactualgivens/testing-react-hook-state-changes-2oga
- https://medium.com/@pylnata/testing-react-functional-component-using-hooks-useeffect-usedispatch-and-useselector-in-shallow-9cfbc74f62fb


## Redux

Redux is a tool for managing state in JavaScript applications.

In React, the concept of props being passed from parent components down to child components is called flow. In a real application there could be many nested components.
React doesn't recommend direct component-to-component communication this way. It's considered poor practice by many because direct component-to-component communication is error prone and leads to spaghetti code — an old term for code that is hard to follow.

This is where Redux comes in handy. Redux offers a solution of storing all your application state in one place, called a store.

![Redux flow](https://css-tricks.com/wp-content/uploads/2016/03/redux-article-3-03.svg)

The general concept of using store(s) to coordinate application state is a pattern known as the Flux pattern.

### Three Principles

Redux can be described in three fundamental principles:

1. Single Source of Truth
Redux uses only one store for all its application state. Since all state resides in one place, Redux calls this the single source of truth.
2. State is Read-Only
According to Redux docs, "The only way to mutate the state is to emit an action, an object describing what happened." This means the application cannot modify the state directly. Dispatching an action is the only way for the application code to express a state change.
3. Changes are made with Pure Functions
Reducers are functions that you write which handle dispatched actions and can actually change the state. A reducer takes in current state as an argument and can only modify the state by returning new state. Reducers should be written as "pure" functions, a term that describes a function with the following characteristics:
- It does not make outside network or database calls.
- Its return value depends solely on the values of its parameters.
- Its arguments should be considered "immutable", meaning they should not be changed.
- Calling a pure function with the same set of arguments will always return the same value.

### Flux vs MVC

MVC is an design pattern for client side and server side applications also , And Flux is a new application architecture from Facebook that promises the same as MVC, but with a different approach that focuses on unidirectional data flow.

#### MVC

![MVC pattern](https://miro.medium.com/max/798/1*2lMcolCABWLZt3xDymIqDw.png)

- Model: manages the behavior and data of the application domain.
- View: represents the display of the model in the UI.
- Controller: takes user input, manipulates the model and causes the view to update.

##### Good Point in MVC

Great separation of code easy to maintain.

- Separating presentation from model should be improving testability.
- Separating view from controller most useful in web interfaces.

##### Bad Point in MVC

In server Side, MVC is good,but in Client side most of the JS frameworks provide data binding support which let the view can talk with model directly, It shoudn't be,Many times it become hard to debug something as there are scope for a property being changed by many ways.

![MVC in client side](https://miro.medium.com/max/780/1*sz1U9AUxtw5WnVNmS1jgww.png)

Facebook developers were facing problem scaling their MVC application and as a result of that the world got a new architecture flux. 

#### Flux

Flux is the application architecture that Facebook uses for building client-side web applications. It complements React's composable view components by utilizing a unidirectional data flow.

![Flux](https://miro.medium.com/max/1400/1*Ek68XwgLgxwlAZl6hhxx1w.png)

- Actions are simple objects with a type property and some data.

- Stores contain the application's state and logic. The best abstraction is to think of stores as managing a particular domain of the application. They aren't the same as models in MVC since models usually try to model single objects, while stores in Flux can store anything.

- The Dispatcher acts as a central hub. The dispatcher processes actions (for example, user interactions) and invokes callbacks that the stores have registered with it. The dispatcher isn't the same as controllers in the MVC pattern — usually the dispatcher does not have much logic inside it and you can reuse the same dispatcher across projects.

- Views are controller-views, also very common in most GUI MVC patterns. They listen for changes from the stores and re-render themselves appropriately. Views can also add new actions to the dispatcher, for example, on user interactions. The views are usually coded in React, but it's not necessary to use React with Flux.

### React-Redux "Flux" and Architecture

Dan Abramov felt that flux architecture could be simpler. Consequently, Dan Abramov & Andrew Clark developed Redux in 2015. Redux is a library, which implements the idea of Flux but in quite a different way. Redux architecture introduces new components like reducer and a centralized store.

#### Main differences

- MVC-type implementation enforces bidirectional data flows, which differs from the unidirectional data flow maintained in Flux and Redux.

- The primary difference of Flux vs Redux is that Flux includes multiple Stores per app, but Redux includes a single Store per app.

- The Store in the Flux handles all logic. In Redux, on the other hand, the Reducer handles all logic, which receives the previous state & one action, then returns the new state.

![Redux-flux](https://miro.medium.com/max/1400/1*QxZJEXWhsS-YuG5SZsRgjA.png)

#### To summarize:

- Action
	- An object setting up information about our data moving into state.
- Dispatch
	- Functions that act as a bridge between our actions and reducers, sending the information over to our application.
- Reducer
	- A function that receives data information from our action in the form of a type and payload and modifies the state based on additional provided data.
- State
	- Where all the data from our reducers are passed into a central source.
- Store
	- Once we have our actions and reducers set up, everything is maintained in our store. The store is where we can set up our initial state, combine our reducers, apply middleware, and hold our information.
- Provider
	- We can then use the provider to wrap our store around our application and by doing so, pass and render our state. We can then use connect with a component and enable it to receive store state from the provider.

### Code example

- [Todo project example](https://github.com/reduxjs/redux/tree/master/examples/todos)

### References

- https://css-tricks.com/learning-react-redux/
- https://redux.js.org/introduction/three-principles
- https://medium.com/@madasamy/flux-vs-mvc-design-pattern-de134dfaa12b
- https://www.clariontech.com/blog/mvc-vs-flux-vs-redux-the-real-differences
- https://medium.com/better-programming/a-simple-redux-tutorial-starter-complete-code-example-9b2923572d71
