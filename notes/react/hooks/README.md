## Hooks

[Hooks](https://reactjs.org/docs/hooks-intro.html) are a new addition in React 16.8. They let you use state and other React features without writing a class.

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
