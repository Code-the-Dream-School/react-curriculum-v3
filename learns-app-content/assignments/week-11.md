## Weekly Assignment Instructions

### Expected App Capabilities

After completing this week's assignment, your app should:

- Retain the same UI behavior and functionality after the refactor.
- Employ the reducer pattern to reduce state management complexity

### Instructions Part 1: Plan Initial State and Actions

This week you will refactor the todo and loading states to use the reducer pattern. Although the assignment will give you all of the action types, there is still a lot of work that goes into transferring over the correct logic to the reducer. Use whatever techniques you are comfortable with to take working notes so you can keep track of what progress you made.

All of the migration work will be from the App component since that is where all the affected state is managed. You will be combining state for:

- `todoList`
- `isLoading`
- `isSaving`
- `errorMessage`

#### State

- Create a new directory, `src/reducers`
- In that directory, create a new file named `todos.reducer.js`.
- Declare an `initialState` constant and assign it an object.
  - Add a property in this new object for each of the states you are going to combine.
  - Assign them the values that were passed into `useState` for their initial value. (eg: `todoList: [],`).
- Export `initialState` using a named export at the bottom of the file.

#### Actions

- Examine logic around related state update functions in the App component.
  - Take note of:
    - All locations where state values are used.
    - All state update function references or invocations.
    - Any logic whose sole purpose is to prepare a value passed to a state update function.
    - Any co-located state update functions for intermediate state
  - Remember optimistic and pessimistic strategies differ.
- Use these notes to identify code passages that can be grouped into actions.
- Compare the list of actions you came up with to the `actions` object below:

```js
const actions = {
    //actions in useEffect that loads todos
    fetchTodos: 'fetchTodos',
    loadTodos: 'loadTodos',
    //found in useEffect and addTodo to handle failed requests
    setLoadError: 'setLoadError',
    //actions found in addTodo
    startRequest: 'startRequest',
    addTodo: 'addTodo',
    endRequest: 'endRequest',
    //found in helper functions 
    updateTodo: 'updateTodo',
    completeTodo: 'completeTodo',
    //reverts todos when requests fail
    revertTodo: 'revertTodo',
    //action on Dismiss Error button
    clearError: 'clearError',
};
```

- Add this `actions` object to the top of the `reducers.js` file.
- Add `actions` to the file's named exports.

### Instructions Part 2: Create Reducer

- Below `actions`, define a `reducer` function that takes in a `state` and an `action`.
- Set the `state` parameter to equal `initialState`
- In the body of the function, define switch/case statement that evaluates `action.type`
- Add a `case` clause for each action in the actions object. Fore each clause:
  - Return an object that contains the destructured `state`

Each case should just return state unchanged. There is also no need to add a `break` statement since the clause returns a new state. Your reducer should look similar to the code below:

```js
// extract from todos.reducer.js
//...code

function reducer(state = initialState, action) {
  switch (action.type) {
    case actions.fetchTodos:
      return {
        ...state,
      };
    case actions.loadTodos:
      return {
        ...state,
      };****
//code continues...
```

### Instructions Part 3: Duplicate State Logic to Reducer

> [!warning]
> Commit progress into working branch! This will give you a good backup point in case you need to start over.

#### useEffect (Pessimistic UI)

Related actions: `fetchTodos`, `loadTodos`, `setLoadError`.

- In the reducer, update the `case` clause `actions.fetchTodos`
  - Add the `isLoading` property to returned state object and set it to `true`.
- Update `actions.loadTodos` clause:
  - Move the logic that maps each record from `records` into a todo.
  - Update the `...records.map` to use `...action.records.map`
  - Update the returned state object:
    - Add a `todoList` property and assign the resulting mapped array
    - Add `isLoading` and set it to false.
- Update the `actions.setLoadError` clause's state object:
  - Add `errorMessage` using `action.error.message`
  - Add `isLoading` set to `false`

#### addTodo (Pessimistic UI)

Related actions: `startRequest`, `addTodo`, `endRequest`, `setLoadError`

- Update the `actions.startRequest` clause's state object so that `isSaving` is `true`.
- Update `actions.addTodo` clause:
  - Copy over the logic that creates `savedTodo` and adds the `isCompleted` property when Airtable omits it from the record.
  - Update the returned state object:
    - Add a `todoList` property containing a new array destructuring `state.todoList` and `savedTodo`.
    - Add `isSaving` set to `false`
- Update the `actions.endRequest` clause's state to set `isLoading` and `isSaving` to `false`.
- Update the `actions.setLoadError` clause's state:
  - `errorMessage: action.error.message`
  - `isLoading: false`

#### updateTodo, completeTodo (Optimistic UI)

Related actions: `updateTodo`, `completeTodo`, `revertTodo`

- Copy the logic that updates the todo to the `actions.updateTodo` clause.
  - Use `action.editedTodo` wherever you used `updateTodo`'s argument.
  - Create a `const updatedState = {}` and destructure `state` and `updatedTodos` into it.
  - If there is an `error` property on the `action` object, add an `errorMessage` property onto `updatedTodos` set to `action.error.message`.
  - At the end of the clause, return the updated state.
- Copy the logic that completes a todo into `completeTodo` clause.
  - Replace `id` with `action.id` wherever the original `completeTodo` uses its argument.
  - Return the state with the `updatedTodos` destructured into the `todoList` property.
- The logic for `revertTodo` should be the same as `updateTodo.
  - If yes: make sure that the `revertTodo` case is written directly above `updateTodo` and remove the return statement. This will cause the action to fall through to the `updateTodo` case.
  - If they differ, copy the logic over and apply the same update patterns that we have gone through several times.

#### Dismiss Error Button

- Update the `actions.clearError` clause to set the `errorMessage` to an empty string.

### Instructions Part 4: Implement useReducer

- Import and alias reducer code.

```js
// extract from App.jsx
//...code
import {
  reducer as todosReducer,
  actions as todoActions,
  initialState as initialTodosState,
} from './reducers/todos.reducer';
//code continues...
```

- Call `useReducer` using `todosReducer` and `initialTodoState`.
  - Assign the state variable to `todoState` and the dispatch function to `dispatch`

### Instructions Part 5: Replace State and Logic with Action Dispatches

For each action that was defined, you need to replace the state update logic in App with a dispatch. If you ended up with differing actions, make sure that you take that into account as you are making the shift over to dispatched actions.

#### useEffect

Related actions: `fetchTodos`, `loadTodos`, `setLoadError`

- Replace `setIsLoading` with a dispatch: `dispatch({ type: todoActions.fetchTodos });`
- Replace `setTodoList` and the logic that maps records to todos with a dispatch:
  - Set the type to `loadTodos`.
  - Add the `records` to the action object.
- In the `catch` block:
  - Remove the `console.error` if you still have it.
  - Replace `setErrorMessage` with a dispatch for `setLoadError`. Include the `error` on the action object.

#### addTodo

Related actions: `startRequest`, `addTodo`, `endRequest`, `setLoadError`

- Replace `setIsSaving` with a dispatch that sets `todoState.isSaving` to true.
- Replace `savedTodo` and `setTodoList` with a dispatch `addTodo` and include `records` in the action object.
- Replace `setErrorMessage` and include the `error` on the dispatched action object.
- Replace `setIsSaving` with a dispatch that sets `todoState.isSaving` to false.

#### updateTodo, completeTodo

Related actions: `updateTodo`, `completeTodo`, `revertTodo`

- Replace `updatedTodos` and `setTodoList` with a dispatch to update the todo. Include the `editedTodo` on the action object.
- Remove the unneeded logic that updates the todo list with the updated todo. Hint: since updating a todo is an optimistic process, you do not have to do anything with the API response unless it includes an error.
- In the `catch block` dispatch a `revertTodo` that includes the `originalTodo`.
- Swap out state logic in `completeTodo` on your own.

### Instructions Part 6: Update References to State

- Replace all references to the old state to use the new `todoListState`. Eg: `<TodoForm onAddTodo={addTodo} isSaving={todoState.isSaving} />`
- Replace the state update function on the button's click handler to dispatch a `clearError` action instead of calling `setErrorMessage('').

### Instructions Part 7: Test Functionality

- Make sure your project starts without errors.
- Test app functionality:
  - [ ] Can you add a new todo? Does the new todo appear after it's saved to the API?
  - [ ] Can you edit an existing todo? Does it retain its edit after you refresh the page?
  - [ ] Can you complete a todo? Does is disappear from the page? Does it remain completed after you refresh the page?
  - [ ] Can you sort the todos? Filter them?
  - [ ] If you introduce a misspelling to the API's URL, do you get the expected errors from each user action?
  - [ ] If you misspell your personal access token (restart the app every time you change your environment file) do you get the expected errors?

### Stretch Goal(optional): Refactor Remaining App-maintained State

The remaining state in App deals with the URL's query params. For additional practice, migrate the remaining state to the reducer that you've already created.

```jsx
//remaining state in App:
const [sortDirection, setSortDirection] = useState('desc');
const [sortField, setSortField] = useState('createdTime');
const [queryString, setQueryString] = useState('');
```

- Plan initial states and actions.
- Update `intialState`
- Add identified actions to actions object and add cases to reducer.
- Duplicate over any logic that prepares state updates.
- Replace state updates with action dispatches.
- Removed unused code.

### Closing Notes

Next week we will be looking at React-Router and the advantages of being able to emulate browser navigation inside of an SPA.
