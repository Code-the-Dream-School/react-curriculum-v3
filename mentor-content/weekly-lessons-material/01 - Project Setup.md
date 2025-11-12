# Lesson Plan 01 – Introduction to React, App Installation, and Project Setup

## About This Week
This week introduces students to **React** — a frontend library for building dynamic user interfaces — and walks them through setting up a new React project using **Vite**.  
By the end of the week, students should be able to:
- Explain what React is and what problems it solves  
- Understand the difference between declarative and imperative programming  
- Create a new React app using the Vite CLI  
- Run their development server with `npm run dev`  
- Identify key files and folders in a React/Vite project  
- Render basic JSX elements using a functional component  

This lesson establishes the foundation for all React work in the course — understanding the structure and workflow of a modern frontend project.

---

## Explore Session (60 minutes)

**Purpose:** Introduce students to React concepts and show how to scaffold and run a React app using Vite.  
**Materials:** Mentor slides (Explore section), terminal demo, code editor (VS Code), and a browser window.

---

### Segment 1 – Warm-Up (5 min)
Ask:
- “What words come to mind when you hear *React*?”
- “What problems do you think React was built to solve?”

Discussion prompts:
> “Before libraries like React, how do you think developers built interactivity into websites?”  
> “What’s the difference between a *library* and a *framework*?”

**Mentor Tip:**  
Frame React as a *library for managing UI and state*. It doesn’t dictate your app’s full structure (like Angular or Vue) — you’re in control.

---

### Segment 2 – I Do (10 min)
Give a high-level overview of React’s **declarative model** and **Virtual DOM**.

Key ideas to cover:
- **Declarative vs Imperative:** You describe *what* the UI should look like, not *how* to build it step-by-step.
- **Components:** Reusable pieces of UI that each manage their own logic and rendering.
- **Virtual DOM:** A lightweight in-memory representation that React uses to update the browser efficiently.

**Example visual:**  
Show or describe a simple React tree:  
`App → Header → TodoList → TodoItem`.

**CFU Questions:**
- “What does it mean that React is declarative?”
- “Why do components make large apps easier to maintain?”

---

### Segment 3 – We Do (20 min)
Walk through setting up a new project using Vite.

**Step 1 – Create the app**
In the terminal:
    
    npm create vite@latest . -- --template react

Explain:
- `vite` is the build tool
- `--template react` installs React + Vite boilerplate
- `.` means “create in current directory”

**Step 2 – Install dependencies**

    npm install

**Step 3 – Run the dev server**

    npm run dev

Show that it runs locally at `http://localhost:5173`.

**CFU Questions:**
- “What does Vite do for us?”
- “What command will you use every time you want to see your app running?”

**Mentor Tip:**  
If students are using Windows and get permission errors, have them re-run as admin or check Node installation path.

---

### Segment 4 – You Do (20 min)
Students clean up the starter code and create their first component.

Steps:
1. Delete most of the default template (e.g., the spinning React logo).
2. In `App.jsx`, remove the counter logic and replace it with:

        import './App.css'

        function App() {
          return (
            <div>
              <h1>My Todos</h1>
            </div>
          )
        }

        export default App

3. Run `npm run dev` again and confirm the page shows “My Todos”.

4. Next, add a simple array and render it as a list:

        function App() {
          const todos = [
            { id: 1, title: "review resources" },
            { id: 2, title: "take notes" },
            { id: 3, title: "code out app" },
          ];

          return (
            <div>
              <h1>Todo List</h1>
              <ul>
                {todos.map(todo => <li key={todo.id}>{todo.title}</li>)}
              </ul>
            </div>
          );
        }

**CFU Questions:**
- “What does the `.map()` method do here?”
- “Why do we need a `key` prop for each list item?”

---

### Segment 5 – Wrap-Up (5 min)
Summarize:
- React uses **components**, **JSX**, and **state** to manage the UI.
- Vite gives us a fast development server and build system.
- JSX looks like HTML but is really JavaScript under the hood.

Ask:
> “What’s one difference between React and plain HTML/CSS/JS so far?”  
> “How does the React dev server help you while coding?”

---

## Apply Session (60 minutes)

**Purpose:** Reinforce project setup and get students comfortable modifying files and understanding React’s basic structure.  
**Materials:** Mentor slides (Apply section), code editor, and browser window.

---

### Segment 1 – Quick Recap (5 min)
Ask:
- “Who was able to run their project successfully?”
- “What challenges did you hit during setup?”

Show how to open the project folder in VS Code and verify `App.jsx` renders correctly.

---

### Segment 2 – Guided Coding (25 min)
Walk students through **understanding project structure** and **package.json**.

In the root folder, point out key files:
- **`index.html`** — entry point served by Vite  
- **`package.json`** — defines dependencies and scripts  
- **`src/`** — main source code folder  
- **`main.jsx`** — mounts the React app to the DOM  
- **`App.jsx`** — root component of the app  

Show the script section of `package.json`:

    "scripts": {
      "dev": "vite",
      "build": "vite build",
      "lint": "eslint .",
      "preview": "vite preview"
    }

Explain that you’ll run all scripts using `npm run`.

---

### Segment 3 – ESLint and Prettier (Optional, 20 min)
Demonstrate how to install ESLint and Prettier extensions in VS Code (stretch goal).

    npm install eslint-plugin-react --save-dev
    npm install --save-dev --save-exact prettier

Discuss:
- ESLint: catches bugs and enforces coding conventions  
- Prettier: automatically formats code  

Example `.prettierrc` file:

    {
      "semi": true,
      "singleQuote": true,
      "trailingComma": "es5"
    }

**CFU Questions:**
- “Why is consistent formatting important?”
- “When might a linter help you catch a real bug?”

---

### Segment 4 – Wrap-Up and Reflection (10 min)
Discussion prompts:
> “What was your biggest takeaway about React today?”  
> “What do you think is the most important file in your React app right now?”

Remind students:
- Commit and push changes with clear messages.  
- Create a pull request from their setup branch to `main`.  
- Submit their PR link through the form.

---

## Notes for Mentors
- Prioritize **conceptual understanding** (what React is and why it’s used) over memorizing setup commands.  
- Expect installation hiccups — especially around Node versions or missing npm.  
- Encourage questions like: *“What happens if I delete this file?”* or *“Why does this error appear in the console?”*  
- Use the opportunity to connect the dots: Node powers React’s build tools, Vite serves React code, and the browser renders it.  

---

### Optional Extensions
If time allows:
- Show how JSX is transformed into `React.createElement()` behind the scenes.  
- Demonstrate hot module reloading by changing the title live.  
- Show how to import a simple image into the `src/assets` folder and render it in `App.jsx`.  
- Discuss what will come next: components, props, and managing state.
