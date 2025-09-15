# Assignment Rubric, Week nn

## Expected App Capabilities

After completing this week's assignment, the student's app should:

- [ ] capability 1

## Technical Details

- [ ] `react-router@~7.2.0` is installed
- [ ] In `main.jsx`, the `App` component is wrapped with `BrowserRouter` from react-router
- [ ] All todo-related components (`TodoForm`, `TodoList`, `TodosViewForm`) are moved out of `App.jsx` to a new `TodosPage` component in `src/pages`
  - [ ] `TodosPage` receives and passes all necessary props to its child components
  - [ ] `App` renders a single `<TodosPage />` instance with correct props
- [ ] A `Header` component exists in `src/shared`
  - [ ] An instance of `Header` replaces the `h1` in `App`
  - [ ] `Header` includes an `h1` and a `nav` with two `NavLink` instances ("Home" and "About")
  - [ ] `NavLink`s use a `className` function to apply `"active"` or `"inactive"` styles based on `isActive`
  - [ ] CSS Modules are used for styling, with correct class names for active/inactive links and layout
- [ ] `App` uses `useLocation` and a `useEffect` to set the header title based on the current route
  - [ ] Title updates for `/` ("Todo List"), `/about` ("About"), and all other routes ("Not Found")
- [ ] `App` uses `Routes` and `Route` components to define navigation:
  - [ ] `/` renders `<TodosPage />`
  - [ ] `/about` renders `<About />` (component in `src/pages`)
  - [ ] All other paths (`*`) render `<NotFound />` (component in `src/pages`)
- [ ] `About` component exists in `src/pages` and displays information about the app/author
- [ ] `NotFound` component exists in `src/pages` and displays a not found message with a `Link` to return home
- [ ] Pagination is implemented in the todo list:
  - [ ] Uses `useSearchParams` from React Router to manage the `page` query parameter
  - [ ] `itemsPerPage` is set to 15
  - [ ] `currentPage` is derived from the `page` param, defaults to 1, and is always a number
  - [ ] Pagination calculations (`indexOfFirstTodo`, `totalPages`) are correct and based on filtered todos
  - [ ] Pagination controls (Previous/Next buttons and page indicator) are present and styled
  - [ ] Handlers for Previous/Next update the `page` param and prevent out-of-bounds navigation
  - [ ] Previous button is disabled on first page; Next button is disabled on last page
  - [ ] A `useEffect` checks for invalid `currentPage` values and navigates to `/` if out of bounds (only if `totalPages > 0`)
- [ ] App functionality is preserved after refactor (adding, editing, completing todos, error handling, etc.)

## Details to Watch For

- [ ] Navigation between Home, About, and Not Found pages works as expected
- [ ] The header title updates correctly based on the current route
- [ ] NavLinks visually indicate the active route using correct styles
- [ ] Pagination controls are always visible when there are todos
- [ ] Pagination controls are disabled appropriately on first/last page
- [ ] The todo list displays only the correct todos for the current page
- [ ] Direct navigation to an invalid or out-of-bounds page param redirects to `/` (when todos are loaded)
- [ ] The About page contains meaningful information about the app/author
- [ ] The NotFound page displays a clear message and a working link to return home
- [ ] App functionality (adding, editing, completing todos, error handling) is preserved after refactor
- [ ] No regression in UI responsiveness or user experience

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
