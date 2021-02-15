# redux

## catalog

- [catalog](#catalog)
- [intuition](#intuition)
- [start and run](#start-and-run)
- [terminologies](#terminologies)
  * [action](#action)
  * [reducer](#reducer)
    + [functions:](#functions-)
  * [store](#store)
    + [functions:](#functions--1)
  * [enhancer](#enhancer)
  * [middleware](#middleware)
    + [functions:](#functions--2)
- [hierarchy of the project](#hierarchy-of-the-project)

<small><i><a href='http://ecotrust-canada.github.io/markdown-toc/'>Table of contents generated with markdown-toc</a></i></small>

## intuition



## start and run

You need to install two package in the project, `npm install redux` ， `npm install react-redux` and `npm install redux-thunk`.

- `redux` is the core library
- `react-redux` binds react and redux together, separating from redux core library. The original redux will not update UI automatically when `state` updates, so this library is needed to subscribe to the store and efficiently update the UI as `state` changes<sup>[ref](https://redux.js.org/tutorials/fundamentals/part-5-ui-react#basic-redux-and-ui-integration)</sup>.
- `redux-devtools-extension` use `redux devtools` to debug.
- `redux-thunk` handles with asynchronous actions such as `fetch`.

Then `npm start` , or `npm install react-scripts` if `react-scripts` package is missed.



## entities

### action

An object containing a `type` field, describes what happened, looks like this `{type: 'todos/todoAdded', payload: todoText}`. 

### reducer

Updates the `redux state` when a certain action happens by using `switch case` snippet or something else.

The input arguments are `state` and `action`.

#### functions

- `combineReducers`



### store

The core of the `redux` , store decides how to handle with actions and update state. It will first dispatch the `action`, and update the `state` according to the actions.

#### functions

- `store.getState` retrieve current states from the state tree

- `store.dispatch` trigger the reducer to update the state

- `store.subscribe` a callback function returning a function to cancel itself. When `state` update, the `store.subscribe` will be triggered to execute. [check the usage here](https://redux.js.org/tutorials/fundamentals/part-4-store#dispatching-actions), also see the code snippet: ```

  ```javascript
  // any time when the state update, print
  const unsubscribe = store.subscribe(() =>
      console.log("state after dispatch: ", store.getState())
  );
  
  store.dispatch({type: "todos/todoAdded", payload: "learn about actions"})
  // something else ...
  
  // stop printing anything
  unsubscribe();
  ```

- `createStore` [click here](https://redux.js.org/tutorials/fundamentals/part-4-store#creating-a-store)

### enhancer

[enhancer tutorial](https://redux.js.org/tutorials/fundamentals/part-4-store#configuring-the-store)

When applying on the `store.dispatch`, wraps the `action` to expect more rather than just update the specified `state` when a certain action or event happens.



### middleware

[middleware tutorial](https://redux.js.org/tutorials/fundamentals/part-4-store#middleware)

To extend the `store.dispacth` function using a pipeline, where the original `store.dispatch` locates at the end of the pipeline.

Just like a powerful `reducer` , middleware can do more than `reducer`.

Hierarchy of `middleware` :

1. outer layer: `store`, including the `middleware` pipeline, `reducer` and `preloadState`.
2. middle layer: the `pointer` of the next node in the pipeline
3. inner layer: the action, actually an `object` with a `type` attribute

#### functions

- `applyMiddleware` don't forget to apply `applyMiddleware` first before `createStore`. Could also combine several `middleware` together.

## hook

In the `component` we can't access to the `store` , so we need hook.

### createSelect

Like a pipeline, all are `input selectors` except the last one which is an `output selector`. The `output selector` need to argue output from all `input selectors`.

[click here](https://redux.js.org/tutorials/fundamentals/part-7-standard-patterns#memoizing-selectors-with-createselector)

### useSelector

Lets your `React components` read data from the `Redux store`. We can get `redux state` by `state.something` , but `useSelector` will re-render the UI if `state` is updated. It is not the same as `component state` since when `component state`  update UI will be updated automatically, but here when `redux state` is updated you need to remind UI that the `redux state`  is updated. So, any `redux state` used in the UI need to go through `useSelector` hook. 

`useSelector` accepts a single function, which we call a **selector** function. **A selector is a function that takes the entire Redux store state as its argument, reads some value from the state, and returns that result**. Whatever the `selector` returns will be returned by the `useSelector` to your `component`

Use `useSelector` in this way 

```javascript
const selectTodos = state => state.todos
const TodoList = () => {

  const todos = useSelector(selectTodos)

  const renderedListItems = todos.map(todo => {
      return <TodoListItem key={todo.id} todo={todo} />
  })

}
```

instead of

```javascript
// Bad: always returning a new reference
const selectTodoDescriptions = state => {
  // This creates a new array reference!
  return state.todos.map(todo => todo.text)
}

const TodoList = () => {

  const todos = useSelector(selectTodoDescriptions)

  const renderedListItems = todos.map(todo => {
      return <TodoListItem key={todo.id} todo={todo} />
  })

}
```

Besides, `useSelector` must be called in a `React function component` or a custom `React Hook function`. `React component` names must start with an uppercase letter



[the whole tutorial, caution and tips, click here](https://redux.js.org/tutorials/fundamentals/part-5-ui-react#reading-state-from-the-store-with-useselector)

### useDispatch

```javascript
import {useDispatch} from 'react-redux'

const dispatch = useDispatch();
dispatch({type: 'todos/todoAdded', payload: something})
```



[tutorial](https://redux.js.org/tutorials/fundamentals/part-5-ui-react#dispatching-actions-with-usedispatch)



## embed `redux` in an existed `react`

### install packages

- `redux`
- `react-redux`
- `redux-devtools-extension` 



### create store

In `src/store.js` file.

### create root reducer

In `src/reducer.js` file.

### create slice file

In `src/Redux/your-name/slice.js` file.

We need `initialState`, `reducer` , `action` and `thunk`

### modify index.js

### `store` and `component`

#### access to `redux state` from `react component`

```javascript
class yourComponent extends Component {...}
function mapStateToProps(state){
    return{
        yourEntity: state.yourEntityName
    }
}
export default connect(mapStateToProps)(yourComponent);
```





## hierarchy of the `react-redux` project

- src
  - features
    - token
      - tokenSlice.js: define `initialState`, `tokenReducer`, define `action` and some support functions that will dispatch `action`, such as `thunk function`, and `createSelector`. Here you can access to `redux state` directly. Don't import `store` here so don't use `store.useSelector`.
      - profileTable.jsx: a UI file to render the snippet UI by using `redux state`, here we use `useSelector` to extract data out and use in `render`.

## PROBLEM

### cannot access 'initialState' before initialization

- I import `store` in the `tokenReducer.js`, so there will be a cycle.

