# Assignment Rubric, Week 09

## Expected App Capabilities

After completing this week's assignment, the student's app should:

- Use `useCallback` for URL string encoding.
- Pause API requests while the user is typing
  
## Technical Details

- [ ] **useCallback and encodeUrl**
  - [ ] `useCallback` is imported and used in `App.jsx`.
  - [ ] `encodeUrl` is defined as a `useCallback` inside the `App` component.
  - [ ] The body of the previous `encodeUrl` utility function is moved into the `useCallback`.
  - [ ] The original utility function is removed from the top of the file.
  - [ ] All dependencies for `encodeUrl` are included in the `useCallback` dependency array.
  - [ ] All calls to `encodeUrl` are updated to use `encodeUrl()` with no arguments.

- [ ] **Debouncing Filter Input**
  - [ ] Local state for the search input (`localQueryString`) is defined in `TodosViewForm.jsx` and initialized with `queryString`.
  - [ ] The search input and Clear button use the local state instead of props from `App`.
  - [ ] A `useEffect` is implemented to debounce updates to the query string:
    - [ ] `useEffect` depends on `localQueryString` and `setQueryString`.
    - [ ] Inside the effect, a `setTimeout` delays the call to `setQueryString(localQueryString)` by 500ms.
    - [ ] The effect's cleanup function calls `clearTimeout` to cancel previous timeouts.
  - [ ] API requests are only sent after the user stops typing for 500ms.

- [ ] **General**
  - [ ] No unnecessary API requests are made on every keystroke.
  - [ ] All code changes are limited to the relevant files (`App.jsx`, `TodosViewForm.jsx`).
  - [ ] ESLint warnings about missing dependencies in `useCallback` and `useEffect` are resolved.

## Details to Watch For

- [ ] Debounce logic is implemented correctly and does not introduce lag or missed updates.
- [ ] No API requests are triggered when the input is empty or cleared.
- [ ] All effect dependencies are correctly specified.
- [ ] No logic is duplicated between `App.jsx` and `TodosViewForm.jsx`.
- [ ] All user interactions (typing, clearing, submitting) behave as expected.

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
