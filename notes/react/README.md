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

> [Create React App](https://create-react-app.dev/) is a comfortable environment for learning React, and is the best way to start building a new single-page application in React.

## Notes

- [Hooks](hooks/README.md)
- [Redux](redux/README.md)
- [Testing](testing/README.md)
