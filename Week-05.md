---
title: Week-05
dateModified: 2024-09-15
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

- Controlled Components
- Forms

### Lesson Objectives

By the end of this lesson, we will:

#### Objective 1: Controlled Components

- Explain controlled components and their importance in managing state
- Demonstrate how to use controlled components to build interactive elements that behave predictably and share user input back to the application

#### Objective 2: React Forms

- Explain the management of form data in React and recognize the form's role in collecting and validating user input
- Use a controlled form to handle form events and validate user input

## Discussion Topics

### Controlled Components

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
