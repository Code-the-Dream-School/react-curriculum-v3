## Weekly Assignment Instructions

### Expected App Capabilities

After completing this week's assignment, your app should:

- Use React-Router
- Have a personalized about page
- Paginate the todo list when it's long

### Instructions Part 1: Implement React-Router

- Install React-Router into the project using the terminal: `npm install react-router`
- In main.jsx, wrap the App instance with BrowserRouter imported from react-router.

#### Refactor App

You will be moving all of the todo-related components to a TodosPage component. You will also replace the heading element in app with a Header component that includes a heading and navigation links.

##### TodosPage

- Create a new component, TodosPage, in `src/pages`
- Refactor all Todo-related component instances in App.jsx to TodosPage.
  - Move instances of `TodoForm`, `TodoList`, and `TodosViewForm` from App to TodosPage.
  - Destructure all the props needed form TodoForm, TodoList, and TodosViewForm from the TodosPage component definition.
    - `todoState` properties can be used for all `todoState` properties when passing props into `<TodosPage />`
  - Pass on appropriate props to each component instance.
- In App, add an instance of `TodoPage` to replace the `TodoForm`, `TodoList`, and `TodosViewForm` instances.
- Add the appropriate props to `TodosPage` instance.

##### Header and Nav

- Create a Header component in `src/shared` to replace the `h1` element in App. Include:
  - An `h1` that takes in a `title` props and add props to component definition.
  - A `<nav></nav>` element that contains:
    - 2 `NavLink` component instances from the React-Router library.
      - "Home": `to:{"/"}`
      - "About": `to:{"/about"}`
    - Give each `NavLink` instance a `className` props based on the truthiness of the `isActive` property on `NavLink`:
      - In the `className` props, construct an anonymous function:
        - Destructure `isActive` out of the object this function receives from `NavLink`
        - It should look something like; `className={({ isActive }) =>{ code continues...`
        - if `isActive` is true, return `"active"`
        - if false, return `"inactive"`
  - Style the component using CSS Modules:
    - Update `"active"` to `styles.active`: Add a class rule that sets a subdued color compared to your surrounding text. (eg: if using black text on page, set to a grayish color)
    - Update `"inactive"` to `styles.inactive`: Use your primary text color.
    - Center the `h1`.
    - Update `nav` styles so that the NavLinks sit side-by-side in the `nav` element.
    - Position the `nav` in the upper-right corner of the screen. It should not affect the `h1`'s position.
- Move to App and replace the `h1` element with an instance of the Header component.
- Import `useLocation` from React-Router and assign its return value to `const location`.
- Create a `useEffect` that sets the title based on `location.pathname`:
  - `"/"`: "Todo List"
  - `"/about"`: "About"
  - `else`: "Not Found"
- Add `location` to the `useEffect`'s dependency array.
- Confirm app works.

##### Route and Routes Components

- In App, add a `Routes` instance containing 3 `Route` instances.The props for each `Route` is provided below:
  - `path="/" element={<TodosPage>}` (copy/paste in the existing TodosPage with all the props already added)
  - `path="/about": element = {<h1>About</h1>}`
  - `path="/\*": element = {<h1>Not Found</h1>`}
- Confirm your app works.

### Instructions Part 2: About Page and Not Found Page

#### About Page

- Create a new component, `About`, in `src/pages`
  - The component should return 1 or more paragraphs including information about the app and optionally the author.
  - Feel free to include whatever other information you want. (author info, credits, etc.)
- Import into `App` and replace the h1 element in the `"/about"` `Route`.
- Create a new component, `NotFound`, in `src/pages`
  - The component should display a "page not found" message and a `Link` to navigate back home.
- Import into `App` and replace the h1 element in the wildcard route with an instance of `NotFound`.

At this point, the updated app should allow the user to navigate between the todo list and about page. It should also display the "Not Found" page for any pathname that doesn't match those defined in the first 2 Route components.
![User navigating between home and about pages](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/assignments/assets/week-12/nav-home-to-about.gif)

### Instructions Part 3: Paginate Todo List

You will be setting up pagination for your todo list. Unlike the pagination in the lesson, this exercise will tie the todo list pagination into React-Router's functionality using URL parameters.

#### Pagination Calculations

- Import `useSearchParams` hook from React-Router
- Call hook and assign the return values: `const [searchParams, setSearchParams] = useSearchParams();`
- Create a constant `itemsPerPage` and set it to 15.
- Create a constant `currentPage` and assign it the numerical value the 'page' param in the URL
  - Use `searchParams.get('page')` to retrieve that params value from the URL. You will have to use `parseInt` on that value since params are always returned as strings.
  - If `searchParams` does not retrieve anything because the params doesn't exist in the URL, default to the value `1`
  - It should look like: `const currentPage = parseInt(searchParams.get('page') || '1', 10);`
- Create a constant `indexOfFirstTodo` that calculates the value based off the page's index (`currentPage` - 1) multiplied by the items on each page.
- Calculate `totalPages` by dividing the `filteredTodoList` length by the items per page. Round up using `Math.ceil` since the last page may contain less than 15 todos.

#### Navigation UI and Handlers

- Create a div with a class "paginationControls" below the unordered list to contain the pagination controls.
  - Add a button containing the text "Previous"
  - Add a button containing the text "Next"
  - Between the buttons, add a span that contains the text: `Page {currentPage} of {totalPages}`
  - Add any styling needed to the div so that the buttons and span line up horizontally and line up neatly with the rest of the interface.

Example of how the buttons and span may look:

![Navigation controls with current and total page numbers between buttons](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/assignments/assets/week-12/nav-controls.png)

- Create a handler function `handlePreviousPage` that uses `setSearchParams` to set `page` to `currentPage - 1` while preventing the value from decreasing below 1.
- Create a handler function `handleNextPage` that increments the `page` value with a max value of `totalPages`.
- Add click handlers to the Previous and Next buttons using these handler functions.

#### Protect against Invalid URL Parameters

- Disable the Previous button if the `currentPage` is the first page using the `disabled` props: `disabled={currentPage === 1}`
- Disable the Next button when the last page is reached: `disabled={currentPage === totalPages}`
- Create a `useEffect` that examines the `currentPage`:
  - If it is not a valid number (eg: "moose"), less than 1, or greater than `totalPages`, programmatically navigate to `"/"`
  - Add `currentPage`, `totalPages`, and `navigate` to the dependency array.

#### Allow the User to Navigate Directly

One problem that arises is that the todos are initially empty while they are being fetched from Airtable. As the `useEffect` is currently written, it automatically navigates back to "/" if a user refreshes the page or navigates directly to a url that includes a page param. At the time if this writing, `isLoading` is insufficient to prevent React-Router from navigating the user away. The render cycle sets that value to false before calculating any of the data used for pagination.

- Wrap the contents of the `useEffect` in an `if` statement that evaluates `totalPages`.
  - If it is greater than 0, permit the `navigate("/")` to fire.

![demo of todo pagination and reloading page](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/assignments/assets/week-12/router-params-demo.gif)

### Closing Notes

Next week, we will be looking at the larger React ecosystem and deploying an app.
