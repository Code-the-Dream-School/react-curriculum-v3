## Weekly Assignment Instructions

### Expected App Capabilities

After completing this week's assignment, your app should:

- Use the API to sort todos by `title` or `createdTime`
- Use the API to search for todos based on title contents

### Instructions Part 1: Using Airtable's Query Parameters

#### Setup

- Add field, `createdTime` for default sort to your Airbase table. This will end up being the default sort field. We are not be able to use the already existing `createdTime` on the `record` object because Airtable functions only work with `fields` properties which are those defined by the user in the table. Use the following options in the field creator:
- Name the field "createdTime".
- Set it to "Date".
- Use the same timezones for everyone and set the timezone to GMT/UTC.
- Set a default value of "Current date".

![configure createTime field](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/assignments/assets/week-08/created-time-config.png)

In App.jsx

#### Examine the APIs Query Parameters

You'll be using Airtable's query parameters to set the sort and filter options. Since we want to set up a default sort order we'll look at the Airtable's `sort`

- Navigate to your table's help documents and select "List records" from the left-hand side navigation. If you don't recall the process to get to that, here's another way you can select your base to get customized documentation: [Introduction - Airtable Web API](https://airtable.com/developers/web/api/introduction)
- Scroll down to `sort` and you'll be able to read about how to work with the `sort` URL query parameter.
  - This will give you some sense about how Airtable's parameters get turned into these ugly URL strings:

```text
sort%5B0%5D%5Bfield%5D=title
sort%5B0%5D%5Bdirection%5D=desc

https://api.airtable.com/v0/airtable_table_id/Todos?sort%5B0%5D%5Bfield%5D=createdTime&sort%5B0%5D%5Bdirection%5D=desc
```

Each one of those character combinations that start with a "%" sign - as in `%5B`, `%5D`, and so on, are characters that have been re-encoded to be URL safe. To better read the string contents, you'll have to rely on a URI decoder or looking up the characters one at a time. Coincidentally, JavaScript comes with a build-in decoder: `decodeUri()` that you can run that in the browser console to see the original string:

```console
decodeURI('https://api.airtable.com/v0/airtable_table_id/Todos?sort%5B0%5D%5Bfield%5D=createdTime&sort%5B0%5D%5Bdirection%5D=desc')

"https://api.airtable.com/v0/airtable_table_id/Todos?sort[0][field]=createdTime&sort[0][direction]=desc"
```

From the decoded string, we can tell that the Airtable API takes string representations of the configuration that Airtable uses to process the request. We don't fully know how the API works in reality, but we can liken this string to setting 2 properties on an object using bracket notation, one at a time. Remember that APIs differ from each other, so other APIs may not follow this same approach. This is why developers rely so heavily on documentation to assist with their work.

2 config params also means we need to represent those values in state so a user can toggle them.

- Still in App, create new state variables for the params with associated update functions:
  - `sortField`: initial value of "createdTime"
  - `sortDirection`: initial value of "desc" as in descending

#### URL Encoding Utility Function

You already employ the `url` in four locations: the `useEffect`, `addTodo`, `updateTodo`, and `completeTodo`. All fetches need to use the same query parameters so that the todo list doesn't spontaneously re-order itself or show all items in a filtered view. You'll centralize the logic in a utility function that exists outside of the component.

- Define a function `encodeUrl` above the `App` function definition:
  - It takes an object that includes the properties: `sortField, sortDirection`.
  - Inside the function, define a template literal that combines the 2 sort query parameters:
    - ``let sortQuery = `sort[0][field]=${sortField}&sort[0][direction]=${sortDirection}`;``
    - This compares to the console output above, but `createdTime` is replaced with a `${sortField}` and `desc` is replaced with `${sortDirection}`.
  - It returns a method call to `encodeURI` that takes in a template literal that appends the query parameters to `url`. The function should resemble:

```jsx
{/*extract from App.jsx*/}
{/*...code*/}

const encodeUrl = ({ sortField, sortDirection }) => {
  let sortQuery = `sort[0][field]=${sortField}&sort[0][direction]=${sortDirection}`;
  return encodeURI(`${url}?${sortQuery}`);
};
{/*code continues...*/}
```

- Replace all references to `url` in App with `encodeUrl()`.
  - At each location, pass in an object containing the state variables `sortDirection` and `sortField`.

 If you are using ESLint, you should see a warning appear on your `useEffect:`

![missing sortDirection and sortField dependencies](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/assignments/assets/week-08/missing-deps.png)

- Update the `useEffect`'s dependency array to include the state values, `sortDirection` and `sortField`. The `useEffect will now fire` each time a user sets the `sortDirection` and `sortField`.

### Instructions Part 2: View Control Forms

You now need to provide the user with a way to update the params.

- Define a new component, `TodosViewForm` on the `src/features` directory.
- Return a form that contains a div that will contain the form controls.
  - Add a `label` containing the text "Sort by".
  - Add a `select` element that is associated with that label
  - Give the `select` element 2 `option` elements:
    - 1st: value="title" and includes the text, "Title"
    - 2nd: value="createdTime and includes the text "Time added"
  - Add another `label` containing "Direction" just after the previous label and select.
  - Add another `select` with `option` elements for:
    - "Ascending" using "asc" for the value
    - "Descending" using "desc" for the value
- Default export the component.
- In `App`, import it and place a horizontal rule between TodoList instance and the div containing the error message display.
- Below the `hr`, add an instance of `TodosViewForm`. The `hr` and `form` will show below the todos now:

![sort form on bottom of todos](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/assignments/assets/week-08/todos-hr-sort-form.png)

- Into `TodosViewForm` instance, pass in the props `sortDirection`, `setSortDirection`, `sortField`, and `setSortField`.

Back over in `TodosViewForm` you'll wire up the form so that each time the user updates the options, the state updates will cause the `useEffect` to automatically fetch from Airtable.

- Destructure `sortDirection`, `setSortDirection`, `sortField`, and `setSortField` out of the component's props.
- On `select`element for the sort by:
  - Add an `onChange`handler:
    - Its callback is an anonymous function that:
      - Takes the event object
      - Calls `setSortField` with the event target's value.
  - Add a `value` props and assign it to `sortField`.
  - Ensure that each `option`'s value matches the target field names.
- On the `select` element for the sort direction:
  - Add an `onChange` handler that uses an anonymous function that:
    - Takes the event object
    - Calls `setSortDirection` with the event target's value.
- To define a function `preventRefresh` whose only job is to prevent the page from refreshing if a user accidentally hits enter while working with this form.
- pass it to an `onSubmit` props on the `form` element.

At this point, the form should allow you to select either "Title" or "Time Added" and the API will automatically return the sorted todos and update the UI.

![Time Added can be sorted ascending or descending](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/assignments/assets/week-08/time-sort.png)

### Instructions Part 3: Search Todo Titles

In this part, you'll add a search field an button to the form that was just created. It will take in a string and then added the url params before it's encoded.

#### Update Utility Function

- In `App`, create the state value (and update function) for `queryString` with an empty string for an initial value.
- Update `encodeUrl` utility function:
  - Add `queryString` to the argument object.
  - Create an updatable variable (`let`) `searchQuery` set to an empty string.
  - Add `${searchQuery}` to the end of the template literal that is used in the return value.
  - Above the return statement, use an `if` block to assign a value to `searchQuery` is truthy.
    - If true, update `searchQuery` in the block: ``searchQuery = `&filterByFormula=SEARCH("${queryString}",+title)`;``
- Update each `encodeUrl` call in App to include `queryString` in its params object. Each of the 3 calls should now resemble: `encodeUrl({ sortDirection, sortField, queryString })`
- Update the `useEffects` dependency array by adding `queryString`.

#### Update Form

- In `App`, to the `TodosViewForm` instance, add props `queryString` and `setQueryString`,
- In `TodosViewForm`:
  - Destructure these props out.
  - Add a `div` above the previous one and include:
  - A label containing the text "Search todos:"
  - An input with props:
  - `type="text"`
  - `value={queryString}`
  - `onChange((e)=> {setQueryString(e.target.value)`
  - A button:
  - Set to the type of button.
  - Contains the text "Clear".
  - And empties `queryString` whenever it's clicked.

Your user is now able to filter the todos based on an input that they then can also reset.

![demo of todo list functionality](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/assignments/assets/week-08/search-demo.gif)

### Closing Notes

This week you added in functionality to sort the todos by title or its creation date and filter the titles to match a string input. To minimize code edits across several fetches, you extracted the logic to build the URL string into a utility function. Also, as a result of how form state is managed, all updates happen live! Each time a user types or selects an option, the `useEffect` initiates a fetch using the updated url params. This produces so pretty snappy results but can eventually become a burden on the network and API services as the application grows or a user's todo's start growing in number. Next week' we will look at a few ways limit network traffic and perform some other optimizations to the codebase to keep the interface running smoothly.
