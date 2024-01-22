# Redux Toolkit and State Management

## What is Redux?

Redux is a state management library for JavaScript applications, commonly used with React. It centralizes and manages the state of an application in a predictable way, making it easier to maintain and debug.

## Why Redux?

Redux is used in web development to manage the state of complex applications, providing a centralized store for data and enabling predictable state changes. It helps avoid prop-drilling by allowing components to access the global state directly. Redux's unidirectional data flow and immutability principles contribute to maintainability and debugging. It's particularly beneficial for large-scale applications with multiple components and complex state interactions, promoting a structured and scalable approach to state management.

## What is Redux Toolkit?

Redux Toolkit is the official opinionated toolset for efficient Redux development. It provides utilities to simplify common Redux patterns, aiming to streamline the code and reduce boilerplate.

## Difference between Redux and react-redux

Redux and react-redux are two different things. Redux is a separate library that gives the ability of state management. React-redux is a bridge between React and Redux. It provides a set of helper functions and components to seamlessly integrate Redux with React applications. React-Redux allows React components to connect to the Redux store, access state, dispatch actions, and update based on changes in the Redux store.

## Installation

To provide access to the Redux store throughout the application, wrap the app inside the `Provider` tag:

```javascript
import { Provider } from 'react-redux';
import store from './app/store.js';
ReactDOM.createRoot(document.getElementById('root')).render(
  <Provider store={store}>
    <App />
  </Provider>
);
```

# Redux Store and Reducers in Redux Toolkit

## Store

A store is a central and immutable state container that holds the entire state tree of your application. In simple words, the store acts as a frontend database like local storage.

### How to create a store in Redux Toolkit?

Using `configureStore()`:

```javascript
import { configureStore } from '@reduxjs/toolkit';
const store = configureStore({});
export default store;
```
# Reducers and Slices in Redux

## Reducers

All the actions corresponding to a particular task are defined inside Reducers. For example, in a basic todo application, the reducer can contain functions such as `addTodo()`, `removeTodo()`, `updateTodo()`, etc.

### Key Characteristics of Reducers:

#### Pure Functions:

Reducers are pure functions; they produce the same output for the same input and do not have side effects. They should not modify the existing state but return a new state object.

#### Action Handling:

Reducers specify how the state changes based on the type of action dispatched.

## Slice

In Redux Toolkit, a "slice" is a way to organize and encapsulate a piece of the Redux store's state and its corresponding reducer logic. Slices help modularize and simplify the management of different parts of the overall state in a Redux application.

A slice typically contains the name of the store, initial state of whatever the store contains, and reducers. Reducer functions in Redux Toolkit can be defined inside the slice function itself. To manipulate the values of the initial states, reducer functions are used.

### Creating a Slice using `createSlice()`

# Todo Application Slice

Given below is the slice of a basic Todo application with the ability to add and delete a task.

```javascript
import { createSlice, nanoid } from "@reduxjs/toolkit";

const initialState = {
   todos: [] // initially the state is empty
};

export const todoSlice = createSlice({
   name: "todo",
   initialState,
   reducers: {
       // State represents the current data or information in the Redux store
       // Action describes the type of action being performed.
       addTodo: (state, action) => {
           const todo = {
               id: nanoid(),
               text: action.payload
           };
           state.todos.push(todo);
       },
       removeTodo: (state, action) => {
           state.todos = state.todos.filter((todo) =>
               todo.id !== action.payload
           );
       },
       updateTodo: (state, action) => {
           state.todos = state.todos.map((todo) => {
               if (todo.id === action.payload.id) {
                   return {
                       ...todo,
                       text: action.payload.text
                   };
               }
               return todo;
           });
       }
   }
});

export const { addTodo, removeTodo, updateTodo } = todoSlice.actions;

export default todoSlice.reducer;
```



**Notice:** All the individual reducers are exported separately to gain their access individually in other components.

The default export is `todoSlice.reducer`, i.e., the whole reducer object containing different functions, is exported as a single file to give access to the store.

```javascript
const store = configureStore({
   reducer: todoReducer
})
```

# `useDispatch()` and `useSelector()`

## `useDispatch()`

The `dispatch` function is used to dispatch actions, triggering state changes in the store.

```javascript
import { useDispatch } from 'react-redux';
const dispatch = useDispatch();
```

# `useSelector()`

The `useSelector` hook is used to extract data from the Redux store's state. It subscribes the component to the Redux store, and when the selected part of the state changes, the component re-renders.

```javascript
import { useSelector } from 'react-redux';
const todos = useSelector((state) => state.todos);
```

