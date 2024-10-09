---
title: Week-03
dateModified: 2024-10-09
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

- Props
- Common Component Props
- State

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

All of the information on an plain web page is composed of data of one type or another - text, images, structure, styling, etc. One thing to note though is that it is static. In other words, it does not change after the page loads. This remains true whether the page is created manually or assembled by a web server then served to a user. JavaScript allows us to bring dynamism to the equation. When going from a static web page to an SPA, we have to determine what data should load dynamically and what should remain static. To one extreme, we may limit it to a username in a page header. Towards the other extreme, everything on the page all the way down to the text inside of a button can be loaded dynamically. Most of the time, it's somewhere in the middle.

We have plenty of options when working with state. The simplest place to start is by defining a variable used inside our component. We can do this outside of the component or inside, before the return statement. We then reference that variable in the desired area inside our return statement surrounded by `{}`. When the component is rendered, React will insert the value in its place in the virtual DOM. One detail to keep in the back of your mind is that every time a function is re-rendered, any variables defined inside the component will be re-instantiated. Most of this time this is not a problem but this will come up later in the course when we focus on optimizing our code.

```jsx
import ctdLogo from './assets/mono-blue-logo.svg';
import './App.css';

const message = 'Coming Soon...'; //This is outside the function definition for App

function App() {
    const title = ' CTD Swag'; // This is inside the Component before the return
    return (
        <div className="coming-soon">
          <h1>{title}</h1>
          <div style={{ height: 100, width: 100 }}>
            <img src={ctdLogo} alt="Code The Dream Logo" />
          </div>
          <h2>{message}</h2>
        </div>
    );
}

export default App;
```

This approach does not give us a way to make updates to `message` or `title`. "Well, we could use `let` instead." Oh, no no no! This breaks one of the rules about writing a React component: state should never be mutated. Not only that, React has no way to tell if and when a variable gets updated. Let's see if we can demonstrate that:

- revisit declarative programming
- discuss immutable data
- useState
	- define useState
	- declare with initial state value
	- declare with an initializer function (callback that returns some initial state)
	- returns array containing a state variable and a state setter function
	- destructuring naming convention
	- more in Week 4

### Props

- props is a tool to communicate with child components
	- object containing key-value pairs that can be referenced in the component
	- values can be anything, including callback functions covered in Week 4
- passing values using JSX
- `children`
- reading values in component that takes props
	- destructuring
- setting default values
- passing props to descendents

### Common Component Props

- html attributes
	- reserved names
		- className vs class
		- htmlFor vs for
	- style
		- takes an object of property:value pairs
		- properties are camelCase except data-* and aria-*
		- more in Week 10
	- hidden
	- title
- common elements
	- form, option, input, textarea, select - Week 5
- `children`
- ref
- event handlers
	- frequently used: keyboard, mouse,
	- take callback functions
	- more on Week 4

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
