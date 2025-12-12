# Week 04: Basic Hooks, Events & Handlers, Updating State — Group Mentor Guide

Welcome to Week 4 of the React v3 course! This week, students are learning:

- Basic Hooks: `useState`, `useEffect`, and `useRef`
- Events and Handlers: Synthetic events, handler props, and event propagation
- Updating State: Working with arrays/objects in state, handler composition
- Practical Application: Building a todo list with add functionality

Students are working on their **Todo List app**, adding the ability to submit todos through a form and render them dynamically.

## Warm-Up (5–10 minutes)

Choose one:

**Relationship-Building**  
- What's your favorite productivity system or way to keep track of tasks?
- If you could automate one repetitive task in your daily life, what would it be?

**Check for Understanding (from last week)**  
- What's the difference between props and state?
- How do you pass data from a parent component to a child component?
- What does the `.map()` method do when rendering lists?

## Explore vs. Apply — Session Formats

**Explore Sessions** → Walk through hooks, synthetic events, and state update patterns  
**Apply Sessions** → Debug form submission, troubleshoot state updates, fix event handlers

## Sample Timing for 1-Hour Session

| Time      | Activity                                 |
|-----------|------------------------------------------|
| 0:00–0:10 | Warm-up + review last week               |
| 0:10–0:30 | Explore: hooks and event handling concepts |
| 0:30–0:50 | Apply: live code + assignment help       |
| 0:50–1:00 | Wrap-up + final questions                |

## Check for Understanding (Ask 2–3)

- What does `useState` return? What are the two values in the array?
- Why do we need to create a new array/object when updating state?
- What's the difference between `useEffect` with an empty dependency array vs. no dependency array?
- What does `event.preventDefault()` do?
- How do we pass a function from a parent to a child component?
- What's a synthetic event in React?

## Explore Prompts

Use these to demonstrate key concepts live:

### Hook Fundamentals
- Let's create a simple counter with `useState` together
- What happens if we update state directly instead of using the setter function?
- Why do we need the dependency array in `useEffect`?
- When would we use `useRef` instead of `useState`?

### Event Handling
- How do synthetic events differ from regular browser events?
- What information can we access from an event object?
- How do we prevent default form behavior?

### State Updates
- Why can't we just do `array.push()` or `object.property = value`?
- What's the difference between shallow and deep comparison?

*Mini-Demo Ideas:*

```jsx
// useState with primitives
function Counter() {
  const [count, setCount] = useState(0);
  
  return (
    <button onClick={() => setCount(count + 1)}>
      Clicked {count} times
    </button>
  );
}

// useState with arrays - WRONG vs RIGHT
function WrongArrayUpdate() {
  const [items, setItems] = useState(['apple']);
  
  // WRONG - mutates original array
  function addItem() {
    items.push('banana');
    setItems(items); // React won't detect the change!
  }
  
  // RIGHT - creates new array
  function addItemCorrectly() {
    setItems([...items, 'banana']);
  }
}

// useEffect with dependencies
function SearchComponent() {
  const [query, setQuery] = useState('');
  
  useEffect(() => {
    console.log(`Searching for: ${query}`);
    // This runs when component mounts AND when query changes
  }, [query]);
  
  return <input value={query} onChange={(e) => setQuery(e.target.value)} />;
}

// useRef for DOM access
function FocusInput() {
  const inputRef = useRef(null);
  
  function handleFocus() {
    inputRef.current.focus();
  }
  
  return (
    <>
      <input ref={inputRef} />
      <button onClick={handleFocus}>Focus Input</button>
    </>
  );
}

// Event handlers with event object
function FormExample() {
  function handleSubmit(event) {
    event.preventDefault(); // Prevents page refresh
    console.log(event.target); // The form element
    console.log(event.target.username.value); // Input value
  }
  
  return (
    <form onSubmit={handleSubmit}>
      <input name="username" />
      <button type="submit">Submit</button>
    </form>
  );
}
```

---

## Apply Prompts (Live Coding & Troubleshooting)

### Assignment Hotspots

**Part 1: TodoForm Submission**
* Forgetting `event.preventDefault()` causes page refresh
* Accessing form input value: `event.target.title.value` (must add `name="title"` to input!)
* Clearing input after submit: `event.target.title.value = ""`
* Using `useRef` and `.current.focus()` to refocus input
* Must pass `onAddTodo` as a prop from App → TodoForm

**Part 2: Render Todos from State**
* Renaming state from `newTodo` to `todoList`
* Creating `addTodo` handler in App with `Date.now()` for unique IDs
* Spreading arrays correctly: `[...todoList, newTodo]`
* Passing `todoList` through props: App → TodoList
* Mapping over `todoList` instead of hardcoded `todos` array

### Common Bugs & Solutions

| Issue | Cause | Solution |
|-------|-------|----------|
| Page refreshes on submit | Missing `event.preventDefault()` | Add it as first line in handler |
| Can't access input value | Missing `name` attribute on input | Add `name="title"` to input tag |
| Todos don't appear | Not passing state through props | Pass `todoList` from App to TodoList |
| State not updating | Mutating state directly | Use spread operator: `[...todoList, newTodo]` |
| "Cannot read property of undefined" | Typo in prop name or not destructuring | Check prop names match parent/child |
| Focus doesn't return to input | Not using ref correctly | `inputRef.current.focus()` at end of handler |

### Try This Live

**Walk through the form submission flow step by step:**

```jsx
// In App.jsx
function App() {
  const [todoList, setTodoList] = useState([]);
  
  function addTodo(title) {
    const newTodo = {
      title: title,
      id: Date.now()
    };
    setTodoList([...todoList, newTodo]);
  }
  
  return (
    <div>
      <TodoForm onAddTodo={addTodo} />
      <TodoList todoList={todoList} />
    </div>
  );
}

// In TodoForm.jsx
function TodoForm({ onAddTodo }) {
  const todoTitleInput = useRef(null);
  
  function handleAddTodo(event) {
    event.preventDefault(); // Stop page refresh
    
    const title = event.target.title.value; // Get input value
    onAddTodo(title); // Call parent's handler
    
    event.target.title.value = ""; // Clear input
    todoTitleInput.current.focus(); // Refocus input
  }
  
  return (
    <form onSubmit={handleAddTodo}>
      <input 
        ref={todoTitleInput}
        name="title" 
        type="text" 
      />
      <button type="submit">Add Todo</button>
    </form>
  );
}

// In TodoList.jsx
function TodoList({ todoList }) {
  return (
    <ul>
      {todoList.map(todo => (
        <TodoListItem key={todo.id} todo={todo} />
      ))}
    </ul>
  );
}
```

**Ask:**
* Why do we need `event.preventDefault()`?
* How does the data flow from child to parent?
* Why use `Date.now()` for the ID?
* What would happen if we forgot the `key` prop?

## Key Concepts to Emphasize

### State Updates are Immutable
```jsx
// ❌ WRONG - Mutates state
items.push(newItem);
setItems(items);

// ✅ RIGHT - Creates new array
setItems([...items, newItem]);

// ❌ WRONG - Mutates object
user.name = 'New Name';
setUser(user);

// ✅ RIGHT - Creates new object
setUser({ ...user, name: 'New Name' });
```

### useEffect Dependency Arrays
```jsx
// Runs only on mount
useEffect(() => {
  console.log('Component mounted');
}, []);

// Runs on mount and when count changes
useEffect(() => {
  console.log(`Count is ${count}`);
}, [count]);

// Runs on every render (usually avoid this!)
useEffect(() => {
  console.log('Component rendered');
});
```

### Data Flow Pattern
1. **Parent → Child**: Pass data through props
2. **Child → Parent**: Pass handler function through props, child invokes it
3. **State lives in parent**: Only parent can update its own state

## Engagement Strategies (for quiet groups)

* **Live Debug**: "Let's add a console.log in each function to trace the data flow"
* **Predict the Output**: "Before we click submit, what do you think will happen?"
* **Compare Approaches**: "What's the difference between using a ref vs. controlled component here?"
* **Whiteboard Data Flow**: Draw the component tree and show how data/handlers flow
* **Pair Programming**: "Work together to add the form submission, then we'll review"

## React DevTools Tips

Show students how to:
- View component props in the Components tab
- Track state changes in real-time
- Inspect the component tree hierarchy
- See which components re-render when state changes

**Demo**: Open DevTools while clicking "Add Todo" to watch the state update!

## Optional Challenges

- Add validation to prevent empty todos from being submitted
- Add a character counter that updates as user types
- Create a "Clear All" button that empties the todo list
- Add a todo count display (e.g., "3 todos")
- Make the input field shake if user tries to submit empty todo
- Add timestamps to each todo showing when it was created
- Implement a simple search/filter input above the todo list

## Common Misconceptions

### "Why can't I just modify the array directly?"
React uses shallow comparison to detect changes. If you mutate an array/object in place, the reference stays the same, so React thinks nothing changed and won't re-render.

### "What's the difference between ref and state?"
- **State**: For values that should trigger re-renders when they change
- **Ref**: For values that persist but don't need to trigger re-renders (like DOM node references)

### "Do I need preventDefault for all events?"
No! Only for events that have default browser behavior you want to prevent (like form submission causing page refresh or links navigating).

### "Why do we need both the handler in App AND TodoForm?"
This is the React data flow pattern:
- **App** owns the state, so only it can update the state
- **TodoForm** knows when the user submits, so it invokes the handler
- This separation keeps concerns organized and makes components reusable

## Debugging Checklist

When things aren't working, check:
- [ ] Is `event.preventDefault()` in the handler?
- [ ] Does the input have a `name` attribute?
- [ ] Is the handler prop name consistent? (parent passes `onAddTodo`, child receives `onAddTodo`)
- [ ] Is the state update function creating a NEW array/object?
- [ ] Are props being passed correctly? (Check React DevTools)
- [ ] Is the handler being invoked vs. passed? (`onClick={handler}` not `onClick={handler()}`)
- [ ] Is the `key` prop set on mapped elements?

## Week 4 Terminology Review

| Term | Definition |
|------|------------|
| **Hook** | Special React function that lets you use state and other features (starts with `use`) |
| **useState** | Hook that returns a state value and update function |
| **useEffect** | Hook for side effects (runs after render, based on dependencies) |
| **useRef** | Hook that returns a mutable ref object that persists across renders |
| **Synthetic Event** | React's cross-browser wrapper around native browser events |
| **Handler/Callback** | Function passed as a prop that gets invoked to communicate back to parent |
| **Dependency Array** | Array passed to useEffect that determines when it re-runs |
| **Immutable Update** | Creating new arrays/objects instead of mutating existing ones |

## Mentor To-Do
- [ ] Run a session using this guide  
- [ ] Help students understand state immutability
- [ ] Walk through form submission flow step-by-step
- [ ] Demonstrate data flow between parent and child components
- [ ] Submit your [Mentor Session Report](https://airtable.com/appoSRJMlXH9KvE6w/shrp0jjRtoMyTXRzh)
