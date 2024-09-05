---
title: Week-02
dateModified: 2024-09-04
dateCreated: 2024-08-20
tags: [react]
parent: "[[Intro to React V3]]"
week: 2
topics: [components, jsx, reactDOM, troubleshooting]
content: lesson plan
---

# Week-02

## Introduction

### Topics Covered

- ReactDOM
- Components
- JSX
- Troubleshooting

### Lesson Objectives

By the end of this lesson, we will:

#### Objective 1: ReactDOM

- Explain the purpose and role of ReactDOM within React applications
- Use createRoot to render a React application in a web page

#### Objective 2: Components

- Define a component and explain their role in building a UI
- List React's common components
- Describe the anatomy of a component
- List the contents of a component file

#### Objective 3: JSX

- Understand advantages of using JSX syntax inside components
- Identify how JSX combines JavaScript with HTML
- List the rules of the JSX syntax
- Describe how Vite.js compiles JSX to JavaScript so the application renders in a browser

#### Objective 4: Troubleshooting

- Discuss common hurdles in React development and how error messages can be used to help developers troubleshoot their application
- Use browser debugging tools to pinpoint and resolve bugs in their code

## Discussion Topics

### ReactDOM

- explain how React attaches to a page
- define react-dom package
- demo createRoot

### Components

> [!drafting note]
> demo components with plain js until JSX

- high level overview
- list common components
- structure of a component
- defining a component
- component rules
- export

### JSX

- format overview
- esbuild transformation process
	- compare - contrast with plain JS
- rules of JSX
	- must return single element
	- close all tags XML-style
	- camelCase
		- aria-* + data-* remain as-is
	- "class" is a reserved word
	- whitespace between lines
- document fragment
- including JavaScript blocks with {}
	- don't forget to wrap object in additional {}
	- inline styles work with object

### Troubleshooting

- common hurdles while developing
- explain browser console
- examine 2 example error messages
- StrictMode component
- React dev-tools
	- install
	- extension walk-through

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

### ReactDOM

- [Client React DOM APIs (React docs)](https://react.dev/reference/react-dom/client)
- [createRoot (React docs)](https://react.dev/reference/react-dom/client/createRoot)

### Components

- [Describing the UI (React docs)](https://react.dev/learn/describing-the-ui)
- [Your First Component (React docs)](https://react.dev/learn/your-first-component)
- [Common Components (React docs)](https://react.dev/reference/react-dom/components/common)

### JSX

- [Components (Complete Intro to React v8 by Brian Holt)](https://react-v8.holt.courses/lessons/no-frills-react/components)
- [Writing Markup with JSX (React docs)](https://react.dev/learn/writing-markup-with-jsx)
- [JavaScript in JSX with Curly Braces (React docs)](https://react.dev/learn/javascript-in-jsx-with-curly-braces)
- [HTML to JSX](https://transform.tools/html-to-jsx)
- [Fragment (React docs)](https://react.dev/reference/react/Fragment)

### Troubleshooting

- [How to Handle Errors in React Applications (FreeCodeCamp)](https://www.freecodecamp.org/news/effective-error-handling-in-react-applications/)
- React Developer Tools
	- [Chrome Extension](https://chromewebstore.google.com/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi?pli=1)
	- [Firefox Add-On](https://addons.mozilla.org/en-US/firefox/addon/react-devtools/)
	- [Using React Developer Tools with Safari and other browsers without plugins](https://react.dev/learn/react-developer-tools#safari-and-other-browsers)
