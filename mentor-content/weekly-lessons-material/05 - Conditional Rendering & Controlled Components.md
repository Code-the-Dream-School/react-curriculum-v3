# Lesson Plan 05 – Conditional Rendering & Controlled Components

## About This Week
This week students learn two fundamental React patterns: conditional rendering (showing/hiding UI elements) and controlled components (managing form inputs with React state). These concepts are essential for building interactive applications. Students will apply these patterns to their todo app by adding conditional messages, checkboxes to mark todos complete, and converting their form to be controlled by React state.



## Explore Session (60 minutes)

**Purpose:** Introduce conditional rendering and controlled components through live examples before students tackle the assignment.  
**Materials:** Code editor with simple React setup, mentor slides.

### Segment 1 – Warm-Up (5 min)
- Ask: "What are some examples of things that appear or disappear on websites you use?" (e.g., navigation menus, error messages, loading spinners)
- Quick discussion: "How do you think React decides what to show and hide?"

*Mentor Tip: Connect to real-world examples students already know—dropdown menus, modals, form validation messages.*



### Segment 2 – I Do: Conditional Rendering (15 min)

Demonstrate the three main approaches:

**1. Ternary Operator**
```jsx
function Greeting({ isLoggedIn }) {
  return (
    <div>
      {isLoggedIn ? <h1>Welcome back!</h1> : <h1>Please sign in</h1>}
    </div>
  );
}
```

**2. Logical && Operator**
```jsx
function ShoppingCart({ items }) {
  return (
    <div>
      <h2>Your Cart</h2>
      {items.length === 0 && <p>Your cart is empty</p>}
    </div>
  );
}
```

**3. Variable/Function (for complex logic)**
```jsx
function StatusMessage({ status }) {
  function getStatusDisplay() {
    if (status === 'loading') return <LoadingSpinner />;
    if (status === 'error') return <ErrorMessage />;
    return <SuccessMessage />;
  }
  
  return <div>{getStatusDisplay()}</div>;
}
```

**CFU Questions:**
- "When would you use `&&` vs a ternary operator?"
- "What gets rendered if the condition is false in a `&&` statement?"

*Mentor Tip: Show these live—change the boolean values in React DevTools to demonstrate the conditional rendering in action.*



### Segment 3 – We Do: Practice Conditional Rendering (15 min)

Together, build a simple toggle example:

```jsx
function MessageToggle() {
  const [showMessage, setShowMessage] = useState(false);
  
  return (
    <div>
      <button onClick={() => setShowMessage(!showMessage)}>
        Toggle Message
      </button>
      {showMessage && <p>Hello, I'm visible now!</p>}
    </div>
  );
}
```

Then extend it:
- "How would we show 'Show Message' or 'Hide Message' on the button based on state?"
- Work through converting the `&&` to a ternary

**CFU Questions:**
- "Why do we need `!showMessage` in the onClick?"
- "What would happen if we just wrote `onClick={setShowMessage(true)}`?"



### Segment 4 – I Do: Controlled Components (15 min)

Explain: "A controlled component is when React state controls what's shown in a form input."

**Show the difference:**

**Uncontrolled (regular HTML):**
```jsx
function UncontrolledForm() {
  return <input type="text" />;
  // React doesn't know what's in this input!
}
```

**Controlled (React manages it):**
```jsx
function ControlledForm() {
  const [name, setName] = useState('');
  
  return (
    <input 
      type="text" 
      value={name}
      onChange={(e) => setName(e.target.value)}
    />
  );
}
```

Emphasize the cycle:
1. User types → `onChange` fires
2. `setName` updates state
3. Component re-renders with new value
4. Input shows updated value

**CFU Questions:**
- "What happens if we have `value` but no `onChange`?"
- "Why does `onChange` take a function with `(e)` parameter?"
- "What is `e.target.value`?"

*Mentor Tip: Type in the input slowly and narrate each step of the update cycle.*



### Segment 5 – We Do: Build a Controlled Input (10 min)

Build a simple controlled form together:

```jsx
function NameForm() {
  const [firstName, setFirstName] = useState('');
  
  function handleSubmit(e) {
    e.preventDefault();
    console.log('Name submitted:', firstName);
  }
  
  return (
    <form onSubmit={handleSubmit}>
      <input
        type="text"
        value={firstName}
        onChange={(e) => setFirstName(e.target.value)}
      />
      <button type="submit">Submit</button>
      <p>You typed: {firstName}</p>
    </form>
  );
}
```

**CFU Questions:**
- "Why do we need `e.preventDefault()`?"
- "How could we disable the button when firstName is empty?"



### Segment 6 – Wrap-Up (5 min)
- Recap: "Conditional rendering = showing/hiding UI. Controlled components = React manages form inputs."
- Preview assignment: "You'll add a message when the todo list is empty, add checkboxes to mark todos complete, and make your form controlled."
- Remind: "The assignment breaks this down step-by-step. Don't try to do it all at once!"



## Apply Session (60 minutes)

**Purpose:** Support students as they complete the week 5 assignment, focusing on common sticking points.  
**Materials:** Assignment instructions, student repos, breakout rooms optional.

### Segment 1 – Quick Recap (5 min)
- Ask: "What's one thing about conditional rendering or controlled components that clicked for you?"
- Address any confusion from Explore session or async questions



### Segment 2 – Walkthrough Part 1: Conditional Message (10 min)

Demo solving the first part together:

```jsx
// In TodoList.jsx
function TodoList({ todoList, onCompleteTodo }) {
  return (
    <div>
      {todoList.length === 0 ? (
        <p>Add todo above to get started</p>
      ) : (
        <ul>
          {todoList.map((todo) => (
            <TodoListItem 
              key={todo.id} 
              todo={todo}
              onCompleteTodo={onCompleteTodo}
            />
          ))}
        </ul>
      )}
    </div>
  );
}
```

**CFU Questions:**
- "Why does the ternary check `length === 0` and not just `length`?"
- "What would `&&` look like here instead of ternary?"

*Mentor Tip: Have students test this by adding/removing all todos to see the message appear/disappear.*



### Segment 3 – Walkthrough Part 2: Complete Todo (15 min)

Walk through the checkbox feature step-by-step:

**Step 1: Update todo schema in App.jsx**
```jsx
function addTodo(title) {
  const newTodo = {
    title: title,
    id: Date.now(),
    isCompleted: false  // New property
  };
  setTodoList([...todoList, newTodo]);
}
```

**Step 2: Create completeTodo handler**
```jsx
function completeTodo(id) {
  const updatedTodos = todoList.map((todo) => {
    if (todo.id === id) {
      return { ...todo, isCompleted: true };
    }
    return todo;
  });
  setTodoList(updatedTodos);
}
```

**Step 3: Pass it down as props**
```jsx
<TodoList 
  todoList={todoList} 
  onCompleteTodo={completeTodo}  // Pass it here
/>
```

**Step 4: Add checkbox in TodoListItem**
```jsx
function TodoListItem({ todo, onCompleteTodo }) {
  return (
    <li>
      <form>
        <input
          type="checkbox"
          checked={todo.isCompleted}
          onChange={() => onCompleteTodo(todo.id)}
        />
        {todo.title}
      </form>
    </li>
  );
}
```

**Step 5: Filter completed todos in TodoList**
```jsx
const filteredTodoList = todoList.filter(todo => !todo.isCompleted);
// Then map over filteredTodoList instead of todoList
```

**CFU Questions:**
- "Why do we use `.map()` in completeTodo instead of directly modifying the todo?"
- "Why does onChange use `() => onCompleteTodo(todo.id)` instead of just `onCompleteTodo(todo.id)`?"

*Mentor Tip: Emphasize the prop-drilling path: App → TodoList → TodoListItem*



### Segment 4 – Guided Work: Controlled Form (20 min)

Students work on converting TodoForm to controlled component with guidance:

**Key steps to emphasize:**

1. Import useState and create state:
```jsx
const [workingTodoTitle, setWorkingTodoTitle] = useState('');
```

2. Connect input to state:
```jsx
<input
  value={workingTodoTitle}
  onChange={(e) => setWorkingTodoTitle(e.target.value)}
/>
```

3. Update handleAddTodo:
```jsx
function handleAddTodo(e) {
  e.preventDefault();
  onAddTodo(workingTodoTitle);  // Use state instead of e.target
  setWorkingTodoTitle('');  // Reset form
}
```

4. Disable button when empty:
```jsx
<button disabled={workingTodoTitle === ''}>Add Todo</button>
```

Circulate or use breakout rooms. Common issues to watch for:
- Forgetting `onChange` (input becomes read-only)
- Calling `onAddTodo` wrong (passing event object instead of state)
- Not resetting the form after submit



### Segment 5 – Debug & Share (10 min)

- Invite 1-2 students to share their screen and show their working code
- Or share common bugs you noticed and solve them together
- Celebrate wins: "Who got their checkbox working? Who got their form controlled?"

**CFU Questions:**
- "What was the hardest part this week?"
- "What's one thing you'll remember about controlled components?"



### Segment 6 – Wrap-Up (5 min)

Reminders:
- Test all four parts of the assignment before submitting
- Submit your PR link in the form
- Don't merge until you get feedback

Preview next week: "Next week we'll refactor and organize our React code better as the app grows!"

*Mentor Tip: Remind students that prop drilling (passing props through multiple components) can feel repetitive—that's normal and next week they'll learn patterns to manage it.*



## Key Teaching Points

### Conditional Rendering
- **Ternary for 2 options:** `condition ? <ComponentA /> : <ComponentB />`
- **&& for show/hide:** `condition && <Component />`
- **null hides elements:** `condition ? <Component /> : null`

### Controlled Components
- Must have both `value` and `onChange`
- State is the "single source of truth"
- Update cycle: user input → onChange → setState → re-render → input shows new value

### Common Student Struggles
1. **Forgetting to call functions in event handlers:** `onClick={handler}` not `onClick={handler()}`
2. **Direct state mutation:** Using `.map()` to create new arrays instead of modifying existing ones
3. **Not understanding `e.target.value`:** What `e` is and where `.target.value` comes from
4. **Prop drilling confusion:** Losing track of where props come from and where they go



## Questions to Assess Understanding

**Conditional Rendering:**
- "When would you use `&&` vs a ternary operator?"
- "What gets rendered if we write `{false && <Component />}`?"

**Controlled Components:**
- "What makes a component 'controlled'?"
- "What happens if we forget the `onChange` handler?"
- "Why do we need `e.preventDefault()` in form submission?"

**React Patterns:**
- "Why can't we modify state directly?"
- "Why do we pass functions as props instead of calling them in the parent?"



## Additional Resources (if students want to dig deeper)

- [React Docs: Conditional Rendering](https://react.dev/learn/conditional-rendering)
- [React Docs: Responding to Events](https://react.dev/learn/responding-to-events)
- [React Docs: Managing State](https://react.dev/learn/managing-state)



*Remember: This week combines two big concepts. Students may feel overwhelmed—reassure them it's normal and these patterns will become second nature with practice.*
