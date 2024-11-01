---
title: Week-03
dateModified: 2024-10-31
dateCreated: 2024-08-20
tags: [react]
parent: "[[Intro to React V3]]"
week: 3
topics: [component attributes, props, state]
content: lesson plan
---

# Week-03

## Introduction

### Topics Covered

- State
- Props
- Common Component Props

### Lesson Objectives

By the end of this lesson, we will:

#### Objective 3: State

- Explain the importance of state in managing dynamic data within React components
- Demonstrate the usage of state with the useState hook

#### Objective 1: Props

- Understand props in React and how they facilitate parent-child component communication
- Demonstrate how to pass data from parent components to child components using props

#### Objective 2: Common Component Props

- Describe the importance of props in built-in components
- Identify common props such as className, handler functions, style, etc and describe their relation to HTML attributes

## Discussion Topics

### State

All of the information on an simple web page is composed of data of one type or another - text, images, structure, styling, etc. When going from a static web page to an SPA, we have to determine what data should load dynamically and what should remain static. To one extreme, we may limit it to a username in a page header. Towards the other extreme, everything on the page all the way down to the text inside of a button can be loaded dynamically. Most of the time, it's somewhere in the middle.

We have plenty of options when working with state. The simplest place to start is by defining a variable used inside our component. We can do this outside of the component or inside, before the return statement. We reference that variable in our JSX surrounded by `{}`. When the component is rendered, React will insert the value in its place in the virtual DOM then go through reconciliation and rendering. One detail to keep in the back of your mind is that every time a component is re-rendered, any variables defined inside the component will be re-instantiated. Most of this time this is not a problem but this will come up later in the course when we focus on optimizing our code for more advanced scenarios.

```jsx
import ctdLogo from "./assets/mono-blue-logo.svg";
import "./App.css";

const message = "Coming Soon..."; //This is outside the function definition for App

function App() {
  const title = " CTD Swag"; // This is inside the Component before the return
  return (
    <div className="coming-soon">
      <h1>{title}</h1> //`title` inserted into heading
      <div style={{ height: 100, width: 100 }}>
        <img src={ctdLogo} alt="Code The Dream Logo" />
      </div>
      <h2>{message}</h2> //`message` inserted into heading
    </div>
  );
}

export default App;
```

This approach does not give us a way to make updates to `message` or `title`. Using `let` instead of `const` allows us to make an update to the variable but it still breaks one of the rules for writing a React component: state should never be mutated. Not only that, React has no way to tell if and when `message` or `title` gets updated. We can demonstrate this with a setTimeout that changes `message` after 3 seconds.

```jsx
import ctdLogo from "./assets/mono-blue-logo.svg";
import "./App.css";

let message = "Coming Soon...";

setTimeout(() => {
  message = "We can feel it...";
  console.log(`Updated message: ${message}`);
}, 3000);

function App() {
  return (
    <div className="coming-soon">
      <h1>CTD Swag</h1>
      <div style={{ height: 100, width: 100 }}>
        <img src={ctdLogo} alt="Code The Dream Logo" />
      </div>
      <h2>{message}</h2>
    </div>
  );
}

export default App;

```

We'll know when the `setTimeout` fires off because of the console statement that prints out the updated `message`. As expected, this has no affect to the "Coming Soon…" message on the page:

![[202410_0222PM-Firefox Developer Edition.gif]]

#### useState

`const [state, setState] = useState(initialState)`

 `useState` is a React hook that allows us to set and update a piece of data that we can then use in our SPA. We invoke `useState` with an initial state value as an argument. That initial state value can be of any type. If given a function, it would be called an "initializer function" in that context. React will run it and use the returned value to set the initial state value. Initializer functions must be pure functions and cannot take any arguments. When called, `useState` returns an array containing a state variable (a reference to the current state) and an updater function. We follow array [destructuring assignment](https://javascript.info/destructuring-assignment) #placeholder/link convention `const [noun, setNoun] = useState(intialState)` to make use of this hook.

With `useState` explained, we can now start setting up CTD Swag's storefront. Let's get some inventory on the page! We first need to put together some sample inventory data for our app to start with. Each item should include `name`, `id` `price`, `description`, and `variants` for color variations, phone models for cases, etc. Garments also need a `size`. Here's an example of an inventory items:

```json
{
  items: [
    {
      name: "Bucket Hat",
      id: "hat01",
      price: 29.99,
      sizes: ["S", "M", "L"],
      description:
        "Protect your head from the sun with this stylish bucket hat",
      variants: [
        {
          color: "black",
          image: "bucket-hat-black.png",
        },
        {
          color: "peach",
          image: "bucket-hat-peach.png",
        },
      ],
    },
  ];
}

```

A JSON file that includes a full starting inventory can be found [here](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/Gists/ctd-swag-inventory.json). We will use Vite's JSON import feature to access the inventory. Let's get some product names and descriptions onto the page!

1. Add the JSON file to `/assets` and then add an import statement for it at the top of App.jsx.
2. Wrap everything in `<main></main>` so we continue to return a single element after adding the inventory list.
3. import `useState`
4. `inventory` contains an array of items that we want to display. Call useState with `inventory.items` as the initial value.
5. Destructure the returned array into a state variable, `inventory` and updater function, `setInventory`.
6. Create an unordered list below the `.coming-soon` div
7. Use JavaScript's `.map()` method to return an array of list items.
8. For now we'll use three `item` properties to create a card[^card] in the list item.
	1. **item.id**: used at the key for the list item so React can efficiently track the item for subsequent re-renders.
	2. **item.name**: used inside of the item card heading
	3. **item.description**: general details about the item displayed

```jsx
import { useState } from 'react';
import ctdLogo from './assets/mono-blue-logo.svg';
import './App.css';
import catalog from './assets/catalog.json';

function App() {
  const [inventory, setInventory] = useState(catalog.items);
  return (
    <main>
      <div className="coming-soon">
        <h1>CTD Swag</h1>
        <div style={{ height: 100, width: 100 }}>
          <img src={ctdLogo} alt="Code The Dream Logo" />
        </div>
      </div>
      <ul>
        {inventory.map((item) => {
          return (
            <li key={item.id}>
              <div className="itemCard">
                <h2>{item.name}</h2>
                <p>{item.description}</p>
              </div>
            </li>
          );
        })}
      </ul>
    </main>
  );
}

export default App;
```

![[202410_0948AM-Firefox Developer Edition.png|500]]

> [!note]
> ESLint may be highlighting `setInventory` since it hasn't been used yet. We will use it next lesson. For now, this is one of the few errors we'll ignore.

![[202410_1122AM-Code.png|400]]

The `key` in `<li key={item.id}>` helps React keep track of elements that are rendered from an array. It may initially seem like a good idea to use the item's array index since each item has one and it is unique. The downside is that an item and its index value are not guaranteed to keep matching. An item may be removed from the inventory, changing the array, when it goes out of stock. `inventory` can eventually have a sort or filter feature added to it which would also have an impact on item order. In either case, using array indices as keys could introduce unexpected behavior or degrade React's rendering cycle. We've included an `id` on the item so this will not happen.

### Props

#### Communicating with Children

At this point we are almost ready to do some refactoring. App component is small but will continue to grow as we set up our storefront. Remember that with "separation of concerns" it is good to limit each component to a single job. The title and the logo can be separated out as a storefront header. The inventory list and inventory cards area also good candidates for refactoring into components. Before we proceed, we need to know how to communicate data down to child components, otherwise we will have no way to pass on the `title` or `description`.

Enter React **`props`**. Short for "properties", the are used to pass data from a component down to its children. They can can accept any type, including functions, but their values are immutable and managed by the parent component. The components receiving props use the data to configure themselves. In order to take props in a component, a value representing props has to be added to the component's function parameters- `function SomeComponent(props){…`. Since props are just objects, it's common to use [destructuring assignment](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment) to immediately get specific values from props in the component's arguments. This allows us to immediately see what sort of values we need from the parent component. An additional benefit is that we can set a default value for any of the props.

```jsx
//without destructuring
function ProductList(props){
	const inventory = props.inventory
	return (
		<ul>
		    {inventory.map((item) => {
			    return (
					<li key={item.id}>
						{item.name}
					</li>
				);
			})}
	    </ul>
	)
}

//with destructuring
function ProductList({inventory = []){ //destructuring assignment grabs `inventory` out of props
									 //we're also setting a default value og `inventory` to an empty array
	return (
		<ul>
		    {inventory.map((item) => {
			    return (
					<li key={item.id}>
						{item.name}
					</li>
				);
			})}
	    </ul>
	)
}
```

Functions passed as props are a key tool for interactivity. This is such an important detail that we have to cover a few more topics before we can fully appreciate their role in an interactive application. We continue talking about functions used in props next week.

#### Props in Action

Let's expand on CTD Swag by introducing three new components.

We create the first two new files and define the components inside each:

- Header.jsx
- InventoryList.jsx

We then extract the elements from the App component and move them over into the new components.

```jsx
/*Header.jsx*/

import ctdLogo from './assets/mono-blue-logo.svg';

function Header() {
  return (
    <div className="coming-soon">
      <h1>CTD Swag</h1>
      <div style={{ height: 100, width: 100 }}>
        <img src={ctdLogo} alt="Code The Dream Logo" />
      </div>
    </div>
  );
}

export default Header;
```

```jsx
/*ProductList.jsx*/

function ProductList({inventory}) {
  return (
    <ul>
      {inventory.map((item) => {
        return (
          <li key={item.id}>
            <div className="itemCard">
              <h2>{item.name}</h2>
              <p>{item.description}</p>
            </div>
          </li>
        );
      })}
    </ul>
  );
}

export default ProductList;

```

**You may see an ESLint error when working with props in Inventory**.

![[202410_1114AM-Code.png|500]]

We will not be using prop-types in our project so we need to disable that rule in `config.eslint.js`. We do so by adding `'react/prop-types': 'off',` to the existing rules. The rules object should look similar to this:

```javascript
/*config.eslint.js rules extract*/
rules: {
      ...js.configs.recommended.rules,
      ...react.configs.recommended.rules,
      ...react.configs['jsx-runtime'].rules,
      ...reactHooks.configs.recommended.rules,
      'react/jsx-no-target-blank': 'off',
      'react/prop-types': 'off',                //disables react/prop-types rule
      'react-refresh/only-export-components': [
        'warn',
        { allowConstantExport: true },
      ],
    },
```

We then need to refactor ProductCard out of the ProductList component.

Create the file, `ProductCard.jsx` and move the list item over to the new component.

```jsx
/*ProductList.jsx*/

import ProductCard from './ProductCard';

function ProductList({inventory}) {
  return (
    <ul>
      {inventory.map((item) => {
        return (
          <ItemCard
            key={item.id}
            name={item.name}
            description={item.description}
          />
        );
      })}
    </ul>
  );
}

export default ProductList;
```

```jsx
/*ProductCard.jsx*/

function ProductCard({name, description}) {
  return (
    <li>
      <div className="itemCard">
        <h2>{props.name}</h2>
        <p>{props.description}</p>
      </div>
    </li>
  );
}

export default ItemCard;
```

> [!Remember]
> `key` is a special prop that React uses to track components rendered from an array. Because of this, `key` gets added to `InventoryItem` instances in `map()` but is **not used** in the `InventoryItem` component.

Finally, we refactor `App` to use the new components.

```jsx
import { useState } from 'react';
import './App.css';
import catalog from './assets/catalog.json';
import Header from './Header';
import ProductList from './ProductList';
import ProductCard from './ProductCard';

function App() {
  const [inventory, setInventory] = useState(catalog.items);
  
  }
  return (
    <main>
      <Header />
      <ProductList />
    </main>
  );
}

export default App;
```

#### Common Component Props

Along with the props that we can define on our own, React's common components feature numerous [built-in props](https://react.dev/reference/react-dom/components/common) that are worth exploring. Here are a few props highlights:

##### Props for All Built-in Components

- **children**: accepts a React node. Valid React nodes include custom or built-in components, array of React nodes, empty node (null, undefined), string, number, or a portal[^portal]. We'll cover children in more detail below.
- **ref**: takes a reference object from useRef (covered in [[Code The Dream/Intro to React V3/Curriculum/Week-04|week 4]]) or createRef, or a callback that gets called when React renders the element.
- **style**: takes an object defining CSS styles in property name/ property value pairs. All property names must be written in camelCase. Eg. `background-color` is written as `backgroundColor`. More in [[Code The Dream/Intro to React V3/Curriculum/Week-10#|week 10]].

##### Props for Standard DOM Components

- **className**: String. Replacement for html attribute `class`. Multiple classes can be added by using spaces between class names.
- **htmlFor**: String. Primarily for `label` or `output` and is a replacement for html attribute `for`.
- **on\* - (onBlur, onClick, onFocus, etc.)**: Takes a callback function. Event handler props are named after a specific event that they listen for on the element where they are used. More in [[Code The Dream/Intro to React V3/Curriculum/Week-04|week 4]]

##### Props for DOM Components that Accept User Input

> [!note]
> These include `<input>`,`<textarea>`, etc.
> We will cover these in depth when we start working with React controlled components and forms in [[Week-05|week 5]].

- **disabled**: Boolean. Prevents a user from interacting with element when true.
- **value**: String: the text contents inside the element.
- **onChange**: Accepts a callback function. Event handler function that fires when an update is made by the user to the element's value.

##### Children Props - A Closer Look

We are able to use a `children` prop to pass React elements into our custom components. Rather than assign `children` a value (`children={someValue}`), they are placed between the opening and closing tags for the element. For example, we may be trying to promote a specific item in our store and want to appear on the top, regardless of filters or sort order. We define that item in the App component and nest it inside `<ProductList></ProductList>` tags.

```jsx
/*App.jsx*/

import { useState } from 'react';
import './App.css';
import catalog from './assets/catalog.json';
import Header from './Header';
import ProductList from './ProductList';
import ProductCard from './ProductCard';

function App() {
  const [inventory, setInventory] = useState(catalog.items);

  function promoteItem() {
    return (
      <ProductCard
        name="Limited Edition Tee!"
        description="Special limited edition neon green shirt with a metallic Code the Dream Logo shinier than the latest front-end framework! Signed by the legendary Frank!"
      />
    );
  }
  return (
    <main>
      <Header />
      <ProductList inventory={inventory}>{promoteItem()}</ProductList> //invoking promoted item between the tags inserts the ItemCard
    </main>
  );
}

export default App;
```

In order for it to appear in `ProductList`, we need to include `{children}` in the desired area of our return statement.

```jsx
/*ProductList.jsx*/

import ProductCard from './ProductCard';

function ProductList({inventory, children}) {
  return (
    <ul>
      {children} //this location guarantees that this list item will be first
      {inventory.map((item) => {
        return (
          <ProductCard
            key={item.id}
            name={item.name}
            description={item.description}
          />
        );
      })}
    </ul>
  );
}

export default ProductList;
```

![[202410_0336PM-Firefox Developer Edition.png|600]]

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

### Props

- [Passing Props to a Component (React docs)](https://react.dev/learn/passing-props-to-a-component)
- [Common Component Props (React docs)](https://react.dev/reference/react-dom/components/common#common-props)

### Common Component Props

- [Passing Props to a Component - Familiar Props (React docs)](https://react.dev/learn/passing-props-to-a-component#familiar-props)
- [Common Components - Reference - Props (React docs)](https://react.dev/reference/react-dom/components/common#common-props)

### State

- [Managing State (React docs)](https://react.dev/learn/managing-state)
- [Preserving and Resetting State (React docs)](https://react.dev/learn/preserving-and-resetting-state)
- [useState (React docs)](https://react.dev/reference/react/useState)
- [State as a Snapshot (React docs)](https://react.dev/learn/state-as-a-snapshot)

[^card]: A card is the common term for a UI element that contains information on a single item out of a list of items. Often used as a layout tool to organize information and typically is linked to a details page for the item summarized in the card.
[^portal]: Portals allow React developers to render some children into a different part of the DOM.
