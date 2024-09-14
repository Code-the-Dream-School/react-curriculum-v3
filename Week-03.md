---
title: Week-03
dateModified: 2024-09-05
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

#### Objective 1: Props

- Understand props in React and how they facilitate parent-child component communication
- Demonstrate how to pass data from parent components to child components using props

#### Objective 2: Common Component Props

- Describe the importance of props in built-in components
- Identify common props such as className, handler functions, style, etc and describe their relation to HTML attributes

#### Objective 3: State

- Explain the importance of state in managing dynamic data within React components
- Demonstrate the usage of state with the useState hook

## Discussion Topics

### Props

- importance of data to render content in the user interface
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

### State

- bring back to importance of data to render opened in Props Topic
	- got to have a way to save + work with it
- revisit declarative programming
- discuss immutable data
- useState
	- define useState
	- declare with initial state value
	- declare with an initializer function (callback that returns some initial state)
	- returns array containing a state variable and a state setter function
	- destructuring naming convention
	- more in Week 4

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
