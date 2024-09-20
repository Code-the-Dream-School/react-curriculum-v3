---
title: Week-01
dateModified: 2024-09-20
dateCreated: 2024-08-20
tags: [react]
parent: "[[Intro to React V3]]"
week: 1
topics: [app installation, intro to react, project setup]
content: lesson plan
---

# Week-01

## Introduction

### Topics Covered

- Intro to React
- App Installation
- Project Setup

### Lesson Objectives

By the end of this lesson, we will:

#### Objective 1: Overview of React

- Understand React at a high level
- Explain the advantages React for developing web applications
- Define key React terms and concepts

#### Objective 2: App Installation

- Use the CLI to create a new project using Vite.js and its React project template
- List the dependencies and scripts in package.json.
- Use the CLI to install Vite.js, React, and dependencies

#### Objective 3: Project Setup

- Describe the role of Vite.js as a development server for React projects.
- Demonstrate how to run installed app on a local development server
- Describe the development workflow using the development server

#### Stretch Goal: Improving the Development Environment

- Explore options to improve the React development experience
- Configure eslint and Prettier

## Discussion Topics

### Intro to React

React is a frontend library used by developers to build **S**ingle **P**age **A**pplications (SPAs). Used along with [ReactDOM](https://react.dev/reference/react-dom) when developing for web browsers, React takes a declarative approach to DOM manipulation to provide interactivity in web applications. React takes care of:

- assembling components we define to render a UI
- listening for user events - mouse cursor hovering, typing in a field, button clicks, etc
- updating state[^state] based on events and other inputs
- automatically updating the UI when state changes

That's helpful but what problems does it solve?!

Developing an SPA without libraries (or even with jQuery) is a complicated process for anything but the smallest app. Prior to their rise of libraries like React or frameworks[^libraries-and-frameworks] like Angular and Vue, developers had to take an imperative approach to programming interactivity into an html document. Stated another way, developers have to programmatically describe the steps their SPA needs to keep the UI updated.

- Elements must be created, destroyed or managed.
- Event listeners have to be added, configured, and managed.
- Elements then need to be added, removed, replaced, or modified to update the UI.

Often, this includes adding or removing sub-elements that aren't known about ahead of time like list items or images loaded from a remote data source. Each element may need event listeners which in turn, are configured with logic to update the interface. Listeners also need to be managed carefully to keep the application performing smoothly. They don't automatically disappear when elements they are used on are removed or are no longer needed. Forgotten event listeners take up memory on a user's system and can cause serious performance issues or even browser crashes. In a highly interactive html page, this is a lot to manage.

The majority of the current front end libraries or frameworks use a declarative approach to programing a UI. Declarative programming allows us to define the SPA's structure and state. It is then library's/framework's responsibility to accomplish the all the tasks needed to keep the UI updated. This stands in contrast with the imperative approach, and as a consequence, it allows us to create complex SPAs with relative ease.

React's strength comes from its use of components and the way it keeps the UI updated. Components allow us to divide an SPA into smaller modules consisting of page elements, styling, and state logic. In programming terms, components encourage "separation of concerns". Separate components allow us to focus on small portions of the application at a time. Each component does one or a few things rather than having a large files (HTML, CSS, JS) that group all of their functionality together.

React provides us hooks which are functions that allow us to simplify state and logic management inside of our components. Props (properties) permit us to pass data and event information between components. We will cover hooks and props in future lessons so don't worry if they sound confusing right now.

Another key aspect of React is the [Virtual DOM](https://legacy.reactjs.org/docs/faq-internals.html#what-is-the-virtual-dom). This is a lightweight copy of the actual DOM (Document Object Model) in memory. When there are changes in the state of a component, React first updates the Virtual DOM and then compares it with the real DOM to identify the changes needed. This diffing process is more efficient than directly manipulating the entire DOM, resulting in faster updates and improved performance.

### App Installation

> [!drafting note]
> Remember that this is an eCommerce project for examples

- Introduce CTD Swag
- set up repo
- intro Vite
- explain development vs deployment servers
- use Vite CLI to bootstrap React template

### Project Setup

- explain React template
- install
- demo run
- describe how to work with dev server
	- running
	- live browser refresh
	- console error messaging
- template housekeeping

### Stretch Goal: Improving the Development Environment

- explain eslint
- install VS Code plugin
- configure eslint
- demo errors it raises
- explain prettier
- install VS Code plugin
- configure prettier
- demo formatting awesomeness
- run through project to update formatting

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

### Intro to React

- [A Guide to Key Concepts - What is React? (FreeCodeCamp)](https://www.freecodecamp.org/news/learn-react-key-concepts#heading-what-is-react)
- [Describing the UI (React docs)](https://react.dev/learn/describing-the-ui)
- [Thinking in React (React docs)](https://react.dev/learn/thinking-in-react)
- [Top Front-End Frameworks in 2024 (Geeks for Geeks)](https://www.geeksforgeeks.org/top-front-end-frameworks/)
- [Inversion of control (Wikipedia)](https://en.wikipedia.org/wiki/Inversion_of_control)
- [Virtual DOM and Internals (React docs)](https://legacy.reactjs.org/docs/faq-internals.html#what-is-the-virtual-dom)

### App Installation

- [Scaffolding your First Vite Project (Vite docs)](https://vitejs.dev/guide/#scaffolding-your-first-vite-project)
- [React + Vite template (GitHub repo)](https://github.com/vitejs/vite/tree/main/packages/create-vite/template-react)
- [@vitejs/plugin-react (GitHub repo)](https://github.com/vitejs/vite-plugin-react/tree/main/packages/plugin-react)
- [Command Line Interface (Vite docs)](https://vitejs.dev/guide/cli.html)

### Project Setup

 - [The "npm run" Command (npm docs)](https://docs.npmjs.com/cli/v10/commands/npm-run-script)
 - [The Basics of Package.json (NodeSource)](https://nodesource.com/blog/the-basics-of-package-json/)

### Improving the Development Environment

- [ESlint Core Concepts (ESLint docs)](https://eslint.org/docs/latest/use/core-concepts/)
- [VS CODE ESLint Extension (VS Marketplace)](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)
- [Prettier (Prettier docs)](https://prettier.io/docs/en/)
- [Prettier Formatter for Visual Studio Code (VS Marketplace)](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)

[^state]: State refers to the current condition or data within an application at a specific point in time. It includes all data relevant to the application, such as user input, server responses, and UI state.
[^libraries-and-frameworks]: Libraries and frameworks are code packages written to solve complex or specialized challenges. We incorporate libraries or frameworks into our projects to simplify the development process. The distinction between the libraries and frameworks is a nuanced topic and can be interpreted differently based on their programming language and what they're used for. "[Inversion of control](https://en.wikipedia.org/wiki/Inversion_of_control)" is a common element to most comparisons between the two.

	With a library, we are in control of how its code is called. Libraries provide a toolset that we use by calling application programming interfaces (APIs) that they provide. Conversely, a framework defines how the application works then calls our code to configure its behavior and to determine what to include. They tend to be larger in scope and provide more programming tools for us to use.

	**Example UI libraries**: jQuery, Knockout, Preact, and React.
	**Example UI frameworks**: Angular, Astro, Ember, Next.js, Remix, and Vue.
