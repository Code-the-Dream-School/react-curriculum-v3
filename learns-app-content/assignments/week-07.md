## Weekly Assignment Instructions

### Expected App Capabilities

After completing this week's assignment, your app should:

- Save todos to an Airtable base
- Load saved todos from Airtable on loading
- Display pending messages during network requests
- Display error messages on network or authentication error

### Instructions Part 1: Set up Airtable Base

An Airtable base is a collection of tables that stores data, similar to a database, but with a user-friendly interface that resembles a spreadsheet. Each base can contain multiple tables, each with its own set of rows and columns to organize and store different types of information.

We will use an Airtable table as an API to save data for the todo list.

#### Before You Begin

You'll need a place to store your personal access token, base id, and table name. These are details that we do not want to publish with our project so we'll use an environment file, .env.local. It has already been listed in the project's .gitignore so we don't have to worry about it being saved into version control.

After creating this week's working branch, create a environment file, `.env.local`, in the base directory of the project then paste in the following text:

```text
VITE_PAT=super_secret
VITE_BASE_ID=xxxxxxxxxxxxxxxxx
VITE_TABLE_NAME=TableName
```

As you go through the setup and documentation, you'll save those values for the project's use.

#### Create Base

- Your first steps is to [sign up for an Airtable account](https://airtable.com/signup). Once completed, follow the screenshots to set up your todo's base.
- Choose the Create button at the bottom left and choose to start from scratch.

![choose start from scratch](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/assignments/assets/week-07/start-scratch.png)

- Rename the table's default name of "Table 1" to "Todos" by clicking on the down arrow on Table 1's tab.
- Save the table name to your environment file under the variable `VITE_TABLE_NAME`
- Update the first field name so that it reads "title" instead of "Title".
- Delete the other fields.

![hit down arrow and select edit field](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/assignments/assets/week-07/update-fields.png)

- Create a new checkbox field named "isCompleted". This type of field acts a boolean.

![add isCompleted field](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/assignments/assets/week-07/add-is-complete.png)

#### Generate Connection Credentials

- Click on profile picture to open menu.
- Select "Builder hub"

![navigate to builder hub](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/assignments/assets/week-07/builder-hub.png)

- Select "Create new token".

![create a new personal access token](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/assignments/assets/week-07/new-pat.png)

- Give the token a name that you'll remember.
- Add the scopes `data.records:read` and `data.records.write`
- Under the Access heading. Hit "Add a base" and choose your new one.
- Hit "Save changes" Copy the token value that shows on the next screen
- Save it to `VITE_PAT` in your environment file.

![configure access token](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/assignments/assets/week-07/config-pat.png)

- We next have to find the base's id - one approach is to use the API documentation accessed from the base's help menu.

![navigate to customized api documentation](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/assignments/assets/week-07/find-api-docs.png)

- After the introduction paragraphs, the documentation includes your personal base's id. Save that to `VITE_BASE_ID` in the environmental file.

![documentation is customized to match base config](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/assignments/assets/week-07/custom-base-docs.png)

Take some time to look over the documentation for Airtable. As you read it over, keep in mind that we will **not** be using Airtable's JavaScript API client. It does not provide anything useful for us in React so it's better to look at the `curl` examples. That'll let you see how the urls and payloads look. Some of the things you can do with the API are pretty cool! Go take a look!

> [!note]
> The backslash `\` in the `curl` code examples lets us hit enter to continue adding arguments to the `curl` command.

```terminal

## using backslash

curl "https://api.airtable.com/v0/app8c6VR6IQ26pgKP/Todos?maxRecords=3&view=Grid%20view" \
  -H "Authorization: Bearer super_secret_api_key"

## without backslash and new line, it's harder to read the command

curl "https://api.airtable.com/v0/app8c6VR6IQ26pgKP/Todos?maxRecords=3&view=Grid%20view" -H "Authorization: Bearer super_secret_api_key"
```

### Instructions Part 2 : Load Todos from Airtable

You will be updating your app to load the todos from Airtable and adopting a pessimistic update approach. While the todos are loading, you'll display a wait message to the user.

In App.jsx:

#### Establish Transition State

- Create an `isLoading` state variable with with a `defaultValue` of `false`. Destructure out its associated update function, `setIsLoading`, too.
- Create another state variable but name it `errorMessage` and default to an empty string, `""`. Include its update function too.

#### Fetch in App

You will use the `useEffect` hook to define and send off the initial fetch request that loads the todos from Airtable. You'll also work with `isLoading` to conditionally show a loading message to the user. If we ever have to make a user wait, it's important to indicate that our app is in the middle of a slow process. This lets the user know that the app is processing a request and they should wait for it to complete.

While still in App.jsx:

- Create constants, `url` and `token`, which will be used in the fetch requests.
  - ``const url = `https://api.airtable.com/v0/${import.meta.env.VITE_BASE_ID}/${import.meta.env.VITE_TABLE_NAME}`;``
  - ``const token = `Bearer ${import.meta.env.VITE_PAT}`;``
- create the `useEffect` that was discussed above:
  - Give it an empty dependency array.
  - In the body of the function in `useEffect`'s first argument do the following:
    - Define an async anonymous arrow function that takes no arguments and save it to `fetchTodos`
    - Make sure to call `fetchTodos()` before closing out the `useEffect`'s first argument.

So far, it should look like:

```jsx
{/*extract from App.jsx*/}
{/*...code*/}
useEffect(() => {
    const fetchTodos = async () => {};
    fetchTodos();
}, [])
{/*code continues...*/}
```

- continuing in `fetchTodos`
  - Use `setIsLoading` to update `isLoading` to `true`
  - Create an `options` object for the fetch request. Include:
    - A `method` property set to "GET",
    - And a `headers` property set to an object containing the following key/value pair: `"Authorization": token`.
  - Set up `try/catch/finally` blocks to handle the fetch:
    - in `try`
      - Save an awaited `fetch` to the const `resp` and pass in the `url` and `options` : `const resp = await fetch(url, options);`
      - If `resp.ok` evaluates to false in an `if` statement, throw a new `Error` that takes in `resp.message`.

After the `if` block that throws, you'll place your remaining logic to process the API's response.

- Await the value that `response.json()` returns. We will be working with the `records` array in the response. An example response would be:

```JSON
{
    "records": [
        {
            "id": "recEAx4TQF6JjzZLV",
            "createdTime": "2025-02-19T22:04:12.000Z",
            "fields": {
                "title": "test todo"
            }
        },
        {
            "id": "recx22h6JwjsMhCku",
            "createdTime": "2025-02-19T22:04:15.000Z",
            "fields": {
                "isCompleted": true,
                "title": "make dinner"
            }
        },
        {
            "id": "recFwX5Nyp1TpkUXI",
            "createdTime": "2025-02-19T22:04:17.000Z",
            "fields": {
                "title": "code out assignment examples"
            }
        }
    ]
}
```

There are several important details to note when you compare this response against the `todoList` state value:

- The properties, `isCompleted` and `title` are stored in a `fields` object.
- The `id` is at the top level of each todo object.
- Any fields that equal false or contain an empty string will not be returned with the todos.
- The `records` value will always be an array.

Because the "shape" of each todo is different than how they are represented in the app's `todoList` array, you will have to map the contents of `record` out of the response body. The same will be true for the payload of any PUT and PATCH requests.

- in a `map` method called on `records`, define an anonymous function:
  - It takes a `record` param.
  - It contains a `const todo = {}`.
  - Inside the object, assign the `record` properties to the appropriate todo properties.
  - If `isCompleted` is not truthy, you will want to explicitly set the property equal to `false`.

Use the below `map` examples as reference if needed:

```js
//example map from an Airtable response.records

const response = await response.json();
const fetchedExamples = response.records.map((record) => {
    const example = {
        id: record.id,
        //destructuring is a compact way to move all fields over
        //won't need to change this a lot as the app evolves
        ...record.fields,
        };
    if(!example.booleanProperty){
        //Airtable doesn't return properties whose values are false or empty strings
        //explicity setting property to false so field exists and prevents logic bugs
        example.booleanProperty = false;
    }
    return example;
})
setExamples([...fetchedExamples])
```

-or a more compact version-

```js
const {records} = await response.json()
setExamples(records.map(record)=> {
    const example = {
    id: record.id,
    ...record.fields,
    };
    if(!example.booleanProperty){
        example.booleanProperty = false;
    }
    return example;
})

```

After this, it's time to move on to the `catch` and `finally` blocks.

- The `catch` block:
  - Takes in an error parameter.
  - Use `setErrorMessage` with `error.message` to set an error message that we will display to the user shortly.
- The `finally` bock:
  - Set the `isLoading` state value back to false.

#### Display Loading Indicator in TodoList

- While still in App.jsx, pass `isLoading` into props for the `TodoList` instance.
- In TodoList:
  - Destructure `isLoading` out of its props.
  - Add a statement that evaluates `isLoading` in the JSX that conditionally renders the existing paragraph OR a new paragraph for the loading message.
  - The new paragraph should have a loading message to display the user. Eg: `<p>Todo list loading...</p>`

At this point, the todo list should display a message while the API is loading the todos. Once the fetch resolves, it re-renders with the todo list (Or the original paragraph if 0 todos are loaded).

![todo list displays loading message](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/assignments/assets/week-07/loading-message.gif)

#### Display Error

In App:

- Add conditional block after the `TodoList` instance. Use the block to evaluate `errorMessage`.
  - If not empty, display a div containing:
    - A horizontal rule `<hr/>`.
    - A paragraph that renders `errorMessage`.
    - A dismiss button that empties `errorMessage`.

When an error happens with our initial fetch, a message is displayed to the user. We've intentionally made this setup as simple as possible so that we can re-use this error message for other operation errors.

### Instructions Part 3: Update Add New Todo Functionality

In part 3, you'll use an pessimistic state update approach for adding todos. You'll use transient state to change the text content of the button while the request is processing. We don't want pause the user but it's useful for them to know that the application is processing the last action before their todo is rendered . When the API has processed the response, the todo in the payload response gets added into the todo list state.

In App:

- Create state value`isSaving` with a `defaultValue` of false. You will need its update function too.
- In the add todo button
  - Replace the button text with a ternary block that evaluates `isSaving`.
    - If true, display "Saving..."
    - If `false`, display "Add Todo" or the text value you were already using.

It should now look like:

```jsx
<button disabled={workingTodoTitle.trim() === ''}>
    {isSaving ? 'Saving...' : 'Add Todo'}
</button>
```

- Update `addTodo(newTodo)`:
  - Convert it to an async function.
  - Inside the function body...
    - Create a `payload` object containing a `records` array containing a single todo object.
    - Create an `options` object for the fetch request:
      - Use the "POST" method.
      - Use the same header options from the last request.
      - Stringify the `payload` object for `options.body`.

At this point, `addTodo` should look like:

```jsx
{/*extract from App.jsx*/}
{/*...code*/}

const addTodo = async (newTodo) => {
    const payload = {
      records: [
        {
          fields: {
            title: newTodo.title,
            isCompleted: newTodo.isCompleted,
          },
        },
      ],
    };
    const options = {
      method: 'POST',
      headers: {
        Authorization: token,
        'Content-Type': 'application/json',
      },
      body: JSON.stringify(payload),
    };
{/*code continues...*/}
```

- Continuing from inside `addTodo`
  - `try` block logic:
    - Set `isSaving` true.
    - Send a fetch request and assign it's awaited return value to `resp`.
      - (hint: `const resp = await fetch(url, options);`)
      - Throw an Error if `!resp.ok`
    - Destructure the `records` from the response by using `const { records } = await resp.json();`
    - Create a `savedTodo` object that:
      - assigns the `id` to the first array entry's id property.
      - destructures the remaining key/values pairs out of the only `records` object.
    - If the `isCompleted` property in the `records` object is not true, explicitly set that on property on `savedTodo` to `false`
      - This looks like `if (!records[0].fields.isCompleted) {savedTodo.isCompleted = false;}`
      - Recall that Airtable does not return false or empty fields in the `records`.
    - Update state using `setTodoList([...todoList, savedTodo]);`
  - `catch` block logic:
    - Log the error to console.
    - Update `setErrorMessage` state value with `error.message`. This will cause errors to show up for users.
  - `finally` block:
    - Set `isSaving` to false.

The updates you've made should flash a brief message on the button while the response processes:

![saving message shows briefly on submit button](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/assignments/assets/week-07/saving-message.gif)

### Instructions Part 4: Update Functionality of Update and Complete Todo

For the final two refactors, you will be using an optimistic approach to UI management. This introduces the need to undo state changes and communicate unexpected API failures to the user. You'll set the updated value into state at the beginning of the `updateTodo` function but not before saving the original version temporarily. If there's a network disruption or some other issue that causes the fetch to fail, you'll use this original version to revert the todoList state back to its state before the error.

#### Update Todos Functionality

In App:

- update `updateTodo:
  - Convert it to an async function.
  - Save the original todo to `originalTodo`:
    - By finding it's associated object in the `todoList` array by each object's id.
    - It should look like: `const originalTodo = todoList.find((todo) => todo.id === editedTodo.id)`
  - Create `payload` object for the fetch request using `editedTodo`
    - The API expects `payload` to be a JSON object containing a `records` array of table records for the Todo table. For reference, here's the JavaScript object you define in preparation to do this:

```JSON
const payload = {
    records: [
        {
            id: editedTodo.id,
            fields: {
                title: editedTodo.title,
                isCompleted: editedTodo.isCompleted,
            },
        },
    ],
};
```

- Continued...
  - Create an `options` object for the fetch request.
    - Use the PATCH method.
    - Use the same Authentication header and include `'Content-Type': 'application/json'` in the `headers` object.
    - For the `body` property, convert the `payload` object to a JSON string using `JSON.stringify(payload)`
  - `try` block:
    - Send a fetch request using `url` as the endpoint and the `options` object just created.
      - Throw an error if `!resp.ok`.
  - `catch` block:
    - Log the error to console.
    - Update `errorMessage` using the ``setErrorMessage(`${error.message}. Reverting todo...`);``
    - Create a `revertedTodos` using the `originalTodo` to reset that todo's value to a previous state.
    - Update state with `setTodoList([...revertedTodos]);`
  - `finally` block:
    - Set the state value, `isSaving`, to false.

In TodoListItem:

We need to cover an edge case where an outdated value is displayed when a user saves a todo and clicks it again immediately.

- add a useEffect that resets the `workingTitle` to the `todo` prop's `.title` value
  - add `todo` to the dependency array.
  - call `setWorkingTitle` and pass in the value of `todo.title`

At this point, the todo app should:

1. Load the todos from Airtable.
2. Create new todos with the todo form and persist them in Airtable.
3. Allow users to edit todos which will then update in Airtable.
4. Display errors messages to the user if there are any fetch issues.

![error displayed to user at bottom of screen](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/assignments/assets/week-07/error-message.png)

#### Update Functionality to Complete Todos

You'll be left to update `completeTodo` with the necessary logic to communicate the completed todo to the Airtable API and keep the UI synced. You can use `updateTodo` as a reference as you work through this final portion. So that you have a checklist of details to think about: the final code should:

- [ ] optimistically update the UI:
  - [ ] The todo completes immediately in the UI immediately and allows user to continue to work with the todo list.
  - [ ] The fetch request happens in the background unless there's a request error.
  - [ ] The UI for the todo list reverts to its original state if the fetch response does not come back okay.
  - [ ] Display an error message to the user, using the `errorMessage` state if fetch encounters any problems.
- [ ] Fetch should use the same headers and method, and take in a payload very similar to the fetch found in `updateTodo`
- [ ] The same object transformation has to happen between the todo object as represented in state and the todo represented in the `records` property on the payload.

### Stretch Goals: Refactor for Reusable Code

You will probably have noticed that you are starting to introduce some duplicate code. While that is okay during prototyping and early development, before submitting any final code for a feature (in our case, this assignment), take the time to refactor your code to make it **[DRY](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself)**. This will keep the codebase clean and easy to manage. Remember that you should commit your code now so that you have a good save point to return to if you need to start the refactor over again.

- Look for any items that are defined multiple times and assign them to a variable.
- Compare functions or procedures to find common logic that could be shared in a helper or utility function.
- Brainstorm some more user-friendly error messages.
- If you visual learner and have to add styling to help visualize the interface better:
  - Stick to class-based styles that you define in App.css.
  - Don't restructure anything in the JSX. We want to maintain a semantically sound html structure so that when we cover styling in week 10, several style choices applied during the assignment will work out properly.
  - Have fun but don't let styling get away of completing the weekly lesson. After all, a baker doesn't ice a cake until after it's baked and cooled.

### Closing Notes

 Next week we cover sorting and filtering. The discussion will take us through implementing sorting and searching on both local and API data stores. We'll then implement an API-based sorting and filtering strategy.
