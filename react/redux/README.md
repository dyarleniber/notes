[Back](../README.md)

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
