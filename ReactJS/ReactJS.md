# ReactJS

## catalog

- [ReactJS](#reactjs)
  * [redux](#redux)
    + [intuition](#intuition)
    + [start and run](#start-and-run)
    + [terminologies](#terminologies)
      - [action](#action)
      - [reducer](#reducer)
        * [functions:](#functions-)
      - [store](#store)
        * [functions:](#functions--1)
      - [enhancer](#enhancer)
      - [middleware](#middleware)
        * [functions:](#functions--2)
    + [hierarchy of the project](#hierarchy-of-the-project)

<small><i><a href='http://ecotrust-canada.github.io/markdown-toc/'>Table of contents generated with markdown-toc</a></i></small>



## redux

### intuition



### start and run

You need to install two package in the project, `npm install redux` and `npm install react-redux`.

- redux is the core library
- react-redux binds react and redux together, separating from redux core library.

Then `npm start` , or `npm install react-scripts` if `react-scripts` package is missed.



### terminologies

#### action

An object containing a `type` field, describes what happened, looks like this `{type: 'todos/todoAdded', payload: todoText}`. 

#### reducer

Updates the `redux state` when a certain action happens by using `switch case` snippet or something else.

The input arguments are `state` and `action`.

##### functions:

- `combineReducers`



#### store

The core of the `redux` , store decides how to handle with actions and update state. It will first dispatch the `action`, and update the `state` according to the actions.

##### functions:

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

- `createStore` 



#### enhancer

[enhancer tutorial](https://redux.js.org/tutorials/fundamentals/part-4-store#configuring-the-store)

When applying on the `store.dispatch`, wraps the `action` to expect more rather than just update the specified `state` when a certain action or event happens.



#### middleware

[middleware tutorial](https://redux.js.org/tutorials/fundamentals/part-4-store#middleware)

To customize the `store.dispacth` function using a pipeline, where the original `store.dispatch` locates at the end of the pipeline.

Just like a powerful `reducer` , middleware can do more than `reducer`.

Hierarchy of `middleware` :

1. outer layer: `store`, including the `middleware` pipeline, `reducer` and `preloadState`.
2. middle layer: the `pointer` of the next node in the pipeline
3. inner layer: the action, actually an `object` with a `type` attribute

##### functions:

- `applyMiddleware` don't forget to apply `applyMiddleware` first before `createStore`.





### hierarchy of the project







