# Assignment Rubric, Week 03

## Expected App Capabilities

After completing this week's assignment, the student's app should:

- [ ] now contain 5 component files (main, App, TodoForm, TodoList, TodoListItem)
- [ ] render each static todo in a TodoListItem component
- [ ] contain a paragraph that uses a value controlled by state

## Technical Details

- [ ] App component should
  - [ ] use useState for `newTodo` with an initial value set to some test text
  - [ ] contain an instance of TodoForm
  - [ ] contain a paragraph tag that uses `newTodo`
  - [ ] contain an instance of TodoList that replaces unordered list
- [ ] Includes a new file and component TodoList
  - [ ] All todos from App are moved into this file
  - [ ] Todos mapped into an unordered list to render a list of TodoListItem component instances
- [ ] Includes a new file and component TodoForm
  - [ ] Contains a nonfunctional form with a single label/input pair and a button
- [ ] Includes a new file and component TodoListItem

## Details to Watch For

- [ ] No props passed to any component instance yet
- [ ] TodoListItem key should be `todo.id` and not the array's index
- [ ] TodoForm: form or button should not have any event handlers yet
- [ ] button can optionally be disabled or not

## Code Hygiene Recommendations

*We leave the code hygiene details to the discretion of the reviewer but also recommend a few general considerations:*

- [ ] consistent formatting and block nesting
- [ ] short, meaningful value names
- [ ] repo contains no unused code
  - [ ] no commented out code
  - [ ] no unused imports/variables
  - [ ] no redundant imports eg: `import React from 'react'`
- [ ] comments should be short and accurate
- [ ] ESLint and Prettier can automate formatting and bad practice identification. This leaves more space to focus on learning React and improves student/mentor interaction. Implementation instructions are in [week 1's stretch goals](https://github.com/Code-the-Dream-School/react-curriculum-v3/blob/main/learns-app-content/assignments/week-01.md#stretch-goals-instructions-optional)..
