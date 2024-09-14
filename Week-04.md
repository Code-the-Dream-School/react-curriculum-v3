---
title: Week-04
dateModified: 2024-09-09
dateCreated: 2024-08-20
tags: [react]
parent: "[[Intro to React V3]]"
week: 4
topics: [basic hooks, events, passing props, updating state]
content: lesson plan
---

# Week-04

## Introduction

### Topics Covered

- Basic Hooks
- Passing Props
- Events
- Updating State

### Lesson Objectives

By the end of this lesson, we will:

> [!drafting note]
> clean up the Events and Updating State objectives - bit mixed up

#### Objective 1: Basic Hooks

- Understand the purpose of commonly used React hooks - useState, useEffect, and useRef.
- Demonstrate how to implement React hooks.

#### Objective 2: Passing Props

- Examine the details of passing props between components in React.
- Demonstrate sending and receiving data through props to build dynamic components
- Explain advanced techniques for working with props

#### Objective 3: Events

- Explain the purpose of the event object and how it relates to DOM events
- Examine the properties and methods found on a React event object
- Select the appropriate handler functions to capture event data and manage event propagation

#### Objective 4: Updating State

- Gain a deeper understanding about how useState works
- Explore techniques to use events and the state update function to update state

## Discussion Topics

### Basic Hooks

- used to access React or custom-build features in components
- hook categories - state, context, refs, effects
	- **state** - stores information like user input that is used to help render components
		- useState
		- useReducer which we will talk about in Week 11
	- **context** - useContext helps us access data from distant ancestor components - in week 11
	- **effects** - useEffect allows us to access external data - more in Week 7
	- **refs** - save and work with values that aren't used for rendering
		- internal to component instance - unique across all copies of the component
		- commonly used
			- to store a reference to a DOM object
			- keep information between renders - timeoutID, sentinel values, internal counters
		- updating doesn't trigger re-render like useState
		- initialized with an initial value (null when used to reference a DOM element)
		- accessed through `.current` - read and write
			- don't do so at the top level of a component - only in event handlers or effects
- more remain that have not been covered. Not going to use them but encourage you to look at them before the start of the final project - you may find something useful!

### Passing Props

- introduced last week to props - deeper dive here
- used to communicate display and configuration data
- communication flow
	- props passed down to/through children
	- callbacks called with arguments
		- arguments pass data up
	- when called, updates state if needed in ancestor component

### Events

- React even object - "synthetic event"
	- contains information about the event
		- common to all + frequently used: cancelable, currentTarget, target, bubbles
	- includes methods to handle behavior of the event
		- common: preventDefault, stopPropagation
- common props include handler functions
	- declarative at it again - not common to need to set up a listener - they're provided
	- conform mostly with DOM events
	- smooths out some inconsistencies between browsers
	- commonly used ones: onClick, onFocus, onBlur, onChange (input-specific)
- when an event happens on a target element, it calls relevant handler function if included
- a handler function's takes an argument of either a callback or an arrow function
	- callback - will automatically be passed the React event object (aka "synthetic event") when invoked
	- arrow function - can optionally be written to include the synthetic event object.
		- allows for more flexibility
		- commonly used to invoke a function passed down through props
		- has access to state defined in that component
		- more on that next week with controlled forms

### Updating State

- back to useState
	- updated using the set function found in the array returned by useState when it was initialized
- update is misleading - state in React is immutable
	- when the update function is called, it:
		- generates new state values
		- requests React to re-render component to refresh UI with new values
		- new values then replace the old state values and represent current state
	- Old state is not accessible unless it was saved in a ref or persisted in another way. We won't doing this in this course but this is a small detail that can come in handy later in your career.
- functions can be defined inside the argument for the event handler functions - good for simple use-cases like updating state local to the component `onChange={(e)=> setMessage(e.target.value)}`
	- arrow function invokes setMessage and passes the target value
- common to define handler functions in components for more control
	- work with an event, e.g., preventDefault(), event.target.value
	- can invoke state setter function passed down as props with a value that for the new state
	- if a handler function accepts an event object (or nothing), a reference (not an invocation) to a handler function can be passed to the event handler which will then call it when the target event happens on the element

> [!drafting note]
> ¿how do children components bubble up events?
>
> eg. Clicking span inside a button - I assume the event happens on the span and then the button get ahold of it and boy-o-boy is does a click-o event

## Weekly Assignment Instructions

### Expected App Capabilities

> [!drafting note]
> We will be adding localStorage as a caching mechanism. Remember to remove this feature when we introduce API usage in Week 7. We're not touching the concept of synchronizing them and they are just different ways of persisting data.

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

### Basic Hooks

- [Built-in Hooks (React docs)](https://react.dev/reference/react/hooks)
- [Mastering React Hooks: A Comprehensive Guide with Examples (dev.to)](https://dev.to/jaingurdeep/mastering-react-hooks-a-comprehensive-guide-with-examples-3e3b)
- [React Hooks Cheatsheet](https://react-hooks-cheatsheet.com/)

### Passing Props

- [Passing Props to a Component (React docs)](https://react.dev/learn/passing-props-to-a-component)

### Events

- [Responding to Events (React docs)](https://react.dev/learn/responding-to-events)
- [How to Handle Events in React (freeCodeCamp)](https://www.freecodecamp.org/news/how-to-handle-events-in-react-19/)
- [Common Components - React Event Object (React docs)](https://react.dev/reference/react-dom/components/common#react-event-object)

### Updating State

- [Updating Objects in State (React docs)](https://react.dev/learn/updating-objects-in-state)
- [useState (React docs)](https://react.dev/reference/react/useState)
