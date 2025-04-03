## Discussion Topics

> [!note]
> Hooks, events, and updating state are extremely interconnected topics. To keep code examples from being too complicated, the first 3 sections will not build on CTD Swag. We will return to that in the final section, Updating State, where we tie everything together. Even though the examples are simplified, they will include some code that has not been covered quite yet. Everything illustrated will be covered by the end of the lesson. Where possible, the discussion highlights specific lines of code to pay attention for each example.

### Basic Hooks

A React Hook is a function that allows functional components add state, integrate with side-effects, and work with other React features. We can also develop our own custom hooks to encapsulate logic into reusable functions. Hooks were introduced in React 16.8 (released February 2019) and gave functional components access to features that were previously available to only class-based components. We won't be working with class-based components but it's important to be aware they exist since you may encounter them in older codebases.

The existing hooks can be broken down into several categories. We'll highlight the common ones.

- **state** - stores information like user input that is used to help render components
  - `useState`: keeps track of a single, updatable value
  - `useReducer`: used when there are a lot of values to track and update in a component - covered in week 11
- **context** - refers to a way to share data between components without having to pass props
  - `useContext` allows a component to read and subscribe to a defined context - covered in week 11
- **effects**
  - `useEffect`: allows us to run side effects like loading data or working with non-React page code - more in week 7.
- **refs** - short for "reference" - they allow us to save and work with values that persist across renders but are not used for rendering
  - `useRef`: creates a `ref` that can hold a value or commonly to access a DOM node
- **performance**: optimizes re-render performance by allowing unnecessary re-rendering to be skipped - more in week 9.
  - `useMemo`: caches result of an expensive calculation
  - `useCallback`: caches a function definition between re-renders
- **custom**: hooks created by the developer or imported from a 3rd party library to encapsulate re-usable logic - advanced topic not covered in this course

Lets examine several more frequently used hooks.

#### useState

`const [state, updateState] = useState(initialState);`

We were introduced to `useState` last week to load our inventory. It's important to recall that React prefers a functional approach. In the instance of `useState`, it means that we do not update state directly. Instead we give a new value to the state update function. This function:

1. Requests React to re-render the component.
2. Replaces the old state value with the new value.

When React re-renders the component, it uses this new value.

React performs [shallow comparisons](https://monsterlessons-academy.com/posts/shallow-comparison-vs-deep-comparison-in-javascript) when looking for state changes. For primitive state values (strings, numbers), updating is a straight-forward process. In the example below, each time the "Count Up" button is pushed, `setCounter` increments the counter then tells React to refresh the component, replacing the previous counter value.

```jsx
function Counter() {
  const [counter, setCounter] = useState(0);
  function updateCounter() {
    setCounter(counter + 1); //
  }
  return (
    <>
      <p>{counter}</p>
      <button type="button" onClick={updateCounter}>
        Count Up
      </button>
    </>
  );
}
```

For objects and arrays React does not look into their contents so cannot see changes to object members or array items. To properly update these state in this case, we need to create a new object or array.

The example below demonstrates how to add items to an array. The buttons use `onClick` event listeners that call `addOption`. Each button press passes a corresponding text value for the option it represents. Inside `addOption`, found on line 6, we call state update function `setTacoOptions` with a new array. Inside this array, we use spread syntax to spread in the current `tacoOptions` and add the new option.

```jsx
//useState with an array

function TacoBuilder() {
  const [tacoOptions, setTacoOptions] = useState(['corn tortilla']);

  function addOption(option) {
    //spread operator expands tacoOptions into a new array
    setTacoOptions([...tacoOptions, option]);
  }

  function showSelections() {
    return tacoOptions.reduce(
      (accumulator, currentValue, currentIndex, tacoOptions) => {
        if (currentIndex === tacoOptions.length - 1) {
          return `${accumulator}, and ${currentValue}`;
        }
        return `${accumulator}, ${currentValue}`;
      },
    );
  }

  return (
    <>
      <div className="mains">
        <button type="button" onClick={() => addOption('chicken')}>
          Chicken
        </button>
        <button type="button" onClick={() => addOption('beef')}>
          Beef
        </button>
        <button type="button" onClick={() => addOption('vegetarian')}>
          Vegetarian
        </button>
      </div>
      <div className="cheeses">
        <button type="button" onClick={() => addOption('cheddar')}>
          Add Cheddar
        </button>
        <button type="button" onClick={() => addOption('queso fresco')}>
          Add Queso Fresco
        </button>
      </div>
      <div className="extras">
        <button type="button" onClick={() => addOption('sour cream')}>
          Add Sour Cream
        </button>
        <button type="button" onClick={() => addOption('salsa')}>
          Add Salsa
        </button>
      </div>
      <div>
        <p>Your taco includes: {showSelections()}</p>
      </div>
    </>
  );
}
```

Object-based state value takes a similar approach. In the example below, the `CreateUserForm` component contains two input fields that are wired to listen to changes. Each time their contents change, they call `updateFirstName` or `updateLastName`. These functions use the `value` from the event object to determine the new value.

```jsx
//useState with an object

function CreateUserForm() {
  const [userInfo, setUserInfo] = useState({ firstName: '', lastName: '' });

  function handleSubmit(event) {
    //logic to handle form submission
  }
  function updateFirstName(event) {
    //more on the event object later this lesson...
    //pulling the first name out of the event object
    const firstName = event.target.value;
    setUserInfo({ firstName: firstName, lastName: userInfo.lastName });
  }
  function updateLastName(event) {
    //pulling the last name out of the event object
    const lastName = event.target.value;
    setUserInfo({ firstName: userInfo.firstName, lastName: lastName });
  }
  return (
    //more on forms in
    <form submit={handleSubmit}>
      <label>
        First Name
        <input onChange={updateFirstName} />
      </label>
      <label>
        Last Name
        <input onChange={updateLastName} />
      </label>
    </form>
  );
}
```

Other things to know about the state value:

- It's private - unless it's is passed through props, a state value can only be referenced in the component where it's defined.
- We don't need to worry about using too many `useState`s. React batches state updates together to keep the render cycle efficient.

#### useEffect

`useEffect(setup, dependencies?)`

> [!note]
> A question mark (on `dependencies?`) is a common way to indicate an argument is optional

The `useEffect` hook allows us to synchronize a component with data found outside the application. This could be an API request, accessing local storage, managing a non-React feature on the same page, or any other scenario where we need to "step outside" of React. This hook takes two arguments: a setup function, and an optional dependency array.

The setup function contains logic needed for the effect and optionally returns a cleanup function. A cleanup function is used to for any necessary cleanup tasks before the component unmounts or before the effect is re-run. This can include clearing timers, unsubscribing from subscriptions, or any other clean-up activities to prevent memory leaks or unexpected behavior in a React component.

The dependency array is an optional list of props, state, and variables and functions defined directly in the component. After a component renders, if any of these have changed, React re-runs the `useEffect`. If this array is empty, the effect is ran only after the initial render. It will not do anything on subsequent re-renders. If the dependency array is omitted, the `useEffect` runs after first render and after each subsequent re-render.

For the example below, we've implemented another counter component. Each time the button is clicked, it updates count. After the component re-renders, React looks at the useEffect's dependencies - if any of the dependencies have changed, it will then re-run the effect. In this case, each time the component re-renders, the `useEffect` logs out a message and the `count` value to the console.

```jsx
//useEffect with dependency array

function AnotherCounter() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    // This code will run when the component first loads
    // and each time `count` changes
    if (count === 0) {
      console.log(`Count has started at: ${count}`);
    } else {
      console.log(`Count has changed to: ${count}`);
    }
  }, [count]);

  return (
    <>
      <button onClick={() => setCount(count + 1)}>Increment Count</button>
      <p>Count: {count} </p>
    </>
  );
}
```

In `WebSocketComponent` below, we use the `useEffect` to create a new web socket connection after the component first renders. The connection remains intact during subsequent re-renders of the component. The `useEffect` returns a cleanup function that disconnects the component from the web socket when the component is removed from the DOM. If this step was not included, the web socket connection would remain open and would not close until the application was closed out or the page manually refreshed.

```jsx
//useEffect with dependency array and cleanup function

function WebSocketComponent {

  useEffect(() => {
    const socket = new WebSocket('sockets://example.com/socket');

    socket.onopen = () => {
      console.log('WebSocket connection opened');
    };

    socket.onclose = () => {
      console.log('WebSocket connection closed');
    };

    return () => {
      console.log('Cleanup - Closing WebSocket connection');
      socket.close();
    };
  }, []);

  return <div>WebSocket Component</div>;
};
```

The `useEffect` in the example logs a message to the console when the component first renders. It does nothing else on subsequent re-renders.

```jsx
//useEffect with empty dependency array
const ExampleComponent = () => {
  useEffect(() => {
    console.log('Component mounted');
  }, []);

  return <div>Example Component</div>;
};
```

In the final example, the `useEffect` logs a message to the console on the initial render and after each re-render.

```jsx
//useEffect with no dependency array
const ExampleComponent = () => {
  useEffect(() => {
    console.log('Effect without dependencies - runs on every render');
  });

  return <div>Example Component</div>;
};
```

#### useRef

`const ref = useRef(intialValue)`

`useRef` is a React Hook that returns an object whose `current` property is initialized with the provided value. The ref's `current` property can be used to hold a value that we can change but remains intact across component re-renders. A change a ref does not trigger a component re-render. `useRef` is commonly used to access DOM nodes and managing focus on input fields.

In the following example, we store a reference to the page title and then use useEffect to keep it updated with the latest `count` value.

```jsx
//useRef used in conjunction with useEffect to update a page title

function PageTitleUpdatingCounter() {
  const [count, setCount] = useState(0);
  const title = useRef(document.querySelector('title');

  useEffect(() => {
    // This code will run when the component first loads
    // and each time `count` changes
    title.current.text = `Times pressed: ${count}`
  }, [count]);

  return (
    <div>
      <button onClick={() => setCount(count + 1)}>Increment Count</button>
    </div>
  );
};

```

Using `useRef` comes with a few rules.

1. This value can be updated during initialization but should never be read or written during the re-render cycle. This means that we should only be working with it in other hooks or functions defined in the component. Changing or reading the value at the top level of the component after the first render leads to unpredictable behavior:

```jsx
//do NOT do this:
function BadComponent() {
  const someValue = useRef('foo');
  someValue.current = 'bar'; //accessing or changing here can lead to problems
  //rest of the component code...
}
```

Updating the value during initialization is alright so long as the initial value is predictable.

```jsx
//this is OK
function ExampleComponent() {
  const title = useRef(null);
  if (title.current === null) {
    title.current = document.querySelector('title');
  }
  //rest of the component code...
}
```

1. Don't use it in the component's return body. These values are not meant for rendering and DOM references cannot be accessed outside of the component without additional tools. [`forwardRef`](https://react.dev/reference/react/forwardRef) helps with this but is an advanced topic so we will not be covering it in this course.

With hooks like useState, useEffect, and useRef, React developers can handle component state, side effects, and manage DOM elements outside the application. Understanding hooks is fundamental in modern React development and essential for building scalable and efficient applications. These are just a few hooks available and we will add new hooks to our toolkit as the weeks continue. We still have events to cover so let's keep on moving…

### Events and Handlers

#### Child Component to Parent Component Communication

Last week we discussed how props are used to communicate in a component tree from parent components down to their children. The data passed is used to configure a component's appearance when it renders but this does not solve the challenge of managing state across an application. Recall that props are read-only and can only be changed with the state update function provided by `useState`.

We cannot pass props up the component tree (child to parent), but we can take advantage of functions passed through props to communicate back to the parent. A function, when it is used this way, is referred to as a "callback" or a handler function. We can use state update functions as callbacks or we can define our own. When we invoke the callback in our child component, we are communicating that a state update needs to be processed. Depending on the the type of state being update, we can include a new value or allow the callback to determine what that new value needs to be.

```mermaid
---
title: Flow of Information in React
---
flowchart LR
Parent --->|props| Child
Child -->|callback invocation| Parent
```

#### Synthetic Events

Browsers emit objects called events that signal when things happen on a web page. This gives developers information about the event so that they can program behavior around it. React introduces a concept of a _React event object_, more frequently known as a _synthetic event_, which is very similar to a browser's _event object_. In fact, a synthetic event is just a wrapper around a normal event object with the main goal of having consistent event APIs across all the different browsers.

[React's documentation](https://react.dev/reference/react-dom/components/common#react-event-object) includes a full list of common properties and methods on the synthetic event. A few are worth mentioning because we will see them frequently:

- properties
  - **`currentTarget`**: DOM node where the event listener has been is attached.
  - **`target`**: DOM node where event was triggered - this could be anywhere in the tree at or below the Component that is reading this property due to [event bubbling](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Building_blocks/Event_bubbling).
- methods
  - **`preventDefault()`**: Prevents the default browser action on the event. eg: preventing a page refresh from a form submission.
  - **`stopPropogation()`**: Stops the event from bubbling up the React tree.

#### Handler Props

React includes an [exhaustive list](https://react.dev/reference/react-dom/components/common#common-props) of built in handler props that we can add to our components. Each one is designed to take a callback function and pass it a synthetic event that it can optionally consume. React adds additional properties to the synthetic event relevant to the event type whenever the listener is triggered. All of these event-based handler props have a prefix `on`, as in `onClick`, `onChange`, etc. Since there are 14 event types, we have to prioritize learning event types and props that relate to our project. We can save the other event types less common handler props for future exploration.

- `MouseEvent` - events that involve mouse navigation
  - example handlers: `onClick`, `onMouseEnter`, `onMouseOver`, and `onMouseLeave`
  - properties added to the synthetic event: which button is pushed, keyboard modifiers, cursor location, cursor movement
- `TouchEvent` - events from interacting with screen interfaces like cell-phones or tablets.
  - `onTouchStart`, `onTouchMove`, `onTouchEnd`, `onTouchCancel`
  - properties added to the synthetic event: keyboard modifiers, touches, and targets of touches
- `InputEvent` - events involving editable inputs such as check boxes, text fields, text-areas etc.
  - `onChange`, `onSelect`
  - properties added to the synthetic event: keyboard modifiers

### Updating State

#### Handler Composition Options

React provides a great deal of flexibility when it comes to composing handler props and handler/callback functions in our applications. Creating a custom callback in the parent or the child are equally acceptable. To help with readability, it is common to prefix a custom function's name with "handle", as in `handleClick`, `handleUpdatePassword`, or `handleAddItemToCart`. This approach lets us know that data related to an event is being handled. The example below transforms a email address to all lowercase letters before it is passed to `setEmail`. It also prevents the default form behavior which is to trigger a page refresh.

```jsx
//setEmail is a state update function provided as a callback by the parent component
function EmailInput({ setEmail }) {
  const emailInput = useRef();

  function handleSubmit(event) {
    //prevents page refresh
    event.preventDefault();
    const lowerCaseEmail = emailInput.current.value.toLowerCase();
    setEmail(lowerCaseEmail);
  }
  return (
    <form onSubmit={handleSubmit}>
      <label>
        Email:
        {/*assigning ref selects the element on the page*/}
        <input type="text" ref={emailInput} />
      </label>
      <button />
    </form>
  );
}
```

In the example below, we've created 2 custom callbacks, `increment` and `decrement` and then pass them each to an instance of a `Button` component. When clicked, `onClick` invokes the callback which uses `setCount` to update state with the appropriate value.

```jsx
//Parent component using custom callbacks on 2 buttons

function YetAnotherCounter(){
 const [count, setCount] = useState(0);

 function increment(){
  setCount(count + 1);
 }
 function decrement(){
  setCount(count -1);
 }
 return (
 <>
  <Button updateCount={increment} text={"increase"}/>
  <Button updateCount={decrement} text={"decrease"}/>
  <p>Counter: {count}</p>
 </>
  );
 )
}
//each instance of Button above behaves differently because it was given a different callback
function Button({updateCount, text}){
 return (
  <button type="button" onClick={updateCount}>{text}</button>
 )
}
```

We can even add an inline arrow function on a handler props directly.

```jsx
//inline arrow function

function Counter() {
  const [count, setCount] = useState(0);
  return (
    <div>
      <button onClick={() => setCount(count + 1)}>Increment Count</button>
      <p>Button pressed {count} times.</p>
    </div>
  );
}
```

#### Putting it all Together

We need to make a minor change to the inventory. Currently, we're not using the updater function for the cart but we will when transition over to API use. Let's implement `useEffect` to load in the inventory in preparation for this move. To make sure we load the inventory only once, we include an empty dependency array in the `useEffect`.

```jsx
//App.jsx
//...component code
const [inventory, setInventory] = useState([]);
useEffect(() => {
 setInventory([...inventoryData.inventory);
}, []); //<--- don't forget the dependency array or you can end up with an infinite loop!!
//...component code
```

It loads the same as before but now the code is a bit more flexible for future changes.

![alt](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/lessons/assets/week-04/render-priority-product-list.png)

Let's fix up the styling to make it look more like a storefront and then add a shopping cart! For now, we'll add classes to elements so that they're easy to select and add style rules to `App.css` - the same that we would do with a static html page. We'll talk about css and images in depth in week 10 because there are other, more flexible options for styling in React, especially as an app grows and becomes more complex.

![product cards](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/lessons/assets/week-04/product-cards.png)

For a cart feature, we need to be able to track items that have been put into a cart. To do so, we'll to add another `useState` to hold and update the items a user has selected. The initial state will be an empty array and we need handler function to add items from the cart. Removing items will come next week when we work with forms. We're also going to remove the special limited edition tee to simplify the product list. It will end up causing bugs because it's not a part of the inventory that we import from `catalog.json`. Sorry Frank, it'll come back later!

```jsx
//App.jsx
//...component code
const [cart, setCart] = useState([]);
function addItemToCart(item) {
  setCart([...cart, item]);
}
function removeItemFromCart(id) {
  const updatedCart = cart.filter((item) => item.id !== id);
  setCart([...updatedCart]);
}
//...component code
```

There's a problem with this implementation… what if a user adds the same item to their cart twice? That would mean there is more than one item in the cart with the same `id`. As it's implemented, the `id` that's used would filter out all the inventory items with a matching `id`. That's not a desired behavior so we need to come up with a unique cart item identifier.

We might employ `Math.random()` to generate a random number but this is not a great idea. [`Math.random`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/random) only generates pseudo-random numbers and we may end up with the identical id every once in a while. A better option would be the `Date.now()` method. This returns a number, in milliseconds from January 1, 1970, UTC. In production, we would probably use a UUID library but this works for our purposes. As we add an item into the cart, we'll assign it a `cartItemId` with the timestamp so we are better able to manage the cart.

```jsx
//App.jsx
//...component code
const [cart, setCart] = useState([]);
function handleAddItemToCart(id) {
  const target = inventory.find((item) => item.id === id);
  //if no inventory items are found
  //we want to prevent the app from crashing
  //by exiting this function now
  if (!target) {
    console.error('cart error: item not found');
    return;
  }
  //create an new object, spread the contents of the item selected
  //and add a `cartItemId`
  const cartItem = { ...target, cartItemId: Date.now() };
  console.log(cartItem);
  setCart([...cart, cartItem]);
}
//...component code
```

Now that we have a handler function to update the cart state, we need to wire `handleAddItemToCart` to the UI so a user can add items to the cart. We will do so by adding buttons to each of the cards so that its product can added to the cart. This will not account for different t-shirt sizes or products that have color variations but we will address those cases after we get the basics of the cart in place.

1. Add the props `handleAddItemToCart={handleAddItemToCart}` to `ProductList`, then from `ProductList` pass down to `ProductCard` using `handleAddItemToCart={handleAddItemToCart}`
2. Create a button in `ProductCard`
3. We then add an `onClick` event prop to the button. Since we have to provide it with an argument unrelated to the synthetic event, we'll create an anonymous arrow function to call our handler with the correct args.

- `<button onClick={() => handleAddItemToCart(cartItemId)}>Add to Cart</button>`

With that done, we can now look at the state in `App` using our [React Dev Tools](https://react.dev/learn/react-developer-tools) (it's highly recommended that you have them installed!). The second State entry grows every time one of the buttons is clicked. When the entries are expanded, they contain all the details of the product but there is also a unique `cartItemId`.

![adding items to cart updates state](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/lessons/assets/week-04/add-product-state.gif)

Now that we have a state representation of the cart, we can add it to the page. The cart does not need to remain open all the time - it would get in the way of the products. We'll keep the cart minimized and add a cart icon with an item count in the upper-left corner which is controlled by the Header component.

1. We add in the icon and place it on the page and use styling to push it over to the side of the screen.
2. We then add a `cart` props to the Header component definition: `function Header({ cart }) {`
3. Back in App, we then add the cart props to the header tag: `<Header cart={cart} />`
4. To get an item count to appear, we calculate the length of the cart array.

Since we haven't talked about conditional rendering, we cannot work on displaying a full cart list. We would have to have this full list available to implement the `removeItemFromCart` handler function. For now, we'll update the `Header` so that it prints the shopping cart to the console. To run useEffect every time the component renders, we'll exclude the dependency list.

```jsx
//Header.jsx

//... component code
useEffect(() => {
  cart.forEach((item) => {
    console.log(item.baseName, item.cartItemId);
  });
  if (cart.length > 0) {
    console.log('--end of cart--');
  }
});
//...component code.
```

![print shopping cart to console](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/lessons/assets/week-04/add-product-console.gif)

> [!note]
> A handler function's name doesn't have to remain the same when it passed through props. We could have called the props `addItem` when defining ProductCard and then added that to the `onClick`
