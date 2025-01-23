## Discussion Topics

### Advanced State

As an application grows, so too does the state that is needed to represent the UI. Adding more data or interactivity leads to an increasing number of state updates to orchestrate. Without careful management of state changes, troubleshooting interface bugs becomes challenging and increases the likeliness that a new feature may introduce bugs.

CTD Swag is starting to show signs of this problem, which can be identified by the quantity of `useState`'s found in any one component. Consider the `App` component:

```jsx
{/*extract from App.js*/}
{/*...code*/}
function App() {
  const [inventory, setInventory] = useState([]);
  const [filteredInventory, setFilteredInventory] = useState([]);
  const [cart, setCart] = useState([]);
  const [isCartOpen, setIsCartOpen] = useState(false);
  const [isCartSyncing, setIsCartSyncing] = useState(false);
  const [isAuthDialogOpen, setIsAuthDialogOpen] = useState(false);
  const [isAuthenticating, setIsAuthenticating] = useState(false);
  const [user, setUser] = useState({});
  const [authError, setAuthError] = useState('');
  const [cartError, setCartError] = useState('');
  const [cartItemError, setCartItemError] = useState('');
  const [isDialogOpen, setIsDialogOpen] = useState(false);
  const [isRegistering, setIsRegistering] = useState(false);
  const [isSortAscending, setIsSortAscending] = useState(true);
  const [sortBy, setSortBy] = useState('baseName');
  const [searchTerm, setSearchTerm] = useState('');
{/*code continues...*/}
```

All of the state update functions listed above are referenced at least once somewhere in the component. Many are found in two places. `setIsAuthenticating` is called in five locations!

![IDE screen shot with all instances of setIsAuthenticating highlighted](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/lessons/assets/week-11/set-is-auth-hilighted.png)

`setIsAuthenticating` is found only in two event handlers, but but state update functions are also used in `useEffect`s and passed in props to child components. You can see how it can be easy to lose track of state!

### The Reducer Pattern

The reducer pattern changes our approach to coordinating state updates in complex applications. In a broader software engineering context, a reducer refers to a design pattern that focuses on updating data by communicating events to a centralized function that makes all of t.

This pattern consists 3 key elements that each work together to coordinate state updates.

- a **reducer** function
- **dispatch** functions
- **actions** (can be of any type but conventionally an object)

A reducer function operates on a collection of data elements, such an array or object, by applying update logic to produce an output. They centralize the logic needed to manage state values. We communicate with the reducer using compact dispatch functions. A dispatch's only job is to relay details about a user action to the reducer function.

```javascript
//example reducer and dispatches

//reducer accepts the current state and an action object
//it outputs an updated state value for us to use
const reducer = (state, action) => {
  switch (action.type) {
    case 'increment':
      return { count: state.count + 1 };
    case 'decrement':
      return { count: state.count - 1 };
    case 'add':
      return { count: state.count + action.value };
    case 'reset':
      return { count: 0 };
    default:
      return state;
  }
};
//dispatch containing an action object that adds 1
dispatch({ type: 'increment' });

//action objects can also be written outside dispatch
const decrement = {
  state,
  type: 'decrement',
};
dispatch(decrement);

//we can provide extra data for the reducer
//to compute the output value
const add5 = {
  state,
  type: 'add',
  value: 5,
};

dispatch(add5);

dispatch({ type: 'reset' });
```

> [!note]
> The code example above is non-functional. The dispatch function provided by the library or tool we adopt. The library also calls the reducer with the required inputs. For CTD-Swag, we will be using the `useReducer` hook.

Up until now, we have focused on making state updates by processing events inside of event handlers and helper functions For example when a user adds a product to the cart:

1. user clicks button "Add to Cart"
2. the click event executes an event handler which:
   1. retrieves relevant data from state and the event
   2. transforms or creates a new value for next state
   3. calls a state update function with the state's next value
3. the state update function starts a render cycle which:
   1. replaces old state value with the update provided
   2. re-renders interface using new value

When working with reducers, it helps to shift perspectives from "what event just happened" to "what a user just did". This is referred to as [inversion of control](https://en.wikipedia.org/wiki/Inversion_of_control). Continuing with the example above, rather than thinking **"the user clicked on the 'Add to Cart' button on a product card representing the bucket hat"**, our new thought process would be **"the user added a bucket hat to their cart"**. This statement is much simpler than the first. It reflects that the component only needs to know that a user performed an action. The reducer function, which exists independently of the component, is responsible for adding the bucket hat to the cart state. Our event handlers' only responsibility for state updates is to communicate the details of the action to the reducer. We'll continue to explore the reducer pattern but need to learn a little about the `useReducer` hook first.

### useReducer

The `useReducer` hook adapts the reducer pattern for use in React applications. Just like other hooks, it must be called at the top level of the component. React attempts to batch state changes so that if several happen in succession, they are all processed during the same render cycle. React also compares the reducer's output to its previous state - if nothing changes, it does not initiate a re-render.

`useReducer` takes a reducer function and an initial state value when called. It outputs a state value and a dispatch function which are assigned similar to `useEffect`.

```js
const [state, dispatch] = useReducer(reducer, initialState);
```

Reducer functions and the initial state values tend to be more complex than the arguments than a `useState` hook. The reducer function also works independently from any component: it reads no values from inside the component directly. Because of these factors we will place these in a separate file to keep the component's size to a minimum.

For this discussion, we will implement the reducer pattern on cart's state. We create a file `cart.reducer.js` and place it into a `/reducers` folder created under `/src`. It's helpful to drop the "x" from the filename's extension since it will not contain any React-specific code or JSX. This will tell us, at a glance that it is not component code without having to open the file up. After creating the new file, we when need to identify the relevant `useState`s:

![ide screenshot highlighting cart state](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/lessons/assets/week-11/cart-state-hilighted.png)

The `useState`s highlighted above all manage some aspect of state related to the cart. From this list, we can determine `initialState`:

```js
// extract from cart.reducer.js
const initialState = {
  cart: [],
  isCartOpen: false,
  isCartSyncing: false,
  //for the sake of simplicity here, we combine cartError and cartItemError
  error: '',
};
//code continues...
```

We then create a reducer that returns the state for now.

```js
// extract from cart.reducer.js
//...code
function reducer(state, action) {
  switch (action.type) {
    default:
      return state;
  }
}

export { initialState, reducer };
```

We will add `case` statements for each of the actions we identify to organize the state logic. `switch/case` flow control is easier to read than `if/else` statements so is usually preferred. Reducer functions tend to grow quite long.

We then import the initial value and the reducer into `App.js` and add them to a `useReducer` hook:

```jsx
{
  /*extract from App.jsx*/
}
{
  /*...code*/
}
import {
  //aliasing with `as` keeps the reducer and state easily identifiable
  initialState as cartInitialState,
  reducer as cartReducer,
} from './reducers/App/cart.reducer';

{
  /*...unrelated code...*/
}

const [cartState, dispatch] = useReducer(cartReducer, cartInitialState);
{
  /*code continues...*/
}
```

Next, we'll find places where `isCartOpen` is used and determine how argument is created. In some cases, it's a direct value that is passed in. In other cases, our event handlers and helper functions calculate that value. Using VS Code's file search we can find 3 instances to `setIsCardOpen`.

![ide file search result numbers for setIsCartOpen](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/lessons/assets/week-11/is-cart-open-search.png)

The first location is in `useState` so we do not need to worry about that for now. The second one is in a handler function that closes the cart and the final on is in the Header instance:

```jsx
{/*extract from App.jsx*/}
{/*...code*/}
 function handleCloseCart() {
  {/*used here*/}
     setIsCartOpen(false);
     setAuthError('');
 }
/*...unrelated code...*/
 <Header
  cart={cart}
  {/*used here*/}
  handleOpenCart={() => setIsCartOpen(true)}
  handleOpenAuthDialog={handleOpenAuthDialog}
  handleLogOut={handleLogOut}
  user={user}
    />
{/*code continues...*/}
```

They **close** and **open** the cart respectively. These actions will be the basis of the action objects the we eventually write. Both locations set the boolean value directly so we don't have any state logic to move over to the reducer other than updating those values.

We add a `case` statement for each of these actions to the reducer. From each we return a new object that spreads the original `state` and a new value for the `isCartOpen` property.

```js
// extract from cart.reducer.js
function cartReducer(state, action) {
  switch (action.type) {
    case "open":
      return {
        ...state,
        isCartOpen: true,
      };
    case "close":
      return {
        ...state,
        isCartOpen: false,
      };
    default:
   return state;
//...code
//code continues...
```

Before removing any state update function, we can place the dispatch along side it if we need to do any troubleshooting. We can the log the reducer's output to ensure the action's `case` block functions as expected.

Recall that the dispatch function takes an `action`. This can be of any type (string, object, number, etc.) but by convention, we stick with objects containing a `type` property to identify the action and add properties as needed. The actions that our reducer will receive for the cart actions resemble:

```javascript
const open = { value: 'open' };
const close = { value: 'close' };
```

When we are happy with how the `case` blocks work in the reducer, we can then update all `isCartOpen` references with `cartState.isCartOpen` to migrate to the updated state.

We then update each `isCartSyncing` with `cartState.isCartSyncing` to use the state our `useReducer` returns.

```jsx
{
  /*extract from */
}
{
  /*...code*/
}
{
  cartState.isCartOpen && (
    <Cart
      cartError={cartError}
      isCartSyncing={isCartSyncing}
      cart={cart}
      handleSyncCart={handleSyncCart}
      handleCloseCart={handleCloseCart}
    />
  );
}
{
  /*code continues...*/
}
```

After completing these updates, our cart behaves the same but that state is now fully managed by the reducer.

![animated screen capture highlighting isCartOpen as UI is manipulated](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/lessons/assets/week-11/is-cart-open-state.gif)

We are now left with 5 more `useStates` to refactor.

```javascript
const [cart, setCart] = useState([]);
const [isCartOpen, setIsCartOpen] = useState(false); //DONE
const [isCartSyncing, setIsCartSyncing] = useState(false);
const [cartError, setCartError] = useState('');
const [cartItemError, setCartItemError] = useState('');
```

Besides `cart`, each other state value update employs the same complexity as `isCartOpen`. We'll refactor these without discussion so we can then explore how we identify actions and refactor the state updating logic over to our reducer.

Refactoring `cart` is the most complex because of the differing ways that the user interacts with the contents of their cart. I`setCart` is referenced in 7 places in `App.jsx` excluding the `useEffect`

- 1 time in `handleAuthenticate`
  - when a user logs in and has a saved cart, `setCart` receives `userData.cartItems` from the API response
- 2 times in `handleSyncCart`
  - when a user is not logged in and they confirm a cart change, `setCart` receives `workingCart`
  - when a user is logged in and they confirm a cart change, `setCart` receives the `cartData` from the API
- 3 times in `handleAddItemToCart` (this is the most complex set of uses since we used an optimistic approach to managing `cart` state)
  - when a user adds an item the event handler:
    - it finds if there's a matching item in the cart.
      - if yes, it creates a duplicate cart item then increments the matching item quantity
      - if no, it creates a new cart item with a quantity of 1
    - it then calls `setState` with an array which includes the newest item
    - if an API response fails, the cart item is reverted to:
      - previous value if it has a quantity greater than one or
      - removes it if its quantity is 1
- 1 time in `handleLogOut`
  - `setCart` receives an empty array to empty the cart

We can distill this list into two actions: **"add item"** and **"update cart"**

- A user logs in and they have a saved cart that is loaded: **"update cart"**
- A user modifies item counts in the cart: **"update cart"**
- A user adds an item to their cart: **"add item"**
- A user logs out which empties the cart: **"update cart"**

"add item" ends up being the more complex of the two because of the data manipulation required to make cart items out of products in the product list. We will take care of "update cart" first since it operates on the whole cart and, as a result, has less supporting logic.

In each occasion where we plan on using the "update cart" action, the reducer is going to receive an action object with a cart value that replaces the existing cart. In each of the areas where we use this action, we are going to pass an updated cart received from the API or when a user confirms a cart edit.

We'll update the cart reducer to include a `case` for "update cart" and add in the logic that returns the new cart value we will place into the action object.

```js
// extract from cart.reducer.js
//...code
function CartReducer(state, action){
 switch(action.type) {
  case "open":
      return {
          ...state,
          isCartOpen: true,
      };
  }
//code continues...
  case "update"

}
```

We can then go back to the codebase and replace the `setCart`s where we intend on dispatching the "update cart" action. You can also see the other dispatches that have already been placed in `handleSyncCart` and `handleAuthenticate` to deal with other aspects of cart state.

```jsx
//extract from App.jsx
//...code
const handleSyncCart = useCallback(
  async (workingCart) => {
    if (!user.id) {
      //dispatch replaces setCart here
      dispatch({ type: 'update cart', cart: workingCart });
      return;
    }
    dispatch({ type: 'sync' });
    const options = {
      method: 'PATCH',
      body: JSON.stringify({ cartItems: workingCart }),
      headers: {
        'Content-Type': 'application/json',
        Authorization: `Bearer ${user.token}`,
      },
    };
    try {
      const resp = await fetch(`${baseUrl}/cart`, options);
      if (!resp.ok) {
        console.log('resp not okay');
        if (resp.status === 401) {
          throw new Error('Not authorized. Please log in.');
        }
      }
      const cartData = await resp.json();
      if (cartData.error) {
        throw new Error(cartData.error);
      }
      //dispatch replaces setCart here
      dispatch({ type: 'update cart', cart: cartData });
    } catch (error) {
      console.error(error);
      dispatch({ type: 'error', error: error.message });
    } finally {
      dispatch({ type: 'not syncing' });
    }
  },
  [user.id, user.token, dispatch],
);
//code continues...
async function handleAuthenticate(credentials) {
  const options = {
    method: 'POST',
    body: JSON.stringify(credentials),
    headers: { 'Content-Type': 'application/json' },
  };
  try {
    setIsAuthenticating(true);
    const resp = await fetch(`${baseUrl}/auth/login`, options);
    if (!resp.ok) {
      if (resp.status === 401) {
        setAuthError('email or password incorrect');
      }
      throw new Error(resp.status);
    }
    const userData = await resp.json();
    setUser({ ...userData.user, token: userData.token });
    dispatch({
      type: 'update cart',
      cart: userData.cartItems,
    });
    setAuthError('');
    setIsAuthenticating(false);
    setIsAuthDialogOpen(false);
  } catch (error) {
    setIsAuthenticating(false);
    console.log(error.message);
  }
}
//code continues...
```

We then have to determine the logic for the "add item" action. The first challenge is that the logic filters through the inventory list to ensure the item exists before making a new cart item or incrementing the quantity on an existing cart item. Since our reducer exists independently from the App component, we need to include the inventory on the action object.

With this in mind, we can move over the logic and update any `inventory` references to `action.inventory` and create a return value that reflects the updated state:

```js
// extract from cart.reducer.js
//...code
 case cartActions.addItem: {
      const inventoryItem = action.inventory.find(
        (item) => item.id === action.id
      );
      if (!inventoryItem) {
        return state;
      }
      const itemToUpdate = state.cart.find(
        (item) => item.productId === action.id
      );
      let updatedCartItem;
      if (itemToUpdate) {
        updatedCartItem = {
          ...itemToUpdate,
          quantity: itemToUpdate.quantity + 1,
        };
      } else {
        updatedCartItem = {
          ...inventoryItem,
          quantity: 1,
          productId: inventoryItem.id,
        };
      }
      return {
        ...state,
        cart: [
          ...state.cart.filter((item) => item.productId !== action.id),
          updatedCartItem,
        ],
      };
    }
//code continues...
```

Almost done! Now we have to back to the App component and change all `cart` references to `cartState.cart`. These are found as props given to the Header component and the Cart component:

```jsx
{
  /*extract from App.jsx*/
}
{
  /*...code*/
}
<Header
  cart={cartState.cart}
  handleOpenCart={() => dispatch({ type: 'open' })}
  handleOpenAuthDialog={handleOpenAuthDialog}
  handleLogOut={handleLogOut}
  user={user}
/>;
{
  /*...code...*/
}
{
  cartState.isCartOpen && (
    <Cart
      cartError={cartState.error}
      isCartSyncing={cartState.isCartSyncing}
      cart={cartState.cart}
      handleSyncCart={handleSyncCart}
      handleCloseCart={handleCloseCart}
    />
  );
}
{
  /*code continues...*/
}
```

After these changes, our cart behaves as it did previously, but all of its state is now managed by the reducer. Because the reducer, dispatch function and the action are so tightly coupled, the reducer function is probably one of the most complex things we have covered so far. While it is harder to employ, it is far easier to manage complex state this way than relying on numerous `useState`s and we also end up with a much more compact function.

### Passing Data Using useContext (WIP)

Managing state and passing data between components is a fundamental aspect of building dynamic and interactive applications. React's `useContext` hook simplifies working with shared state across different parts of your application without the need for prop drilling. It allows us to centralize state logic which makes it easier to maintain and scale React applications.

- CTD-Swag doesn't have any deep state issues - we'll stick to a clean, minimal example.
  - sibling components (li) placed in a ul component that do varying calculations (and also have a reset button) on an input provided in a form located in a parent component.
  - Form has componentized inputs

```terminal

└── App
    ├── Form
    │   └── Input
    └── Calculations
  └── Calculation
```

- useContext described
  - allows us to communicate into a deeply nested component without the need to pass state and updaters down through props down the whole tree.
  - allows a top level component create a "context" (interrelated conditions in which the data exists) that any of its descendants can access. Restated:
  1. component tells React that it has a state value and an updater that it wants to share with its descendants
  2. React makes that data available without having requesting it up and down a chain of parent-child relationships. "prop drilling"
- useContext put into use
