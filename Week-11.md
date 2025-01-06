---
title: Week-11
dateModified: 2025-01-06
dateCreated: 2024-08-20
tags: [react]
parent: "[[Intro to React V3]]"
draftStatus: draft
content: lesson plan
topics: [advanced state, useReducer]
week: 11
---

# Week-11

## Introduction

### Topics Covered

- Advanced State and useReducer
- Passing Data Using useContext

> [!drafting note] #drafting-note
> Stage: "big refactor" to prepare for routing and the addition of new features

### Lesson Objectives

By the end of this lesson, we will:

#### Objective 1: Advanced State

- Examine the use of useReducer to manage complex state
- Implement dispatch functions to update state

#### Objective 2: Passing Data Using useContext

- Explain the problems that that can be introduced by "prop drilling" and lifting state too high
- Describe how useContext can be implemented in deeply nested components to avoid prop drilling

## Discussion Topics

> [!drafting note]
> braindump
> - consolidating form state into a single useState
	- object in useState `{}`
		- can run into problems with this in advanced projects but this is fine here
		- the more advanced useReducer is better for this but we will that we'll explore it in
	- destructure the old state with key/value pairs extracted to make a new state object
		- `[event.target.name]: event.target.value`
	- pass the new state object to the state update function

### Advanced State

- managing a large number of state variables or event listeners can become challenging
	- hard to keep up with the way changes affect the application
	- leads to unpredictable UI behavior and conflicting states (bugs!)
- `useReducer`
	- replaces all `useState`s in a component
	- groups all the state logic together
	- allows us to think about the events that we respond to
		- we describe what happened
		- the reducer we write is used to update the state
		- we don't have to call multiple `set` functions in a row - it's too easy to forget one!
	- 3 concepts
		- action: objects used to describe the event
			- includes relevant data (id, date-time, etc.)
			- convention to add in a `type` with a short string that describes what happened
		- dispatch: function provided by useReducer to send the action and necessary state to the reducer when an event happens
		- reducer: function that updates state using an action received from a dispatch
			- reducer must be a pure function
				- same inputs guarantee the same result every time
				- no accessing external resource (network, Browser APIs, DOM elements, etc)
				- no side effects
				- does not mutate data
- replacing multiple state variables with `useReducer`
- cont…
	1. existing handler functions updated with actions - console.dir for now
	2. reducer function written to update state when an event triggers a dispatch
		- takes in current state + action object and returns updated state
		- defined outside of component (so it lives outside of render cycle)
		- use a [switch](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/switch) statement comparing `action.type` to different cases
		- and return the appropriate updates
			- switch statement with cases for each action type
	3. useReducer hook takes in the reducer and an initial state
	4. useReducer returns the current state and a dispatch function
		- current state configures UI just like state variables
		- dispatch added to handler functions taking in action

### Passing Data Using useContext

- useContext described
	- allows us to communicate into a deeply nested component without the need to pass state and updaters down through props down the whole tree.
	- allows a top level component create a "context" (interrelated conditions in which the data exists) that any of its descendants can access. Restated:
			1. component tells React that it has a state value and an updater that it wants to share with its descendants
			2. React makes that data available without having requesting it up and down a chain of parent-child relationships. "prop drilling"
- useContext put into use

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

### Advanced State and useReducer

- [Choosing the State Structure (React docs)](https://react.dev/learn/choosing-the-state-structure)
- [useReducer (React)](https://react.dev/reference/react/useReducer)
- [switch - JavaScript (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/switch)

### Passing Data Using useContext

- [useContext (React docs)](https://react.dev/reference/react/useContext)
- [A Guide to React Context and useContext() Hook ( Dmitri Pavlutin)](https://dmitripavlutin.com/react-context-and-usecontext/)
