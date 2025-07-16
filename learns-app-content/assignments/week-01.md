## Weekly Assignment Instructions

>[!note]
>We are building a todo app for the weekly assignments. You are welcome to code along with CTD Swag but that project will not be turned in.

### Expected App Capabilities

After completing this week's assignment, your app should:

- be in a version controlled directory linked to a GitHub repo
- use Vite's React template using JavaScript (no TS or SWC)
- start with no console errors or warnings
- render a title and an unordered list of todos

### Instructions Part 1: Repo Setup for the Todo App

#### Create new public repo on GitHub

- give it the name "todo-list" or something similar and description
- don't add a .gitignore or a license
- clone the repo to your local environment (this differs based on if you're using HTTPS, SSH, or GitHub Desktop. See [GitHub's documentation on remote repos](https://docs.github.com/en/get-started/getting-started-with-git/about-remote-repositories) for more details.)
- don't create a new branch yet - remain on `main` branch

### Instructions Part 2: Installation

#### Scaffold Vite Using CLI

- Bootstrap a new project with command: `npm create vite@latest . -- --template react`
- Install with command: `npm install`

You will end up with a project structure that looks similar to the following:

![screen capture of the newly installed project directory](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/assignments/assets/week-01/project-directory.png)

### Instructions Part 3: Project Setup

> [!notes]
>
> - Some of the cleanup steps may cause errors to appear in the browser window and console but they will resolve themselves.
> - The odd, html-like syntax in the return statements is called JSX - we'll cover that next week.
> - We also won't provide all the code snippets in every assignment. The only reason why we're doing that here is because you have not been introduced to JSX yet.

#### Version Control Tasks

We want to preserve a fresh installation of the project in case anything happens to our code. Rather than having to re-install, we can revert back to this version!

- Stage all the files with git: `git add .`
- Commit them to main: `git commit -m "installed react"`
- Push the changes to GitHub: `git push`
- Create and check out a new branch for week 1's assignment before continuing: `git checkout -b week-01-setup`
- Publish the branch to github: `git push origin week-01-setup`

#### Clean up Template

- Start the development server with the command: `npm run dev`
- Open a browser to the **Local** link listed in the CLI
- Delete the contents of App.css but keep the file.
- Delete the contents of index.css but keep the file.
- Clean up App's code:
  - Remove all imports except for App.css
  - Delete the line containing `const [count, setCount] = useState(0)`
  - Remove everything from the return statement.
  - In the now empty return statement, add a div containing an h1 for the title for the app

Your App component should look like:

```jsx
import './App.css'

function App() {

  return (
    <div>
      <h1>My Todos</h1>
    </div>
  )
}

export default App
```

Refresh the page and you'll end up with something that looks like the screencap below. Note that the console contains no errors.

![screen capture of app title in browser](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/assignments/assets/week-01/title-screencap.png)

#### Add First Todos

In App.jsx:

- After App's first line but before the return statement, create an array named `todos`containing 3 empty objects. Each one of those objects will represent a todo.
- Populate each object with a todo using the following object keys: `id` and `title`.

```jsx
{/*extract from App.jsx*/}
function App() {
const todos = [
    {id: 1, title: "review resources"},
    {id: 2, title: "take notes"},
    {id: 3, title: "code out app"},
]
{/*code continues...*/}
```

- Below the h1 in the return statement, create an unordered list using html tags.
- Place an empty code block between the list's opening and closing tags `<ul>{}</ul>`
- Inside the code block, map the todos to html that will render a list item per todo.

```jsx
{/*extract from App.jsx*/}
{/*...code*/}
return (
    <div>
        <h1>Todo List</h1>
        <ul>
            {todos.map(todo => <li key={todo.id}>{todo.title}</li>)}
        </ul>
    </div>
  );
{/*code continues...*/}
```

In the browser, you should have a list of 3 todos under the app's title:

![screen capture of the todos in browser](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/assignments/assets/week-01/todos-screencap.png)

### Stretch Goals Instructions (optional)

*Skip to **Instructions Part 4** if you are not interested in ESLint or Prettier.*

#### Enable ESLint to Highlight Code Problems  (Stretch goal 1 of 2)

- in VS Code, click on the extensions icon in the left-hand sidebar

![screen capture highlighting extension icon in VS Code's tool tray](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/assignments/assets/week-01/extensions-icon.png)

- search for "ESLint". The appropriate extension will have the following icon:

![screen capture of the VS Code extension search results for ESLint](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/assignments/assets/week-01/eslint.png)

- You need to add another plugin package, eslint-plugin-react, for additional usefulReact linting rules:
  - In the terminal, install it using `npm install eslint-plugin-react --save-dev`
  - Go to `eslint.config.js` add the import statement `import react from 'eslint-plugin-react';` above the `reactHooks` import.
  - In the second object in the exported array:
    - Add a `settings` object: `settings: { react: { version: 'detect' } },`.
    - This should be placed just above the `plugins` object.
  - Add `react` to the `plugins` object, above `react-hooks: reactHooks`. *tip: You don't need to include a key/value pair like the other plugins.*
  - Add the following to the `rules` below `...js.configs.recommended.rules,`
    - `...react.configs.recommended.rules,`
    - `...react.configs['jsx-runtime'].rules,`
- Take some time to review the `eslint.config.js` file you just updated. Documentation for the [ESLint plugin](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint) and [ESLint library](https://eslint.org/) can explain most of the contents.
- Finally, we recommend adding the following entries to the `rules` object:

```js
rules: {
        //...other rules
        'no-unused-vars': 'warn', //this changes the error to a warning
        'react/prop-types': 'off', //this suppresses warnings about not using prop-types
        //other rules...
}
```

#### Integrate Prettier (Stretch goal 2 of 2)

##### Install Prettier in VS Code

- search for "Prettier". Choose the following from the results:

![screen capture of the VS Code extension search results for Prettier](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/assignments/assets/week-01/prettier.png)

- after installed, go to VS Code's settings and search for "format"
  - find the "Default Formatter" setting and change the option to Prettier - Code formatter.
  - If you've previously modified VS Code settings, refer to the full installation instructions on the [Prettier's VS Code Extension homepage](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode) to to help with troubleshooting.

##### Install Prettier in the Project

- In the terminal issue the command: `npm install --save-dev --save-exact prettier`
- Create a .prettierrc file at the root of the project directory
  - You can use this to add any special formatting rules by adding a JSON object
  - All configuration options can be found on [Prettier's configuration page](https://prettier.io/docs/configuration).
  - We recommend starting the snippet below and modify it based on what you find useful.

```json
{
"semi": true,
"singleQuote": true,
"trailingComma": "es5"
}
```

### Instructions Part 4: Final Steps

#### Update the Project's README

> [!note]
> Write in with whatever language you are most comfortable. We suggest you also use [markdown](https://gist.github.com/Myndex/5140d6fe98519bb15c503c490e713233) for any formatting - it's a simple document markup syntax that can be learned very quickly.

- Open the README.md in the root of the project and empty its contents.
- Write some basics about the project. Include at least the following:
  - App name and description
  - Installation instructions
  - How to run the development server

#### Version Control Closeout Tasks

- Commit changes to your local working branch.
- Push the changes up to GitHub (publish the branch there if you have not done so already).
- In GitHub, create a PR (pull request) that compares the working branch to `main`.
- Copy the PR link and submit assignment.
