## Weekly Assignment Instructions

### Expected App Capabilities

After completing this week's assignment, your app should:

- Contain a TodoList component that contains all todo code
- Contain a TodoForm component with
  - a non-functioning form with 1 input field and a submit button
- Display a h1 heading, a todo entry form, and the todos we added last week

### Instructions Part 0: Pre-Work Version Control Tasks

*Before proceeding, make sure your week-01 lesson has been approved.*

- Merge your PR in GitHub
- On your local environment, check out `main` and then pull down the latest changes:

```terminal
git checkout main
git pull
```

- Create and checkout a working branch for this week: `git checkout -b week-02-components`
- Publish the branch to GitHub: `git push origin week-02-components`
- Start up your development server and open the app in your browser.

### Instructions Part 1: TodoList Component

- Create a new file, `TodoList.jsx` inside the `src` directory
- Scaffold a component inside that file with the name `TodoList`. This includes:
  - the function definition
  - an empty return statement
  - export default TodoList at the bottom of the file

At this point, the TodoList component should look like:

```jsx
{/*extract from TodoList.jsx*/}
function TodoList(){
    return
}

export default TodoList
```

- In the `App` component, import `TodoList`
- Below the heading tag in the JSX, add an instance of the `TodoList`
- Move the `todos` array over to `TodoList`
- Observe the errors that appear in the browser console. You'll see something like:

![screen capture of ReferenceError in browser console](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/assignments/assets/week-02/reference-error.png)

Errors like list are common during development and help us out quite a bit. Make sure you understand what the "Uncaught ReferenceError" message means and where it comes from. You may not know what an "error boundary" in the second message. Take an opportunity to do some research into what that message may mean. Our next step will resolve this.

- Move the unordered list and the `todos.map...` to the return statement in `TodoList`. Remember that parens `()` can be used for multi-line return statements.
- Refresh the browser page and those error messages should be gone. The page should also look the same as it did before.

### Instructions Part 2: TodoForm Component

- Create a new file, `TodoForm.jsx` in the `src` directory.
- Create a component with the same name in that file.
- Add an empty form to `TodoForm`'s return statement.
- In the form add
  - a label with an `htmlFor` props set to `todoTitle`.
  - between the label tags, insert the text, "Todo"
  - a text input with an id of `todoTitle`
  - a button with the text "Add Todo"
- Import the `TodoForm` into `App`
- Place an instance of `TodoForm` between the heading and `TodoList`

 The app should now look like:

![screen capture of todo list with new todo form in the browser](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/assignments/assets/week-02/todo-list-with-form.png)

### Instructions Part 3: Final Steps

#### Version Control Closeout Tasks

- Commit changes to your local working branch.
- Push the changes up to GitHub.
- In GitHub, create a PR (pull request) that compares the working branch to `main`.
- Copy the PR link and submit assignment.

### Closing Notes

After this week, the instructions will start assuming some knowledge of the topics and tasks covered the previous weeks. As a result, fewer code examples will be given in instructions. You will also be responsible for remembering all basic version control tasks - merging, pulling, committing, publishing to GH, etc.

As a React student, it is important to gain practice in working through some tasks on your own and being able to reach out for assistance when you get stuck. We aim to make this course challenging but it won't be impossible either! Any areas that are particularly challenging or confusing will include tips or hints that can help out.
