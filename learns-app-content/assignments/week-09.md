## Weekly Assignment Instructions

### Expected App Capabilities

After completing this week's assignment, your app should:

- Use `useCallback` for URL string encoding.
- Pause API requests while the user is typing

### useCallback and encodeUrl

- Import `useCallback` into App.jsx.
- Create a variable `encodeUrl` (same name as helper function created last week) inside the App component and assign it an empty `useCallback`.
- Define an empty arrow function that takes no arguments in the `useCallback`. It should now look like:

```jsx
{/*extract from App.jsx*/}
{/*...code*/}
const encodeUrl = useCallback(()=>{},[])
{/*code continues...*/}
```

- Move the body of the `encodeUrl` utility function this empty arrow function.
- Delete the utility function from the top of the file.
- If you're using ESLint, it should be giving you the following warning about `useCallback` dependencies:

![missing dependencies shown in eslint](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/assignments/assets/week-09/missing-deps.png)

- Add these dependencies to the `useCallback` dependency array.
- Search your App.jsx for all instances of `encodeUrl` then remove the arguments wherever the function is called. All calls to `encodeUrl` should look like: `encodeUrl()` since the `useCallback` handles all the dependencies now.

### Debouncing Filter Input

Sending a network request for every character typed can use up API and network resources.

![each keypress sends a network request](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/assignments/assets/week-09/undebounced.gif)

Earlier the lesson, we covered throttling as a means to control how rapid an event fires. Another approach to controlling network requests, especially well-suited for keyboard events is debouncing. This prevents any events from being processed until they go below a certain rate. In the case of the todo list, you'll prevent keyboard events from updating the query string until they stop typing for a half a second (500ms).

<details>
<summary>More about the term "debounce"</summary>
<p>This term "debounce" originally comes from an electrical engineering solution that deals with noise that gets introduced in a circuit when a mechanical switch closes. No matter how carefully two conductive surfaces come into contact (in a button, switch, dial, etc), there is a little bounciness as they come together. That bounce opens and closes the connection rapidly causing bad information to get transmitted.</p>
<p>The solution to this is to wait for the new circuit to stop "bouncing" and settles into an open or closed state before acting on a change. This is same thing that happens when you drop a basket ball onto cement - it'll bounce repeatedly, a little bit less each time, until it stops on the ground. It just happens on a different time and size scale.</p>
</details>

The best way to balance UI performance and keeping API calls down is to prevent any fetch from happening until a certain time has passed. We want it to pause long enough that our app doesn't send out a request for every keystroke but not force the user to wait a long period of time. With deeper research you may come up with a delay length, but 500ms is a good starting point.

#### Putting Debounce into Action with useEffect

You'll combine the use of a useEffect and a setTimeout with a 500ms delay. Remember that a useEffect's return value is used to clean up after the previous useEffect as the component re-renders. To take advantage of React's re-render process you can use the cleanup function to delete the previous timeout each time there's a change. When a user pauses typing long enough, the last called setTimeout will finally get a chance to execute it's callback function. That callback function will pass the locally managed state back up to the App component which finally kicks off a fetch request with updated query params.

In TodosViewForm.jsx:

- Define a local state for the search input and set its `defaultValue` to `queryString`: `const [localQueryString, setLocalQueryString] = useState(queryString);`
- Refactor the search input and the Clear button to use the local state instead of the `queryString` and `setQueryString,` from App.
- Create a useEffect
  - Add `localQueryString` and `setQueryString` to the dependency array since you'll be working with them.
  - Call `setTimeout` and assign it to a constant, `debounce`.
    - In the body of `setTimeout`'s callback, call `setQueryString(localQueryString)`
    - Give it a delay of 500ms.
  - In `useEffect`'s return statement, add an anonymous function that calls `clearTimeout` that takes in `debounce`.

![keystrokes debounced to 500 milliseconds](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/assignments/assets/week-09/debounced.gif)

### Closing Notes

Next week we'll talk about styling and using 2 prominent React styling libraries: CSS Modules and Styled Components. That will give us a chance to brighten up a bland interface and give it character!
