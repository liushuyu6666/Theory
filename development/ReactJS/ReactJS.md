# Implementation

## Start

- enter an empty folder
- in `cmd` type `npx create-react-app my-app`
- `cd my-app`
- `npm start`

## Install dependencies

- `npm install react-router-dom`
- redux
- bootstrap

## proxy

configure the proxy in `package.json`.

# Theoretical Knowledge

## Class Component

### Library

#### router

- App.js

  ```javascript
  <Router>
      <Switch>
      	<Route path="/shops/:shopId/dishes">
          	<DishesInShop />
          </Route>
  	</Switch>
  </Router>
  ```

##### use parameter in link

- refers `shopId` in DishesInShop.js

  ```javascript
  this.props.match.params.shopId
  ```

##### use `history.push()`

- refers another link in DishesInShop.js

  ```javascript
  this.props.history.push(`../${dishId}/...`);
  ```



## Functional Component

### Library

#### router

##### use `history.push()`

- refers 

