# Week 2: ReactDOM, Components, JSX & Troubleshooting — Group Mentor Guide

This week, students:

- Learned how `ReactDOM` renders an app inside the browser using `createRoot`
- Built their first *real* components (`TodoList`, `TodoForm`)
- Practiced JSX syntax and the component tree structure
- Used error messages and dev tools to debug issues

They’re working in the **week-02-components** branch of their `learns-app` repo, modifying `App.jsx`, and creating `TodoList.jsx` and `TodoForm.jsx`.

## 🧊 Warm-Up (5–10 minutes)

Choose one warm-up to start your session:

**👋 Relationship-Building**
- What’s something visual or interactive you've always wanted to build in an app?
- What’s your favorite “interface” — real or digital?

**💡 Check for Understanding (from Week 1)**
- What’s the role of `App.jsx` in a React project?
- What files did you edit last week, and what were you trying to build?
- What did `Vite` help you do?

## 🧭 Explore vs. Apply — Session Formats

**Explore Sessions** → Big-picture discussion, code walkthroughs  
**Apply Sessions** → Hands-on coding, debugging, review of assignment code

Mix both as needed!

## ⏱️ Sample Timing for 1-Hour Session

| Time      | Activity                              |
|-----------|---------------------------------------|
| 0:00–0:10 | Warm-up + Week 1 check-in             |
| 0:10–0:30 | Explore JSX, components, structure     |
| 0:30–0:50 | Apply: live coding or troubleshoot     |
| 0:50–1:00 | Wrap-up: questions, tips, check-ins    |

## ❓ Check for Understanding (Pick 2–3)

- What is a React component? What’s special about it?
- What are the three arguments to `React.createElement`?
- What’s the difference between HTML and JSX?
- Why must all JSX tags be closed?
- What error did you get this week, and how did you fix it?

## 🧑‍🏫 Explore Prompts

- Why does React use components instead of plain HTML + JS?
- When would you split something into a new component?
- Can someone walk us through what `StrictMode` does?
- How does Vite find the `#root` div and render your app?

🧑‍💻 *Mini-Demo Idea:*  
> Live-code a simple component and show how to:
> - Pass props (like `title="My Todo List"`)
> - Use JSX
> - Introduce a basic error and fix it together

## 🛠️ Apply Prompts (Live Coding & Troubleshooting)

### 🔧 Assignment Hotspots
- Not closing JSX tags
- Forgetting `return` inside a component
- Returning multiple JSX elements without using `<>...</>`
- Moving JSX to a new component but forgetting to `export default`
- Forgetting to import the new component into `App.jsx`

### ✅ Try This Live

> “Let’s build a simple component together!”

```js
// TodoItem.jsx
function TodoItem({ title }) {
  return <li>{title}</li>;
}
export default TodoItem;
</code></pre>

> “Now let’s use it inside a fake list!”
```

```js
// App.jsx
function App() {
  return (
    <ul>
      <TodoItem title="Buy eggs" />
      <TodoItem title="Take a nap" />
    </ul>
  );
}
</code></pre>
```

Ask:  
- “What happens if we forget to close a tag?”
- “Can we map over an array of titles and render multiple?”

## 💬 Engagement Strategies

- **Fix This JSX**: Show an invalid snippet (missing slash, nested incorrectly) and ask what’s wrong.
- **Component Tree Sketch**: “Can someone sketch the component tree on screen or describe it aloud?”
- **Think-Pair-Share**: Have everyone fix an error solo, then explain what happened to a partner.
- **Chat First**: “Write your component name + one thing it renders in the chat.”

## 💡 Optional Challenges

For advanced students or bonus time:

- Add a new component: `TodoHeader` or `Footer`
- Use inline styles with a `style={{}}` object
- Add a component that renders based on a conditional (`isComplete`)

## 📎 Resources & Links

- [Assignment Instructions (Week 2)](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/assignments/week-02.md)
- [React.createElement (Docs)](https://react.dev/reference/react/createElement)
- [JSX Rules (React Docs)](https://react.dev/learn/writing-markup-with-jsx)
- [React Developer Tools](https://react.dev/learn/react-developer-tools)

## ✅ Mentor To-Do

- [ ] Run an Explore or Apply session using this guide
- [ ] Help students debug or extend their components
- [ ] Submit your [Mentor Session Report](https://airtable.com/appoSRJMlXH9KvE6w/shrp0jjRtoMyTXRzh)
