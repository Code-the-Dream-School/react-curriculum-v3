## Weekly Assignment Instructions

## Expected App Capabilities

After completing this week's assignment, your app should:

- conditionally render a message when the todo list is empty
- disable the Add Todo button when the input is empty
- allow users to complete a todo
- utilize a controlled form

## Instructions Part 1: Conditional Rendering a Message

In TodoList.jsx:

- create a [ternary statement](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Conditional_operator) that compares the todoList's length to zero.
  - if `true`, render a paragraph tag that includes the text "Add todo above to get started"
  - if `false`, render the unordered list includes the mapped todo list.

Your app should show the paragraph if the list is empty or the todos if the `todoList` contains any.

![hitting enter adds todo](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/assignments/assets/week-05/enter-todo.gif)

## Instructions Part 2: Mark Todo as Complete

Rather than deleting a todo item outright, we want to indicate it has been completed. We'll update the schema that the todos use and create a helper function that toggles the value when the user clicks a checkbox on the todo.

- In `App`, update `newTodo` in the `addTodo` event handler with a new property called `isCompleted` and set it to `false`. Each todo should now consist of 3 properties: `title`, `id`, and `isCompleted`.
- Create a helper function `completeTodo`. It:
  - takes an `id`
  - maps through the `todoList` and:
  - if the current `todo.id` matches the `id`, return a new object that destructures the current todo and `isCompleted` set to `true`
  - otherwise (if `todo.id` does not match the `id`) return the `todo`
  - saves the resulting array to a const `updatedTodos`
  - update the `todoList` state with `updatedTodos`

the function will use logic very similar to the example below:

```js
function favoriteColor(colorId) {
    const updatedColors = colors.map((color) => {
        if (color.id === colorId) {
        return { ...color, isFavorite: true };
    }
        return color;
    });
    //update `colors` state with `updatedColors`
}
```

- pass your new helper function to a `TodoList` props named `onCompleteTodo`
- in `TodoList`, destructure that out of the component's props.
- add the same props to the `TodoListItem` instance and pass in that helper.
- in `TodoListItem`
  - destructure the function out of props
  - create a form element in the list item and move `{todo.title}` into it.
  - above that, create an input
    - give it the type of "checkbox"
    - add the prop `checked` and set it equal to `todo.isCompleted`
    - add an `onChange` event listener and then invoke an anonymous function that returns the helper function with the `todo` object's id.

> [!remember]
> You do not want to call the helper function directly in an event listener props. If you do this, the function will immediately fire off when the component renders, causing bugs!

To assist with the structure of the components elements, follow this layout:

```jsx
{/*extract from TodoListItem.jsx*/}
function TodoListItem({ todo, onCompleteTodo }) {
  return (
    <li>
      <form>
        <input
          {/* set the `type` prop to "checkbox" */}
          {/* add the `checked` props */}
          {/* add `onChange` event listener that uses the `onCompleteTodo` helper` */}
        />
        {todo.title}
      </form>
    </li>
  );
}

export default TodoListItem;
```

Each todo should now have a checkbox. When the checkbox is clicked, the helper function will set `isCompleted` to `true`. Our final step is to filter out the completed todos.

In `TodoList`, we need to filter out the completed todos before we map the todos to `TodoListItem` instances.

- create a constant `filteredTodoList` that contains the `todoList` that has been filtered to remove any todo whose `.isCompleted` property is `true`
- replace the `todoList` references in the JSX with the `filteredTodoList`

Now, when a user clicks on the checkbox beside an item, that todo disappears from the list.

![submit and complete a todo](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/assignments/assets/week-05/add-complete-todo.gif)

## Instructions Part 3: Convert Form to Controlled Component

### Create Local State

- import `useState` into `TodoForm`
- create a `workingTodoTitle` state variable with the accompanying state update function.
- give the `useState` an initial value of an empty string.

### Connect Form Input to Component State

- to the `input` element in the form, add a `value` props set to `workingTodoTitle`.
- add an `onChange` listener to the `input`.
  - it should call an anonymous arrow function that takes in the event object.
  - the anonymous function should call `setWorkingTodo` and pass in the event's target value.

### Update `handleAddTodo`

- remove the lines that access the title's value from the event object
  - hint: there's one that retrieves the value and one that sets the target's title `value` back to `""`
- update `onAddTodo` so that it gets passed `workingTodoTitle` instead of `title`.
- after `onAddTodo`, call the `setWorkingTodo` state update function with an empty string to reset the form input.

## Instructions Part 4: Disable Form Button when Input is Empty

We are pretty close to being complete for this week but there is one minor problem with the interface. A user is able to add empty todos to their list. Since the form was converted to a controlled component, we can do so based on the value of the working state in the component.

![user is able to hit add todo even when input is empty](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/assignments/assets/week-05/submit-enabled.gif)

- In `TodoForm`'s button element, add a `disabled` prop that evaluates to true when `workingTodo` is an empty string.

![submission button disabled when input is empty](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/assignments/assets/week-05/submit-disabled.gif)
