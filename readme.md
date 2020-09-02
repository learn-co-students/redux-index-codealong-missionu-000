# Displaying a List of items with Redux

## Objectives

With this lesson we'll finish up what we worked on the in the forms code along 
by displaying our list of todos. By the end of this lesson, you will be able to:

* Display a list of elements from our __Redux__ store

## Goal

Our state is properly updating but we are not displaying these updates to the 
user. We need a component that references the store and then uses the data from 
the store to render the list of Todos.

## Displaying todos

The `CreateTodo` component is handling the creation side of things, so let's make 
a new component where we'll be getting todos from the store. We'll call this 
`TodosContainer` and connect it to __Redux__.


```js
// ./src/components/todos/TodosContainer.js

import React, { Component } from 'react';
import { connect } from 'react-redux';

class TodosContainer extends Component {

  render() {
    return(
      <ol></ol>
    );
  }
};

export default connect()(TodosContainer);
```

Next we need to get the list of todos from our __Redux__ state, so we'll need 
to write out a `mapStateToProps()` function and include it as an argument for 
`connect()`:

```js
...
const mapStateToProps = state => {
  return {
    todos: state.todos
  }
}

export default connect(mapStateToProps)(TodosContainer);
```

We can confirm this is working by adding a log in the render of TodosContainer. 
We have already imported and rendered the TodosContainer in our App component.

Now that we have a way to get data from __Redux__, we can create a presentational 
component to handle displaying our todos.

## Creating a Presentational Todo Component

To start, we'll have each todo rendered as a list item. Open the `Todo.js` file 
which is inside the `./src/components/todos` folder. Inside it, let's write a 
functional component that returns an `li` displaying props:

```js
// ./src/components/todos/Todo.js

import React from 'react';

const Todo = props => {
  return (
    <li>{props.text}</li>
  );
};

export default Todo;
```

Then, in our __TodosContainer__ component, we'll import our `Todo` component, 
then map through the list of todos we're getting from state and render the `Todo` 
component for each of them:

```js
// ./src/components/todos/TodosContainer.js

import React, { Component } from 'react';
import { connect } from 'react-redux';
import Todo from './Todo';

class TodosContainer extends Component {

  renderTodos = () => this.props.todos.map((todo, id) => <Todo key={id} text={todo} />)

  render() {
    return(
      <ol>
        {this.renderTodos()}
      </ol>
    );
  }
};

const mapStateToProps = state => {
  return {
    todos: state.todos
  }
}

export default connect(mapStateToProps)(TodosContainer);


```

Now our TodosContainer is mapping over the todos it received from __Redux__, 
and passing the value of each todo into a child component, Todo. Todo in this 
case doesn't have any __Redux__ related code, and is a regular, functional 
component.

## Cleanup Todo Input

Currently when we enter a todo and click Submit, the text we entered remains in 
the input field; let's fix that. We can do that inside our __handleSubmit__ 
function. We can reset the *component's* state each time the submit button is 
clicked by changing our function to the following:

```js
// ./src/components/todos/CreateTodo.js

...

handleSubmit = event => {
  event.preventDefault();
  this.props.addTodo(this.state)
  this.setState({
    text: '',
  })
}


...
```

This will cause the page to rerender, and update the value attribute of our input 
field, clearing out the contents.

That's it! We've got a working app that takes in form data and displays it in a list.

## Summary

We now have a fully functioning todo application. In the previous lesson, we 
built our `CreateTodo` component, which contains a simple form for adding a 
todo. We made it a controlled form by creating component state and writing an 
`onChange` handler that updates the component's state as the user types in their 
todo. We also wired the text in the component state to the input field by 
setting its `value` attribute to `this.state.text`. 

Next, we connected our `CreateTodo` component to Redux, using the `connect` 
function. We created a `mapDispatchToProps` function to provide an `addTodo` 
function to our component. The `addTodo` takes the form data as an argument, 
creates the action object and dispatches it to the reducer. We then created an 
`onSubmit` handler that calls `addTodo` when the form is submitted, passing in 
the component state as an argument.

Finally, we built our reducer to take the action and concat the new todo into 
our Redux state.

Then, in this lesson, we got our __Todos__ component working simply by accessing 
the state from the store, and then iterating through the list of todos and 
rendering the __Todo__ component for each one.


## References

- [React Documentation - Controlled Components](https://facebook.github.io/react/docs/forms.html)
