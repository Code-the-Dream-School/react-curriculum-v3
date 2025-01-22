# Script-Week-03

## Video 1 of 2: State

- everything on a page is composed of data
  - page structure and markup
  - text, multimedia
  - styling
- dynamic elements use data that changes
- state is managing data that changes over time
- start with variables
  - interpolating them into JSX as content
  - values can updated but React does not track them
  - changes violate state mutation rule
  - outside vs inside Component declarations - remember that with each render variables internal to components are re-instantiated every time
- discuss `useState`
  - brief definition
  - import
  - destructuring assignment convention: `const [noun, setNoun] = useState(intialState)`
  - discuss state variable, state updater function
  - discuss initialState
    - direct assignment (any type allowed)
    - assignment through variable (and/or import)
    - assignment with initializer function
      - must be pure functions
      - cannot take args
      - return of any type
  - demonstrate how incorporate in JSX
    - as an interpolated element/text
    - as a prop value
    - mapping over an array + returning JSX elements incorporating item data

## Video 2 of 2: Props and Common Component Props

_don't discuss updating objects - covered next week_

- short for "properties"
- tool to communicate state to children
- values are immutable and managed by parent
- can be any type
  - primitives
  - arrays
  - objects
  - functions - used to communicate back up to parent - discussed next week
- demonstrate passing props to child component
- demonstrate using props inside of that component
  - (optional) destructuring assignment to grab values out of props object - will be covered in [[Code The Dream/Intro to React V3/Curriculum/Week-06|Week-06]]
- highlight ESLint error for missing props validation
  - not using prop-types in this class (TS does this better anyways)
  - disable this rule in `config.eslint.js`
- students can safely disregard linting error for unused update functions for now
- discuss `key`'s role in React's diffing + rendering cycle
  - allows React to keep track of elements rendered from an array
  - `key` not used by component that it is added to
  - children components don't track their own position in the DOM - they have no knowledge of sibling components and work in isolation
  - discuss pitfalls of using array index as `key` value
- common component props (highlights - no need for lengthy explanations)
  - props for all built-in components
    - `children`
    - `ref`
    - `style`
  - props for standard DOM components
    - `className`
    - `htmlFor`
    - `on*` (onBlur, onClick, etc)
  - props for DOM components that accept user inputs
    - `disabled`
    - `value`
    - `onChange`
- closer look at `children`
  - used to pass down React elements
  - placed between opening and closing element tags
  - demonstrate usage in child component
