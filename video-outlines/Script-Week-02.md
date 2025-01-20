# Week-02

## Video 1 of 4: ReactDOM

- Explanation of ReactDOM as it relates to React + web
- describe `Main.jsx` as a root file for the React
  - explain `createRoot`
    - identify `#root` in entry file, `index.html`
      - file served by vite
      - runs attached script
      - script takes over management of `#root` to insert app
  - explain `.render`
- include brief explanation of StrictMode since it sticks out

## Video 2 of 4: Components

- focus on functional components - no class components
- fundamental concept that underpins React
- serve as building blocks/modules
  - scopes functionality
  - reusability
- start with JavaScript syntax
  - JSX discussed next video
- `React.createElement(args)`
  - args (`type`, `props`, `…children`)
- differing ways to define a component
  - function declaration
  - function expression
  - arrow function
- component rules
  - Capitalized or PascalCase
  - pure functions (by React's definition)
    - idempotent
    - no side effects during render (except local mutations)
    - no mutation of state or props
- best practices
  - One component per file (excepting styled-components)
  - recommend using default export
- built-in components
  - `Fragment`
  - `StrictMode`
- ReactDOM common components
  - component representation include all HTML and most SVG elements

## Video 3 of 4: JSX

- JavaScript XML - syntax extension that combines JS with XML (similar to HTML but stricter)
- benefits of JSX
  - terser than JS
  - it's easier to read hierarchical content structures
    - closely represents output html - easier to visualize output
  - allows for embedding of JS aside markup
- JSX rules
  - return single element
  - terminate all tags (closing tag, self-closing tag)
  - keys and attributes must be camelCase
    - exceptions for `aria-` and `data-`
  - `class` and `for` are JS reserved words - replace with `className` and `htmlFor`
  - JS codeblocks must be wrapped in curly brackets
    - highlight that objects should be double-wrapped
      - commonly forgotten
      - eg: `style={{background-color: "blue:}}`
  - code blocks should have return value that is a valid React element
- JSX and code blocks can be nested
  - eg `<div>{isLoggedIn ? <p>Welcome {userName}!</p> : <button onClick={() => logIn()}>Log In</button>}</div>`

## Video 4 of 4: Troubleshooting

- runtime feedback
  - terminal and console messages
  - demonstrate missing `key`
- traces from application crashes
  - Vite includes code snippet and hints where possible
  - include restart instructions if Vite cannot recover
- React Developer Tools (demo on a complex project if available)
  - explain installation process
    - Chrome, Firefox, Edge
    - standalone version
  - describe utilities
    - walk through component inspector
    - show profiler in action
