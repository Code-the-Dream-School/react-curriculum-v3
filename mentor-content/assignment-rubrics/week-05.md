# Assignment Rubric, Week 05

## Expected App Capabilities

After completing this week's assignment, the student's app should:

- [ ] conditionally render a message when the todo list is empty
- [ ] disable the Add Todo button when the input is empty
- [ ] allow users to complete a todo
- [ ] utilize a controlled form

## Technical Details

- [ ] if app contains no todos, it should render a paragraph including a message that the todo list is empty or for the user to add a todo to get started
- [ ] each todo object should include a `isCompleted` property that defaults to `false` when the todo is created.
- [ ] **`App` component changes:**
  - [ ] `addTodo` helper function refactored:
    - [ ] now takes a `title` instead of a `todo`
    - [ ] creates a new todo using the passed in `title`, a permanently unique id (we used Date.now() but other approaches that guarantee uniqueness can be used), and `isCompleted` set to false.
    - [ ] use `setTodoList` to add the new todo to the end of `todoList`
  - [ ] New a helper function, `completeTodo`, that marks a todo as completed based on the todo `id`
  - [ ] The `TodoList` instance in `App` should have a new props that takes that helper function (eg:`<TodoList todoList={todoList} onCompleteTodo={completeTodo} />`)
- [ ] **`TodoList` component changes:**
  - [ ]  Props are updated to destructure out this helper function
  - [ ]  Now includes a helper function that returns only todos that are not completed
  - [ ]  The JSX should conditionally display a paragraph or an unordered list based on the filtered todos array length:
    - [ ]  if 0, render a paragraph with a message for the user.
    - [ ]  otherwise, render an unordered list from the filtered todos.
  - [ ]  the `TodoListItem` instances should have 3 props: `key={todo.id}`, `todo={todo}`, and `onCompleteTodo={onCompleteTodo}`
- [ ] **`TodoListItem` component changes:**
  - [ ] destructures the helper function out of props
  - [ ] the list item the component returns:
    - [ ] now includes a form element that contains an `input` of the type `checkbox` and the todo title
    - [ ] the checkbox in the form uses `onChange` to invoke the helper function to complete the todo and passes the todo's `id` in as an argument
- [ ] **`TodoForm` component changes:**
  - [ ] include a  `useState` that controls a text-based state value, `workingTodo` with an `initialValue` of an empty string
  - [ ] include a `useRef` with an initial starting value of an empty string. This is passed to a `ref` props on the input field in the component's form.
  - [ ] `handleAddTodo` should be updated
    - [ ] The code that retrieves the title from the target title value and creates a new todo should be removed
    - [ ] should call helper function and pass in `workingTodo`
    - [ ] reset the `workingTodo` to blank
    - [ ] set the focus back onto the input that is tied to the `useRef` (eg: `todoTitleInput.current.focus();`)

## Details to Watch For

- [ ] Whenever a todo is entered, the input regains focus and you can type again immediately.
- [ ] Make sure that the todo order does not change in the `todoList` whenever the todo is updated.
- [ ] `useContext` will be introduced later so students should not be using `useContext` to avoid prop drilling yet.
- [ ] Make sure that when a helper function is refactored, that the relevant function calls have been updated with the correct argument.

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
