---
title: Week-07
dateModified: 2024-10-09
dateCreated: 2024-08-20
tags: [react]
parent: "[[Intro to React V3]]"
week: 7
topics: [conditional rendering, data fetching, ui update strategies]
content: lesson plan
---

# Week-07

## Introduction

### Topics Covered

- Data Fetching
- Conditional Rendering
- UI Update Strategies

### Lesson Objectives

By the end of this lesson, we will:

#### Objective 1: Data Fetching

- Discuss data fetching in React applications to send and receive data
- Implement data fetching to retrieve and save data to manage application state

#### Objective 2: Conditional Rendering

- Incorporate conditional rendering techniques to show or hide elements based user interactions or received data

#### Objective 3: UI Update Strategies

- Examine pessimistic and optimistic UI update strategies related to how they impact a user's experience
- Implement an optimistic UI update strategy and handle failure scenarios where we need to revert changes

## Discussion Topics

### Data Fetching

- fetch used to work with APIs -CRUD
	- pull in data to display
	- create and update data outside application
	- delete remote data
- async keyword used so that we can work with promise objects
- allows application to continue processing while awaiting a response
- once the promise settles, it returns the data will be transformed and introduced into the application
- An API error, network outage, or other interruption may cause the fetch to fail - we need to handle these
	- wrap in try/catch block

### Conditional Rendering

- may not want to show an element at all times
	- list may be empty - no items should render
	- item sold out - shouldn't appear in the store for sale
	- user not logged in - shouldn't show a link to get to an account
- ternary operator can show an either or
	- In React, putting null in the `or` portion will render into nothing
- boolean state variable, `isLoading`
- derive state where possible to avoid "impossible state"
	- too many useStates make managing state error-prone

### UI Update Strategies

> [!note]
> We will not be discussing `useOptimistic` hook. At the time of this writing, this feature exists only in the [canary](https://www.techtarget.com/whatis/definition/canary-canary-testing) version of React so is not suitable for normal use.

- interfaces rely on information changes to update their interface
- pessimistic - updates only after network operation completes
	- accurately reflects of state in a system - only displays changes after a successful server operation
		- servers will send persisted data back with the success response - this is the data that is added to React's state, not direct user input
	- simpler to code - no need for reversions in case of server or network errors
	- feels sluggish to users even on fast internet connections
- optimistic - updates immediately while network operation is still underway
	- sacrifices state accuracy to give the user a faster experience
	- user changes shown immediately
		- data updated in the background with the server's response
	- more complex to program than pessimistic
		- must manage a fallback state in case operation fails
		- UI should indicate when a display is transitional data
			- grayed out or italicized text
			- spinners
			- autosave success [toast](https://uxpickle.com/what-is-a-toast-notification/) messages
		- messaging must be clear to explain change reversions
			- error handling
		- optionally save transmitted data for recovery after failure (repopulate form fields so users don't have to do it again)

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

### Stretch Goal: Suspense

- Use the built-in Suspense component to display fallback content

## References and Further Reading

### Data Fetching

- [Using the Fetch API (MDN)](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch)
- [Fetch (javascript.info)](https://javascript.info/fetch)
- [Modern API data-fetching methods in React (LogRocket Blog)](https://blog.logrocket.com/modern-api-data-fetching-methods-react/)

### Conditional Rendering

- [Conditional Rendering (React docs)](https://react.dev/learn/conditional-rendering)
- [React Conditional Rendering (freeCodeCamp)](https://www.freecodecamp.org/news/react-conditional-rendering/)

### UI Update Strategies

- [Optimistic and Pessimistic UI Rendering Approaches (Medium)](https://medium.com/@whosale/optimistic-and-pessimistic-ui-rendering-approaches-bc49d1298cc0)
- [A Primer on Optimistic UI (Dan Imhoff)](https://imhoff.blog/posts/optimistic-ui-primer)
- [What is a Toast Notification? (UX Pickle)](https://uxpickle.com/what-is-a-toast-notification/)
- [What is Canary Testing? (TechTarget)](https://www.techtarget.com/whatis/definition/canary-canary-testing)
