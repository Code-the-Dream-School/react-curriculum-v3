---
title: Week-04
dateModified: 2025-01-06
dateCreated: 2024-10-31
parent: "[[Unsorted Notes|Unsorted Notes]]"
draftStatus: draft
---

# Week-04

## Events and Handlers

> [!note]
> removed 2024-10-31 - materials were rearranged and this did not fit

### Handling Events

#### Clicker Widget in HTML and JavaScript

```html
<!-- clicker widget written in html -->
<!-- other page html -->
<button id="clicker">Click me!</button>
<p id="times-clicked" data-click-count="0">Times clicked: 0</p>
<script src="clicker-app.js" defer />
<!-- other page html -->
```

```js
//clicker-app.js
const button = document.querySelector(".clicker");
const countReader = document.querySelector(".times-clicked");

function increment(){
	const nextValue = countReader.dataset.clickCount + 1;
	countReader.innerText = `Times clicked: ${nextValue + 1}`;
	countReader.dataset.clickCount = nextValue;
}
button.addEventListener("click", increment)
```

#### Clicker Widget Written in React

```jsx
//clicker written in a React component

import (useState) from "react";

export default ClickWidget(){
	const [count, setCount] = useState(0);
	return (
		<>
			<button onClick(()=> setCount(count + 1))>Click me!</button>
			<p>Times clicked: ${count}</p>
		</>
	)
}
```

Implementing event listeners in React is easier to read and is more succinct than the equivalent code in JavaScript, not including the libraries we are using. Developers lean on React to keep their code as clean, succinct, and as easy to update as possible. The two examples above illustrate the difference in the amount of code and readability between the approaches. With the HTML + JS approach, the code is a lot more involved. There's more to remember and the process of getting and setting state is much more involved. Here's a list of details that had to be considered while developing the above examples:

- Items to think about with HTML + JS
	- `<button>` and `<p>` have to be selected from the dom - we use `id`s to make selection easier.
	- the paragraph needs the `data-click-count` attribute to persist the state value in the DOM. We could parse the innerHTML but using a `data-*` attribute is easier so less prone to bugs
	- we attach an event listener to the button and include a callback function
	- in the callback we have to
		- retrieve the current state value, `clickCount`
		- we set the inner text with a new calculated value
		- we also set the paragraph's `data-click-count` attribute
- Items to think about with React
	- we import `useState`
	- we call `useState` with an initial value of 0 then retrieve a reference to state and a state updater function
	- we return a single React element containing the button and paragraph
	- in the button, we add `onClick` handler props and a callback (inline this time since it's short)
	- the callback function calls the state update function `useState` with an updated value
