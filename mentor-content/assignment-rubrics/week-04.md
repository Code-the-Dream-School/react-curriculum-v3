# Assignment Rubric, Week 04

## Expected App Capabilities

After completing this week's assignment, the student's app should:

- [ ] allow users add a todo
- [ ] retain the `input`'s focus when a todo is submitted with the button or enter key
- [ ] render entered todos in a list

## Technical Details

- [ ] App:
  - [ ] `newTodo` should be updated to `todoList` and take an initialState of an empty array
  - [ ] contains a new updater function used to add new todos, `addTodo(title)`. Function should:
  - [ ] TodoForm instance should take the updater function as a props
  - [ ] removes the paragraph instance from the JSX
  - [ ] TodoList should take the `todoList` as a props
- [ ] TodoForm:
  - [ ] destructures the add todo handler from props (`TodoForm({onAddTodo})`)
  - [ ] include a handler function that:
    - [ ] takes in an event object
    - [ ] prevents page refresh
    - [ ] retrieves the title value from `event.target.title.value`
    - [ ] creates a todo object: `{title, id: Date.now()};`
    - [ ] resets the `event.target.title.value` to empty string
    - [ ] invokes the state update function passed in by props and passes in `title`
  - [ ] form instance should
    - [ ] include `onSubmit` that takes the handler function defined in the component
  - [ ] input should include a name props, `title`
- [ ] TodoList:
  - [ ] destructures the todo list from props
  - [ ] removes the statically defined todos list and updates the map to use the version from props
- [ ] TodoForm:
  - [ ] useRef used inside the submit handler function to refocus on input after user submits todo

## Details to Watch For

- [ ] The form should still be uncontrolled
- [ ] Each todo object ID should be unique and use a static value (Date.now() is one approach, just make sure student uses something with very low collision possibility)

## Code Hygiene Recommendations

*We leave the code hygiene details to the discretion of the reviewer but also recommend a few general considerations:*

- [ ] consistent formatting and block nesting
- [ ] short, meaningful value names
- [ ] repo contains no unused code
  - [ ] no commented out code
  - [ ] no unused imports/variables
  - [ ] no redundant imports eg: `import React from 'react'`
- [ ] comments should be short and accurate
- [ ] ESLint and Prettier can automate formatting and bad practice identification. This leaves more space to focus on learning React and improves student/mentor interaction. Implementation instructions are in [week 1's stretch goals](https://github.com/Code-the-Dream-School/react-curriculum-v3/blob/main/learns-app-content/assignments/week-01.md#stretch-goals-instructions-optional).
