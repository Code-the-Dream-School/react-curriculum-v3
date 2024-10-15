---
title: Week-06
dateModified: 2024-10-15
dateCreated: 2024-08-20
tags: [react]
parent: "[[Intro to React V3]]"
week: 6
topics: [project organization, refactoring, reusable components, testing]
content: lesson plan
---

# Week-06

> [!drafting note] #drafting-note
> we will probably expose the students to a repo that has pre-written tests rather than write their own. This week will probably have 2 submissions

> [!drafting note] #drafting-note
> We've sit on props destructuring assignment for this week

1. submission #1 - code updates to refactor project
2. submission #2 - diagnose 3-4 failed tests, stretch goal: write test for a component

## Introduction

### Topics Covered

- Reusable Components
- Refactoring
- Project Organization
- Testing

### Lesson Objectives

By the end of this lesson, we will:

#### Objective 1: Reusing Components

> [!drafting note] #drafting-note
> Take advantage of the variants property: phones have models, shirts, hats, jackets have different colors
> This may be a good way to deal with optional props with default values

- Discuss the value of reusable components and how they foster efficiency, scalability, and consistency in React development
- Implement reusable components that can be shared across an application

#### Objective 2: Refactoring out Sub-components

- Examine how refactoring out portions of an existing component into re-usable components improves code organization and readability
- Demonstrate the process of extracting logic and elements from components to craft sub-components

#### Objective 3: Organizing Files in a React Project

- Discuss essential methodologies for effective file organization in a React project
- Formulate an approach to organize our project

#### Objective 4: Testing React Components

- Discuss the fundamental importance of testing to ensure functionality, robustness, and stability in React components
- Compare various testing methodologies like unit testing, integration testing, and end-to-end testing
- Use Jest and the React Testing Library to test React components and troubleshoot bugs

## Discussion Topics

### Reusing Components

- keeps code dry - write once and instantiate as many instances as needed
	- certain elements tend to repeat themselves
		- may be styled differently but contain the exact same content
			- navigation menus
			- social media sharing buttons
		- may have the same structure but contain different data
			- they differ only in the data that is provided through props
			- product cards in a grid
			- gallery of profile cards
				- each card contains same characteristics - name, profilePicture, location, motto, etc.
			- list items
			- error, info, success, and warning dialogs
- instances configured through props
	- affect content, layout, color, even positioning
	- example props in action
		- dialogs: message, type (error, info, warning), callback
		- profile cards: name, profilePicture, location, motto, profileUrl
	- discuss setting default values
- component libraries provide re-usable components that:
	- can be used 1 or more times in an application
	- provide styling or animation through component definition
	- solve common accessibility or internationalization challenges
	- implement challenging layouts or complex behaviors
		- tables
		- calendars
		- carousels

### Refactoring Out Sub-components

- Components can split the application to organize its layout
	- each page/pane/screen should be its own component - we will call them "features"
		- props can be used to pass on application-wide state but may not be needed
	- features tend to be splittable into smaller, sub-blocks
		- headers and footers are common across web pages
		- single main, sidebar but could still be worth being its own component
		- #placeholder/demo-code 2 examples splittable features from [[[Code-Along - eCommerce Store]]]
		- features tend to be areas where state is managed
- Smaller feature or elements that appear in more than one place
	- cluster of social media share buttons
- State used across components siblings needs to be managed by the common ancestor
	- could be a parent, a parent of a parent, or deeper
	- implement useState in common ancestor then pass variable and updater function to sub-component instance
- Maintain local state and communicate major changes up
	- local state to sync user input to what is being displayed
	- invoke a callback that communicates changes to parent who manages application state
- 1 component for 1 purpose - within reason

### Organizing Files in a React File

> [!NOTE]
> There are a lot of discussions and even more opinions about how to structure projects. What we provide here is a sensible approach that works well with small to medium React projects. Larger projects or those that use prominent frameworks such as Next.js, Astro, Remix may need a different approach to organizing code.

- explore [[Code-Along - eCommerce Store]]

> [!drafting note] #drafting-note
> Clean the directory up to match the CTD Swag

```terminal
.
├── node_modules/
├── public/
	└── vite.svg
├── src/
    ├── assets/
	    └── react.svg
    ├── features/
	    ├── About.jsx
	    ├── Account/
		    ├── Profile.jsx
		    └── Orders.jsx
	    ├── Account.jsx
	    ├── Checkout.jsx
	    └── Store.jsx        
    ├── services/
	    └── localStorage.js
    └── shared/
	    ├── Footer.jsx
	    ├── Header.jsx
	    ├── NavigationMenu.jsx
	    └── Cart.jsx
├── .eslint.cjs
├── .gitignore
├── .prettierignore
├── .prettierrc
├── index.html
├── package-lock.json
├── package.json
├── README.md
└── vite.config.js

```

- group files by usage
- **public/**
	- static resources not used in code
	- files that need their file name to remain unchanged
	- content that is accessed by URL but not imported into application
- **src/** contains majority of working files
	- **assets/** - static resources that code needs to import
		- images (.jpg, .png, .svg, etc), json, csv, etc.
		- file names get hashed in production
			- programmatically ensures file names are unique
			- helps with [cache busting](https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching#cache_busting)
		- [plugins](https://github.com/vitejs/awesome-vite#plugins) available to optimize files (we will not use any of these)
			- image dimensions
			- compression
	- **features/** - pages of a website or whole screens of the app
		- `About`, `Account` , `Checkout`, and `Store` components
		- each feature can be broken into sub components
			- if used only in that feature, create a folder with the name of the feature
				- Account.jsx + Account/
			- if used elsewhere, place in shared
				- Footer, Header, NavigationMenu, Cart
	- **services/** - non-React functionality extracted from components
		- external connectivity: API calls, WebRTC connections, local storage
		- adapters to 3rd-party libraries
		- user authentication
	- **shared/** - re-usable components that can be used anywhere
		- common page elements

### Testing React Components

> [!drafting note] #drafting-note
> This section NEEDS attention
>
> [React Testing Library Tutorial (Robin Wieruch)](https://www.robinwieruch.de/react-testing-library/)
> What tools do we need
> - Jest (optionally [Vitest](https://www.robinwieruch.de/vitest-react-testing-library/))
> - React Testing Library (replacement for Enzyme)

- purpose of testing
- types of testing methodologies
	- unit - individual parts in isolation
	- integration - ensure components work together as a group or as a larger feature
	- end-to-end - tests that run across an entire stack that replicate user tasks to ensure functioning
- testing in React - focus on unit and integration tests
	- ensure rendering under different conditions
	- uses, updates state correctly
	- handles events correctly
	- not every function or component needs a test
	- test for functionality, not implementation
		- Testing Library reenforces this by working with DOM nodes, not component instances
- steps to testing
	1. install & configure test tools
	2. determine what to test
	3. write test
	4. run
	5. examine the output

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

### Reusable Components

- [Importing and Exporting Components (React docs)](https://react.dev/learn/importing-and-exporting-components)
- [How to build Reusable React Components (freeCodeCamp)](https://www.freecodecamp.org/news/how-to-build-reusable-react-components/)

### Refactoring

- [Common Sense Refactoring of a Messy React Component](https://alexkondov.com/refactoring-a-messy-react-component/)
- [Choosing the State Structure (React docs)](https://react.dev/learn/choosing-the-state-structure)
- [Don't repeat yourself (Wikipedia)](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself)

### Project Organization

- [How To Structure React Projects From Beginner To Advanced](https://blog.webdevsimplified.com/2022-07/react-folder-structure/)
- [A Better Way to Structure React Projects (freeCodeCamp)](https://www.freecodecamp.org/news/a-better-way-to-structure-react-projects/)
- [vitejs/awesome-vite: A curated list of awesome things related to Vite.js (GitHub)](https://github.com/vitejs/awesome-vite#plugins)
- [HTTP caching - Cache Busting (MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching#cache_busting)

### Testing React Components

- [React Testing Library Tutorial (Robin Wieruch)](https://www.robinwieruch.de/react-testing-library/)
