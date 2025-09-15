# Assignment Rubric, Week

## Expected App Capabilities

After completing this week's assignment, the student's app should:

- [ ] capability 1

## Technical Details

- [ ] `src/reducers` directory exists and contains `todos.reducer.js`
- [ ] `todos.reducer.js` exports:
  - [ ] a named `initialState` object combining `todoList`, `isLoading`, `isSaving`, and `errorMessage` with correct initial values (for the student's project)
  - [ ] (optional but encouraged) a named `actions` object with all required action types (`fetchTodos`, `loadTodos`, `setLoadError`, `startRequest`, `addTodo`, `endRequest`, `updateTodo`, `completeTodo`, `revertTodo`, `clearError`)
  - [ ] a `reducer` function that handles all action types with a switch statement
- [ ] All state logic for `todoList`, `isLoading`, `isSaving`, and `errorMessage` is managed by the reducer, not with `useState`
- [ ] App uses `useReducer` with the reducer and initial state from `todos.reducer.js`
  - [ ] State variable is named `todoState` (or something similar) and dispatch function is named `dispatch`
- [ ] All state updates in App are replaced with `dispatch` calls using the correct action types and payloads
  - [ ] Includes useEffect for loading todos, addTodo, updateTodo, completeTodo, and error handling
- [ ] All references to old state variables are updated to use the reducer-managed state (e.g., `todoState.todoList`, `todoState.isLoading`, etc.)
- [ ] Dismiss Error button dispatches `clearError` action
- [ ] App functionality is preserved after refactor (adding, editing, completing todos, error handling, etc.)
- [ ] (Stretch) If attempted: remaining App state for sort and query params is migrated to the reducer

## Details to Watch For

- [ ] The reducer does not mutate state directly (always returns new objects/arrays)
- [ ] All reducer actions are pure and do not cause side effects (e.g., no API calls or direct DOM manipulation inside the reducer)
- [ ] The UI remains responsive and reflects loading, saving, and error states accurately
- [ ] No regression in core app functionality (adding, editing, completing, and reverting todos)
- [ ] All todos retain their correct properties (e.g., isCompleted, id, title) after any action
- [ ] The app does not use useState for any reducer-managed state
- [ ] (Stretch) If implemented: sort and query param state is managed by the reducer and updates correctly

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
