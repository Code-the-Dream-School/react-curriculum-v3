# Assignment Rubric, Week 06

>[!note]
>This week includes 2 student assignments. This rubric is only for the todo list.

## Expected App Capabilities

After completing this week's assignment, the student's app should:

- [ ] be organized by feature and shared components
- [ ] use the same label and input on new todo form and edit todo
- [ ] allow the user to edit their todo's title

## Technical Details

- [ ] `src` should contain 2 new directories: `features` and `shared`
- [ ] `features` should contain TodoForm.jsx and a sub-directory, `TodoList` which contains TodoList.jsx and TodoListItem.jsx
- [ ] `shared` should contain a new component file, TextInputWithLabel.jsx
- [ ] TextInputWithLabel component should
  - [ ] Use props for at least the `id`, `ref` `onChange`, `label`, and `elementId`.
  - [ ] Return a label and input with associated props
  - [ ] Replace TodoForm's existing label and input instances, ensuring that props are moved over.
- [ ] App should:
  - [ ] Include new helper function, `updateTodo` that takes in an edited todo which replaces the todo matching the edited todo's id.
  - [ ] Pass `updateTodo` as props to the TodoList instance. TodoList should then pass it along to the TodoListItem instance.
- [ ] TodoListItem should:
  - [ ] Use an `isEditing` state variable to toggle between a TextInputWithLabel instance or the pre-existing checkbox and span.
  - [ ] Use `workingTitle` state variable with an `initialValue` of `todo.title` from props that manages the form input while the user edits a todo.
  - [ ] Include a `handleCancel` event helper that resets the working title and sets `isEditing` to `false`.
  - [ ] Include a `handleEdit` event helper that takes an `event` and retrieves the value out of its target value to update `workingTitle`.
  - [ ] While `isEditing` is true, include buttons below the TextInputWIthLabel instance to cancel or edit the user's update that are wired to their respective helper functions.
  - [ ] The span containing the todo's title should include an `onClick` event listener that sets `isEditing` to `true`

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
