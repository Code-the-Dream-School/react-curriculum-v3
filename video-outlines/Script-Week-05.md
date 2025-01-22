# Week-05

## Video 1 of 2: Conditional Rendering

- importance of showing and hiding elements in a dynamic interface
- importance of changing content and visual elements based on user inputs, events, etc
- options
  - ternary operator:
    - good for choosing between to options to render - 2nd item can be `null` so nothing renders
  - && operator
    - good for showing + hiding something based off a truthy evaluation on the left-hand expression
  - extracting a function
    - good for complex logic
    - choosing between 3+ rendering options
      - return valid jsx - may also indicate need for sub-component if functionality discrete enough
- putting it into action
  - show examples of showing/hiding
  - show examples of changing data or results of element's props value update

## Video 1 of 2: Controlled Components and Forms

- controlled vs uncontrolled
  - uncontrolled
    - browser manages stat
    - more performant
    - independent of React's re-render cycle
    - default form behavior on submission
    - only basic html input validation
    - values have to be accessed from DOM each time used
  - controlled
    - React manages values and form behavior
    - continuous access to form values
    - live customizable validation
    - render elsewhere in app
    - more code to orchestrate value updates on screen
- value of working in local state
  - make a working copy from "single source of truth"
  - keep local state's working values as close to the element consuming them as possible
    -minimize re-render impacts to smallest element
    -keep "single source of truth" clean until ready to commit working change
    -can discard local state (cancel pending changes) instead of undoing changes to application state
- form demo with at least
  - how to copy app state to a local (working) state
  - explanation of how to wire input value to local state variable and onChange to state update function
  - validate a field (we'll discuss handling invalid fields in week 7 or 10)
  - wired submission button with a handler that
    - prevents default form behavior
    - gathers working state to update app state
  - wired cancel button that re-sets from single source of truth
