# JavaScript

## DOM

| function                                 |      |      |
| ---------------------------------------- | ---- | ---- |
| document.getElementById                  |      |      |
| document.getElementsByTagName("body")[0] |      |      |
| object.getAttribute("href")              |      |      |
| document.createElement("p")              |      |      |



## Data Type

### convert `array` to `object`

```javascript
const arr = ['a','b','c'];
const res = arr.reduce((acc,curr)=> (acc[curr]='',acc),{});
# {a:'', b:'', c:''};
```

- Each time of the loop `curr` inserted into accumulator `acc[curr] = ''` like this. 
- `acc` is returning the accumulator once loop end. 
- `{}` initial value of the `acc`.

### check if `object` is empty

```javascript
var o = {};
if(o) // check if undefined or empty
```



## asynchronous

### callback function



### `Promise`

#### define a `Promise`

`promise` is a class, who has two attributes, `PromiseStatus` and `PromiseValue`.  

We define `promise` in this way with two function handles: `resolve` and `reject`, whose name could be modified but the position can't be changed.

```javascript
const promise = new Promise((resolve, reject) => {
    ...
    if(){
        resolve("resolved");
    }
    else{
        reject("rejected")
    }
})
```

- `resolve` and `reject` are two function handles. 
- `resolve` set `PromiseStatus` into `fulfilled` and load "resolved" (or other object) into `PromiseResult`
- `reject` set `PromiseStatus` into `rejected` and load "rejected" (or other object) into `PromiseResult`

check this image:

 <img src="img/promise_resolve_and_reject.png" style="zoom:70%;" />

- The error shows that here is no `catch()` for `yourPromise`.

#### `then`

`then` is a method of `Promise` which can be used after a `Promise`. `then` returns a `Promise` as well.

We will use `then` in this way:

```javascript
const promise = new Promise((resolve, reject) => {
    ...
    if(){
        resolve("resolved");
    }
    else{
        reject("rejected")
    }
})
promise.then(res => {
    // handle with res if PromiseStatus = fulfilled
},
            rej => {
    // handle with rej if PromiseStatus = rejected
})
```

- The `.then()` method takes up to two arguments; the first argument is a callback function for the resolved case of the promise, and the second argument is a callback function for the rejected case.
- The `.then()` will first check the `PromiseStatus`  of the `Promise`, if it is `fulfilled`, then execute resolve function, if it is `rejected`, then execute reject function.
- After that, the `then()` create a `Promise`

check the picture:

<img src="img/then_resolve_and_reject.png" style="zoom:50%;" />

-  `PromiseState` of `myPromise` is `fulfilled` and `yourPromise` is `rejected`. so in the first case, `resolve` function is called and in the second case, `reject` function is executed.

- ```javascript
  res => console.log(res) // resolve callback function
  rej => console.error(rej) // reject callback function
  ```

- `res` and `rej` are the `PromiseResult` of the previous `Promise`.

- the `then()` will return a `Promise`, in these two cases, both `PromiseState` are `fulfilled` and both `PromiseResult` are `undefined` since we didn't return anything.

check this picture:

<img src="img/then_resolve_and_reject2.png" style="zoom:50%;" />

- in these two cases, since we return `res` or `rej`, the  `PromiseResult` of the new `Promise` is filled.

The `then()` could have only one argument - a `resolve` function handle without a `reject` function handle, here we annotate it as `then(res)`. If so, a `rejected` Promise will be omitted by all `then(res)`in the `Promise` chain until the final `.catch()`. 

<img src="img/then_one_argument.png" style="zoom:50%;" />

#### `catch`

Just like one argument `then()` who just focus on `resolved Promise`, the `catch()` only pay attention to the `rejected Promise`. The `catch()`returns a `Promise`.

<img src="img/catch.png" style="zoom:50%;" />

#### `Promise` chain

By use one argument `then()` and `catch()` , we can form a `Promise` chain, code like this:

- ```javascript
  promise.then(res => ...).then(res => ...).then(res => ...).catch(error => ...)
  ```
  - the `rejected Promise` will be omitted by the next `then()` until received by the `catch()`

- For a `rejected Promise`: `yourPromise` is a `rejected Promise`, only the last `catch()` is trigged.

  <img src="img/promise_chain_1.png" style="zoom:50%;" />

- For a `fullfilled Promise`:  `myPromise` is a `fulfilled Promise` all but the `catch()` will be executed.

  <img src="img/promise_chain_2.png" style="zoom:50%;" />

- If a `then()` return a `rejected Promise`, all other `then()` behind it will be ignored.

  <img src="img/reject_in_then.png" style="zoom:67%;" />

  - Here the second `then()` return a `rejected Promise`, the third one is ignored.



#### `throw`

When throw the exception, it is almost the same as a `rejected Promise`.

<img src="img/throw_1.png" style="zoom:67%;" />

#### asynchronous

We use `setTimeout()` to stimulate asynchronous `Promise`.

- create a Promise

  <img src="img/asyn_1.png" style="zoom:67%;" />

  - `herPromise` will call `resolve` 20 seconds later, so at first it is `pending` until the `resolve` is called.

- create a chain

  <img src="img/asyn_2.png" style="zoom:67%;" />

  - at first the initial  `Promise` is still `pending`, so then will wait until the `Promise` change to `resolved`.
  - set an 8-second `Timer A` and hang the print 1.1.
  - print 1.
  - set an 1-second `Timer B` and hang the print 2.2.
  - print 2.
  - `Timer B`'s time is up, print 2.2.
  - `Time A`'s time is up, print 1.1.

- unfair for `Promise`

  <img src="img/asyn_3.png" style="zoom:67%;" />

  - `hisPromise` executes after `console.log("hello")`.

### `async` and `await`

check it here [`async` and `await`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function)



## Operands

### `||` logical OR

```javascript
expr1 || expr2
```

- If `expr1` can be converted to `true`, returns `expr1`; else, returns `expr2`.
- Examples of expressions that can be converted to false are:
  - `null`;
  - `NaN`;
  - `0`;
  - empty string (`""` or `''` or ````);
  - `undefined`.
- `expr1 || ''` convert `falsy` value to `''`

### `&&` logical AND

```javascript
expr1 && expr2
```

- If `expr1` can be converted to `true`, returns `expr2`; else, returns `expr1`.
- Examples of expressions that can be converted to false are:
  - `null`;
  - `NaN`;
  - `0`;
  - empty string (`""` or `''` or ````);
  - `undefined`.

### `||` and `&&` chain

```javascript
expr1 && expr2 || ''
```

- `yourHead.hair && yourHead.hair.color || ''` if you have hair, return its color, if not, return `''`
- `users && user[0] || null` if `users` is null, return null, else return the first user.

## Map

### function in map

