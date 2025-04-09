## Weekly Assignment Instructions

### Expected App Capabilities

After completing this week's assignment, your app should:

- allow users add a todo
- retain the `input`'s focus when a todo is submitted with the button or enter key
- render entered todos in a list

### Instructions Part 1: TodoForm Submission

#### Prepare App to Manage Todo State

Refactor App with the following changes:

- Remove the paragraph tag and its contents from the return statement.
- Rename the existing state value, `newTodo`, to `todoList` and the update function, `setNewTodo`, to `setTodoList`.
- Change the `useState`'s `initialValue` to an empty array.
- Between the `useState` and the return statement, create a handler function named `addTodo`
  - It takes an argument `title`
  - Create a const `newTodo` with an object that uses `title`, and `id` as keys.
    - Set the `id` using `Date.now()`.
  - The handler then calls `setTodoList` passing in an array containing the destructured `todoList` and `newTodo`. It should look like: `setTodoList([...todoList, newTodo])`
- Pass the function to an `onAddTodo` props on the `TodoForm` instance.

#### Adding an Event Handler to the Form

Go to the `TodoForm` and make the following changes:

- On the `input` tag, add a `name` props with the value of "title".
- Define a `handleAddTodo`function above the return statement:
  - The function takes an `event` object
  - Call the `event.preventDefault()` in the first line of the function to prevent the page from refreshing when a user clicks the Add Todo button.
- Add a `console.dir(event.target)` - *we will use this console statement to figure out how to access the input value from the form*.

This is how `handleAddTodo` should look so far:

```jsx
{/*extract from TodoForm.jsx*/}
{/*...code*/}
function handleAddTodo(event){
    event.preventDefault()
    console.dir(event.target)
}
{/*code continues...*/}
```

- On the form, add an `onSubmit` event listener and pass in `handleAddTodo`: `<form onSubmit={handleAddTodo}>`. When the form submits, the event listener will fire off the function and pass it the submit event's event object.
- Enter some text in the input on the browser window and hit the Add Todo button.

![todo being entered](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/assignments/assets/week-04/type-todo.gif)

You should see the event object in the console statement:

![event object containing form data](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/assignments/assets/week-04/form-data.png)

> [!note]
> All screenshots are from a Chrome-based browser. Firefox or Safari display objects in the console differently.

Since we added `name="title"` to the input's props, we can reference the input with `event.target.title`.

- Update the `console.dir` statement to log out `event.target.title` and then hit the button again. The console now logs:

![object containing data about the todoTitle input](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/assignments/assets/week-04/todoTitle-data.png)

- The console statement shows that you can access the input's value with `event.target.title.value`.
- Back in the `handleAddTodo`, remove the console and create a new `const title` and assign it the event target's title value.
- Invoke `onAddTodo` that you get from the component's props and pass the `title` into the function's params.
- To help the user out, you then clear out the input by setting the input's value to an empty string. `event.target.title.value = ""`

#### Re-focus Input After Submit

When a user hits enter after adding a todo, the form retains focus so the user can rapidly enter more todos. However, when they click on the submit button, they then have to click on the field again to return the focus. Note below how they have to click into the field again before entering text.

![typing a todo but input focus is lost when todo is added](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/assignments/assets/week-04/type-todo.gif)

We can improve the user's experience by using a `ref` on the input element. Recall that this can act like a selector (`document.querySelector("#todoTitle")`), so we can use it to re-focus the input in the `handleAddTodo` event handler.

- Import `useRef`
- Invoke `useRef` in the body of the component with an empty string and assign it to a const, `todoTitleInput`.
- In the `input` element, add a `ref` props that takes the `todoTitleInput`.
- Finally, at the end of the event handler, we need to reference the `current` property on the `ref` and then chain on a `focus()` method. It will look something like: `todoTitleInput.current.focus();`

With that in place, a user should now be able to click the Add Todo button and the focus will return to the field.

![input retains focus after submitting new todo](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/assignments/assets/week-04/enter-todo-2.gif)

### Instructions Part 2: Render Todos from State

- In `App`, add the `todoList` state value to the props of the `TodoList` component instance.
- In the `TodoList` component:
  - Destructure `todoList` out of the component's props argument.
  - Remove the `todos` array.
  - Replace the reference to `todos` with `todoList` so that `todoList` is now being mapped to `TodoListItem`.

Your `TodoList` component should now look like:

```jsx
{/*extract from TodoList.jsx*/}
import TodoListItem from "./TodoListItem"

function TodoList({todoList}){

    return(
        <ul>
            {todoList.map(todo => <TodoListItem key={todo.id} todo={todo} />)}
        </ul>
    )
}

export default TodoList
```

Back over in the browser, we are still rendering the `TodoList` but it's children are now tied to the `todoList` state. Since the `initialValue` is an empty array, no `TodoListItem`s are rendered.

![no todos rendered in react dev tools](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/assignments/assets/week-04/no-todos.png)

- In the browser, type a new todo in the input - the todo form should now add items to your list!

![TodoListItems rendering as todos entered into form](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/assignments/assets/week-04/enter-todo.gif)

### Closing Notes

Next week, we'll improve the way we handle the form when we talk about "controlled components". The approach we used to form is a little awkward but it allowed us to examine the event object from the form submission. We'll also:

- allow the user to edit or complete todo list items
- persist the todoList to local storage
