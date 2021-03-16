# ReactJS

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

