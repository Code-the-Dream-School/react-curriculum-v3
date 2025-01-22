# Week-01

## Video 1 of 4: Intro to React

- front end ui library to create interface with JS
- React DOM companion library helps with SPAs
- benefits of using framework
  - dom and event management
  - componentize codebase
- benefits of React
  - imperative vs declarative
  - separation of concerns + usage of components
  - grouping code by language vs grouping by functionality
  - traditional web separates web pages html/php, styles, and javascript
  - React philosophy single-function modules better than organizing code by language/filetype
  - hooks
  - Virtual DOM and diffing process to optimize rendering

## Video 2 of 4: App Installation

- using Vite with React template
  - replacement for aging create-react-app
  - easy way to get started
  - performant for small to medium React apps
- establish GH repo + clone locally
- explain and run `npm create vite@latest ./ -- --template react`
  - dissect command
- explain and run `npm install`
  - brief overview of scaffolded files and directories
- working with package scripts indirectly with `npm run {{someScript}}`
- start hello world app
  - mention port number
  - explain importance of running SPA while developing
  - (some students don't unless told)

## Video 3 of 4: Project Walk-Through

- discuss tools Vite uses
  - esbuild
  - Rollup
  - PostCSS (we won't be working with this but it's worth mentioning)
  - CSS Modules
- discuss Vite features
  - Hot Module Replacement (HMR)
  - TypeScript support
  - JSX transformation
  - CSS, JSON, blob importing
  - static assets
  - plugin ecosystem (mention it + provide links but don't dive in - that's [[Code The Dream/Intro to React V3/Curriculum/Week-13|Week-13]])

## Video 4 of 4: Stretch Goal - Improving the Development Environment

- explain static analysis and linting - useful tools for dx
  - VS Code intellisense does some highlighting but we can expand with ESLint
  - introduce ESLint
  - install
  - demo tooltips on errors (both intellisense and eslint versions -hint: unused variables trips both)
  - explain how to add rules or disable existing ones
  - per line
  - per file
  - `eslint.config.js`
- introduce code formatting with prettier
  - describe benefits
  - individual developers
  - working with team codebases
  - install and configure (format on save, auto formatting, formatting with `npm run ???`)
  - explain how to add, change rules with `.prettierrc`
  - demonstrate before and after
