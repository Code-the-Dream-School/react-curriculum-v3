## Weekly Assignment Instructions

### Expected App Capabilities

After completing this week's assignment, your app should:

- render each static todo in a TodoListItem component
- contain a paragraph containing a value controlled by state

### Instructions Part 1: TodoListItem Components

- Create a new file named `TodoListItem.jsx` in the `src` directory.
- In that file, create a component by the same name that takes in a `todo` props.
  - Remember that you can destructure this value from the props directly in the component function's arguments. eg: `function ExampleComponent({name}){...`
- Add a list item element to the return statement that includes the `todo`'s title between the element tags.
- Add a default export statement for that component at the bottom of its file.
- Returning to `TodoList.jsx`, import `TodoListItem`.
- Replace the list item tags in the `map` with an instance of the `TodoListItem` using a self-closing tag.
- Pass the `todo` in as props to the `TodoListItem` instance.
- Remember that the map statement returns an array. React requires that we place a `key` props in Components rendered from an array. Use the `todo.id` as the value of `key`.

At this point, the todo list in the browser should appear identical. However, if you go to the [React developer tools'](https://react.dev/learn/react-developer-tools) component tab in your browser's dev tools, you'll see that the `TodoList` has three sub-component instances of `TodoListItem`. Each contains a key coinciding to the `todo.id`.

![screen capture of component tree in React dev tools](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/assignments/assets/week-03/component-tree.png)

### Instructions Part 2: State for New Todos

- Go to the `App` component and import the `useState` hook.
- Inside the function definition, before the return statement, create a new state value that will hold a new todo.
  - Use array destructuring to save the state value as `newTodo` and the state update function as `setNewTodo`.
  - Use a short string as the state's `initialValue`.

```js
//example useState
const [exampleStateValue, setExampleStateValue] = useState(42)
```

- Between the `TodoForm` and the `TodoList`, add a paragraph and place the state value between its opening and closing tags.

The browser should render that `initialValue` between the form and the todo list.

![screen capture with paragraph tag containing the todo state value](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/assignments/assets/week-03/todo-list-with-paragraph-tag.png)

### Closing Notes

 Next week we will work more with the state as we cover all the basic react hooks, event and handler functions, and updating state during next week's discussion.
