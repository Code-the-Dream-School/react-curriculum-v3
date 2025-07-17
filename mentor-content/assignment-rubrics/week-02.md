# Assignment Rubric, Week 02

## Expected App Capabilities

After completing this week's assignment, the student's app should:

- [ ] Contain a TodoList component that contains all todo code
- [ ] Contain a TodoForm component with
  - [ ] a non-functioning form with 1 input field and a submit button
- [ ] Display a h1 heading, a todo entry form, and the todos we added last week

## Technical Details

- [ ] App should return a heading, a TodoForm component instance, and a TodoList component instance
- [ ] `src/` directory should have 2 new component files: TodoForm.jsx and TodoList.jsx (case insensitive), each containing a component of the same name.
- [ ] TodoForm returns a form that contains:
  - [ ] an input and label pair with the correct htmlFor <-> id association
  - [ ] a button element with no props
- [ ] TodoList:
  - [ ] contains an array of todos objects that each include an `id` and`title`
  - [ ] returns an unordered list containing the todos mapped out to list items.

## Details to Watch For

- [ ] ensure that the students are using the same or very similar names to the assignment
- [ ] component names are in PascalCase
- [ ] mapped list items use unique keys (`todo.id`) and not the array's index
- [ ] TodoForm and TodoList do *not* take any props yet

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
