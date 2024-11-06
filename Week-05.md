---
title: Week-05
dateModified: 2024-11-06
dateCreated: 2024-08-20
tags: [react]
parent: "[[Intro to React V3]]"
week: 5
topics: [controlled components, forms]
content: lesson plan
---

# Week-05

## Introduction

### Topics Covered

- Conditional Rendering
- Controlled Components and Forms

### Lesson Objectives

By the end of this lesson, we will:

#### Objective 1: Conditional Rendering

- Incorporate conditional rendering techniques to show or hide elements based user interactions or received data

#### Objective 2: Controlled Components

- Explain controlled components and their importance in managing state
- Demonstrate how to use controlled components to build interactive elements that behave predictably and share user input back to the application

#### Objective 3: React Forms

- Explain the management of form data in React and recognize the form's role in collecting and validating user input
- Use a controlled form to handle form events and validate user input

## Discussion Topics

### Conditional Rendering

When we left off with CTD Swag, we had a shopping cart icon and a counter on the upper-right portion of the screen. This gave the user a count of the items in their cart but left them without the means to see everything they've select. A detailed shopping cart takes up a lot of screen real estate and gets in the way of the shopping experience. It's common to create components like these that can be toggled on and off the screen or changed in some other way.

Conditional rendering allows us to develop an interface that adapts to different states, user interactions, or data inputs. Developers can employ different techniques to hide and show elements, change styles, and update visual content to create interactive and personalized user interfaces. Before we create a shopping cart that we can show/hide, let's look at the techniques we can employ.

#### Ternary Operator

`condition? expressionIfTrue : expressionIfFalse`.

The ternary operator is a useful tool that evaluates a condition and then executes one of 2 expressions depending on the outcome. If true, the expression on the left executes. False causes the right-hand expression to execute. In either case, these expressions can be written in JSX which results in extremely compact code.

```jsx
//ternary operator that shows 1 of 2 components
const [isTruthy, setIsTruthy] = useState(true);
return(
	<>
		{isTruthy ? <TrueComponent /> : <FalseComponent /> }
		<button type="button" onClick=(()=> setIstTruthy(!isTruthy))>Toggle Value</button>
	</>
);
```

React allows us to use `null` in replacement for an expression so we can show or hide an element.

```jsx
//ternary operator to conditionally show a component
const [isRendered, setIsRendered] = useState(true);\
return(
	<>
		{isTruthy ? <SpecialComponent /> : null }
		<button type="button" onClick=(()=> setIsRendered(!isRendered))>Show/Hide Element</button>
	</>
);
```

This approach can also be used to conditionally provide values to props. In this case, we're changing the background color of the div to `green` if `isGreen` is true.

```jsx
//ternary operator to set an element's background color
  const [isGreen, setIsGreen] = useState(true);
  return (
    <>
      <div
        style={{
          backgroundColor: isGreen ? "green" : null,
          height: "100px",
          width: "100px",
        }}
      ></div>
      <button type="button" onClick={() => setIsGreen(!isGreen)}>Toggle green</button>
    </>
  );
```

To summarize, a ternary operator is a good approach to use when trying to render one of two components (or elements) or choose between 2 values to provide a props. It can also handle the optional display of an element if the second expression is `null`.

#### Logical && Operator

We can take advantage of the way JavaScripts logical `&&` operator works to show or hide an element. Remember that JavaScript evaluates the left side (operand) first before continuing onto the right hand side. When the first operand is truthy, the second operand is evaluated - that is where we return our element. If that first operand is false, JavaScript stops the entire evaluation which prevents the item from being rendered.

```jsx
//logical && operator to conditionally show a component
const [isRendered, setIsRendered] = useState(true);
return(
	<>
		{isTruthy && <SpecialComponent /> }
		<button type="button" onClick=(()=> setIsRendered(!isRendered))>Show/Hide Element</button>
	</>
);
```

Using the logical `&&` operator is an extremely compact way to conditionally render a component or element.

#### Package Display Logic in a Function

In cases where we have more than two options or our rendering logic is complex, we can define functions inside the component and use their return values. What's nice about this approach is that the JSX rules apply to their return statements the same as they do for component return statements. We'll talk about some performance implications in [[Code The Dream/Intro to React V3/Curriculum/Week-09|Week-09]] that may arise from functions inside components but we will not be writing anything that will cause any performance problems during this course.

In the example below, the interface renders a button that calls `cycleLight`. Each time it does, it cycles through traffic light colors (green->yellow->red->green->…and so on). Each time `trafficLightColor` is updated, the component is re-rendered, calling `renderLight`. `renderLight` uses a switch-case statement to determine the correct component to render.

```jsx
const [trafficLightColor, setTrafficLightColor] = useState('green');

function cycleLight(){
	switch(trafficLightColor){
		case "green":
			setTrafficLightColor("yellow");
			return;
		case "yellow":
			setTrafficLightColor("red");
			return
		default:
			setTrafficLightColor("green");
			return;
	}
}

function renderLight(){
	switch(trafficLightColor){
		case "green":
			return <YellowLight />
		case "yellow":
			return <RedLight />
		default:
			return <GreenLight />
	}
}

return(
	<>
		{renderLight()}
		<button type="button" onClick={cycleLight}>Cycle Light</button>
	</>
)
```

#### Putting Conditional Rendering into Action

> [!tip]
> When we need to see dynamic elements in our layout but are not ready to code them, we add placeholders in the JSX. These can be plain text substitutions for values like a cart's total price or placeholder images similar to the one found on the product's card. This helps us determine our layout and anticipate any tasks that we are going to have to complete. As we continue to build the feature we can replace these substitutions with live code.

##### Create Cart Component

Looking back at CTD swag, we can now determine which approaches to use that best suites our needs at each step of building out the feature.

To get started, we create a cart component which contains the contents of the shopping cart, a cart total, and a button to close the cart. We need to include props for the cart and the handler function that handles closing the cart. The inside an unordered list, items are mapped to `li.cartListItem` similar to how the products were mapped to a list of cards in the store. We can then include a cart total and a button to close the cart. We pass an `onClick` props to the button with the handler function.

```jsx
//Cart.jsx

import placeholder from './assets/placeholder.png';

// `handleCloseCart` is not made yet but we know we will need it
function Cart({ cart, handleCloseCart }) {
  return (
    <div className="cartListWrapper">
      <ul className="cartList">
        {cart.map((item) => {
          return (
            <li className="cartListItem" key={item.cartItemId}>
              <img src={placeholder} alt="" />
              <h2>{item.name}</h2>
              <div className="cartListItemSubtotal">
                <p>${item.price}</p>
              </div>
            </li>
          );
        })}
      </ul>
      {/* cart total will need to be calculated */}
      <h2>Cart Total: $0.00</h2>
      <button onClick={handleCloseCart}>CloseCart</button>
    </div>
  );
}

export default Cart;

```

### Track Cart's Display Status

We next move to the `App` component to add the cart to the UI. The logic to show and hide the cart is simple so we do not need to extract a function to perform our conditional rendering. We are left with ternary operator or the logical `&&`. Both choices are valid but implementing the feature with the logic `&&` operator will leave us with the least amount of code. Let's add the toggle-able cart underneath the `ProductList` component.

We need a boolean state value that starts out with a value that hides the cart when the SPA is first visited.

`const [isCartOpen, setIsCartOpen] = useState(false);`

We then conditionally render the `Cart` based on `isCartOpen`. The example below provides the `App`s full return statement to help show how it relates to the other screen elements rendered out by `App`.

```jsx
// App.jsx excerpt

return (
    <>
      <Header cart={cart} handleOpenCart={handleOpenCart} />
      <main>
        <ProductList
          inventory={inventory}
          handleAddItemToCart={handleAddItemToCart}
        ></ProductList>
        {/*`isCartOpen has to be true for the cart to be rendered*/}
        {isCartOpen && <Cart cart={cart} />}
      </main>
      <footer>
        <p>
          Made with ❤️ | &copy; {year.current}{' '}
          <a href="https://codethedream.org/">CTD </a>
        </p>
      </footer>
    </>
  );
```

Let's open the React dev tools to see how things are working. We don't have out button wired up yet but we should be able to modify that state value to show/hide the cart.

![[202411_0445PM-Firefox Developer Edition.gif|700]]

### Opening and Closing Cart

The cart is working as expected so now it's time to add handlers to open and close the cart.

```jsx
//App.jsx
//component code...
  function handleCloseCart() {
    //prevents re-render if unchanged
    if (isCartOpen) {
      setIsCartOpen(false);
    }
  }

  function handleOpenCart() {
    //prevents re-render if unchanged
    if (!isCartOpen) {
      setIsCartOpen(true);
    }
  }
  //component code...
```

We then need to add the props for the handler function into the component definition of the `Header`. When that's been added, we go back to `App` and pass the props into the `Header` `<Header cart={cart} handleOpenCart={handleOpenCart} />`. We complete the handling for opening of the cart by wrapping the image with a button element and then adding in an `onClick` props with the handler function. We also can remove the `useEffect` since it was just to log the cart contents to the console. The `Header`'s code will now look like this:

```jsx
//Header.jsx
import { useEffect } from 'react';
import ctdLogo from './assets/icons/mono-blue-logo.svg';
import shoppingCart from './assets/icons/shoppingCart.svg';

function Header({ cart, handleOpenCart }) {

  return (
    <header>
      <div className="siteBranding">
        <img src={ctdLogo} alt="Code The Dream Logo" />
        <h1>CTD Swag</h1>
      </div>
      <div className="shoppingCart">
        <button type="button" onClick={handleOpenCart}>
          <img src={shoppingCart} alt="" />
          <p className="cartCount">{cart.length}</p>
        </button>
      </div>
    </header>
  );
}

export default Header;
```

Since we already have the `handleCartClose` in the props of the `Cart` component, it's now a matter of adding the handler in as props in `App`. `{isCartOpen && <Cart cart={cart} handleCloseCart={handleCloseCart} />}`.

![[202411_0529PM-Firefox Developer Edition.gif|500]]

#### Cleaning up the Cart

![[202411_0650AM-Firefox Developer Edition.gif|500]]

Currently, the cart displays item information but the total is static. It may also be nicer for the user to see a message rather than rendering an empty list. Showing a message or a list is another conditional rendering scenario to address. The logic to change between the two elements is still simple enough that we don't need to extract a function for conditional rendering. The `&&` operator will not work here either so we are left with using a ternary. We look at the `cart`'s length and then determine if it's empty or not. If `cart.length === 0` then the lefthand expression executes, showing the message. If it contain items, the righthand executes, showing the list.

```jsx
// extract from Cart.jsx
//...component code
{cart.length === 0 ? (
	<p>cart is empty</p>
) : (
	<ul className="cartList">
		{cart.map((item) => {
			return (
				<li className="cartListItem" key="{item.cartItemId}">
				<img src="{placeholder}" alt="" />
				<h2>{item.name}</h2>
				<div className="cartListItemSubtotal">
					<p>${item.price}</p>
				</div>
			</li>
		); })}
	</ul>
)}
//component code...
```

We are left with updating the cart total. It's not important to hide this total from the user so we'll add the cart total after the ternary block. To calculate the total, we iterate over the `cart` items to add up the price. Since the logic to do this is a little more complicated, we create a function to return the cart total for display.

```jsx
//extract from Cart.jsx

function Cart({ cart, handleCloseCart }) {
  function getCartPrice() {
    // floating point arethmetic cannot fully represent normal
    // math and introduces suprising rounding issues.
    // eg: .99 + .99 +.99 === 2.9699999999999998;
    // teams would normally use a mathematics or a currency library in
    // a live application because money is a bad thing to get wrong
    return cart.reduce((acc, item) => acc + item.price, 0).toFixed(2);
  }
	return (
		<div className="cartListWrapper">
		//...component code
				<h2>Cart Total: ${getCartPrice()}</h2>
		    <button onClick={handleCloseCart}>CloseCart</button>
	    </div>
	);
}

export default Cart;
```

---

### Controlled Components

Our shopping cart looks pretty good but there are a few improvements that can be made. In shopping carts on other sites, users are able to make changes to their carts. They can remove items and they can change the quantity of items in the cart.

![[202411_0700AM-Firefox Developer Edition.gif|400]]

- not all user activity needs to be kept long term but needs to be kept in state
	- commonly used on interactive elements
		- inputs
		- textareas
		- radio buttons
		- check boxes
		- date/time picker
		- dropdown list selector
- normal interactive html elements do not need React to work on page
- giving React control of them lets us work with the user data much more efficiently
	- as a user types, even handler functions update locally maintained state
	- when the state updates, it the component re-renders
		- places all the the latest values into into the elements
		- re-renders are fast + efficient & feel just the same as normal interactive html elements
	- when a form is submitted we already have access to the inputed data
		- don't need to programmatically access form elements & retrieve their values
- inputs are bound to state and its update function
	- val = state
	- onChange = a function that updates the state variable
		- arrow function passing event target value to a state update function
		- handler function reference that uses the state update function
			- optionally does other work too - field validation, style changes, sends query parameters to an API for some filtered data

### React Forms

- frequently written as controlled components
	- gives us access to user data
	- allows parent component to fully control its behavior
	- allows us to add complex validation
		- aside: html element validation properties as a performant alternative
			- prefer html validation over programmatic wherever possible
			- work better than javascript but lack flexibility for complex validation
	- provide immediate user feedback on changes
- form's `action` prop takes a URL or function
	- URL: form will act like a normal html form
	- function: the function passed will be used to handle form submission
		- function is passed a
	- can use a default button
	- using action always causes form to use `post` method
- button type="button" since any button defaults to submit when in form
- using `onSubmit` instead of `action`
	- takes a function that can can work the synthetic submit event
	- note: the event contains data from the form but it's preferred to pull data from component state

> [!drafting note]
> this assertion needs proved pronto!!
> https://developer.mozilla.org/en-US/docs/Web/API/FormData
> https://react.dev/reference/react-dom/components/form

- using `onSubmit` instead of `action` cont…
	- preventDefault() - page refresh bad for app
	- pulls values from current state
	- passes data back to parent component by calling a callback that was passed down through props
	- parent component processes the data
		- persist application state
		- updates components affected by the form data
		- make API request
			- network call is independent of the form action so request can use any method (get, post, put, etc)
		- saves to localStorage
- general form conventions
	- associate a label for each input
	- inlining form element in label will associate them
	- `htmlFor` matches `id`
		- note: `for` is a reserved js keyword…like `class`
	- precede a form element with a sibling label
	- prefer the use of buttons over divs that look like buttons
		- it's common to see this but buttons are the more appropriate option
- The repeated usage of useState for each one of the state variables can make the component hard to maintain. We will cover ways to address this when we work with advanced state in [[Week-11]]. #placeholder/link-to-week-11

## Weekly Assignment Instructions

### Expected App Capabilities

After completing this week's assignment, the app should be able to:

- capability 1
- capability 2
- capability n

### Instructions Part 1

 1. [ ] step 1
 2. [ ] step 2
 3. [ ] step n

### Instructions Part 2

 1. [ ] step 1
 2. [ ] step 2
 3. [ ] step n

### Instructions Part n

 1. [ ] step 1
 2. [ ] step 2
 3. [ ] step n

## References and Further Reading

### Controlled Components

- [Input - Controlling an input with a state variable (React docs)](https://react.dev/reference/react-dom/components/input#controlling-an-input-with-a-state-variable)

### Forms

- [The Form Element (MDN)](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/form)
- [HTMLFormElement: submit event (MDN)](https://developer.mozilla.org/en-US/docs/Web/API/HTMLFormElement/submit_event)
- [How to Build Forms in React (freeCodeCamp)](https://www.freecodecamp.org/news/how-to-build-forms-in-react/)
