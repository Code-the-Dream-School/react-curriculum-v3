# Lesson Plan 08 – Reusable Components, Refactoring, Project Organization & Testing

## About This Week
This week is a milestone—students learn professional React practices that separate hobbyist code from production-ready applications. They'll refactor their todo app for better organization, create reusable components, and write their first tests. **Note:** This week has TWO assignments: continued todo app work AND a separate testing assignment with failing tests to fix. The breadth of material is substantial but sets students up for real-world React development.



## Explore Session (60 minutes)

**Purpose:** Introduce reusable components, project organization strategies, and testing concepts before students refactor their todo apps.  
**Materials:** Code editor with React project, example components, testing setup.

### Segment 1 – Warm-Up (5 min)
- Ask: "Have you noticed any repeated code patterns in your todo app? Places where you copy-pasted similar code?"
- Quick poll: "How many files is your todo app now? Do you ever struggle to find the right file?"

*Mentor Tip: Many students will have messy projects by now. Normalize that refactoring is a natural part of development—first make it work, then make it clean.*



### Segment 2 – I Do: Why Reusable Components? (10 min)

**Show the problem:**

```jsx
// TodoForm.jsx - has label + input
<label htmlFor="todo-input">Todo</label>
<input 
  type="text" 
  id="todo-input"
  value={title}
  onChange={(e) => setTitle(e.target.value)}
/>

// TodoListItem.jsx - also has label + input (for editing)
<label htmlFor="edit-input">Edit</label>
<input
  type="text"
  id="edit-input"
  value={editTitle}
  onChange={(e) => setEditTitle(e.target.value)}
/>
```

**What's wrong with this?**
- Code duplication
- If you need to update styling, must change in multiple places
- Inconsistent behavior if you forget to update everywhere
- More code to maintain

**Show the solution:**

```jsx
// TextInput.jsx - reusable component
function TextInput({ label, id, value, onChange }) {
  return (
    <>
      <label htmlFor={id}>{label}</label>
      <input
        type="text"
        id={id}
        value={value}
        onChange={onChange}
      />
    </>
  );
}

// Use it anywhere:
<TextInput 
  label="Todo"
  id="todo-input"
  value={title}
  onChange={(e) => setTitle(e.target.value)}
/>
```

**Benefits:**
- Write once, use everywhere
- Update in one place, changes everywhere
- Consistent behavior and styling
- Easier to test

**CFU Questions:**
- "What makes a component 'reusable'?"
- "When should you extract code into a reusable component?"
- "What's the downside of making EVERYTHING a separate component?"

*Mentor Tip: Balance is key. Don't extract too early, but don't wait until duplication is everywhere.*



### Segment 3 – We Do: Creating a Reusable Dialog Component (15 min)

**Build a flexible Dialog component together:**

```jsx
// Dialog.jsx - version 1
function Dialog() {
  return (
    <div className="dialog">
      <div className="heading">
        <p>INFO dialog</p>
      </div>
      <div className="content">
        This is some message
      </div>
      <button>Okay</button>
    </div>
  );
}
```

**Make it flexible with props:**

```jsx
// Dialog.jsx - version 2: add kind prop
const colors = {
  error: '#f6bed7',
  info: '#bec7f6',
  success: '#bef6c5',
  warning: '#f6eebc',
};

function Dialog({ kind = 'info' }) {
  return (
    <div className="dialog">
      <div 
        className="heading" 
        style={{ backgroundColor: colors[kind] }}
      >
        <p>{kind.toUpperCase()} dialog</p>
      </div>
      <div className="content">
        This is some message
      </div>
      <button>Okay</button>
    </div>
  );
}

// Use it:
<Dialog kind="success" />
<Dialog kind="error" />
```

**Make it even more flexible with children:**

```jsx
// Dialog.jsx - version 3: add children
function Dialog({ kind = 'info', children }) {
  return (
    <div className="dialog">
      <div 
        className="heading"
        style={{ backgroundColor: colors[kind] }}
      >
        <p>{kind.toUpperCase()} dialog</p>
      </div>
      <div className="content">
        {children}
      </div>
      <button>Okay</button>
    </div>
  );
}

// Now you can put anything inside:
<Dialog kind="success">
  <h2>Success!</h2>
  <p>Your todo was saved.</p>
</Dialog>
```

**CFU Questions:**
- "What does the `children` prop give us?"
- "Why use default props like `kind = 'info'`?"
- "How would you add a custom onClose handler?"

*Mentor Tip: The `children` prop is powerful but can be confusing. Emphasize it's just "whatever you put between the opening and closing tags."*



### Segment 4 – I Do: Project Organization (10 min)

**Show the problem - flat file structure:**

```
src/
  App.jsx
  TodoForm.jsx
  TodoList.jsx
  TodoListItem.jsx
  Header.jsx
  Footer.jsx
  Button.jsx
  TextInput.jsx
  ... (gets messy fast!)
```

**Show a better structure:**

```
src/
  features/
    TodoList/
      TodoList.jsx
      TodoListItem.jsx
    TodoForm/
      TodoForm.jsx
  shared/
    TextInput.jsx
    Button.jsx
  layout/
    Header.jsx
    Footer.jsx
  App.jsx
```

**Explain the grouping logic:**

**Features:** Components that belong to a specific feature
- TodoList and TodoListItem work together for one feature
- TodoForm is its own feature
- If only used in one feature, group together

**Shared:** Components used across multiple features
- TextInput used in both TodoForm and TodoListItem editing
- Button used everywhere
- If used by 2+ features, it's shared

**Layout:** Structural components
- Header, Footer, Sidebar
- Organize the page structure
- Usually on every page

**CFU Questions:**
- "Where would you put a Modal component used everywhere?"
- "If TodoForm uses a special button only it needs, where does it go?"
- "Why organize at all? Why not leave everything flat?"

*Mentor Tip: There's no "perfect" structure. Emphasize consistency and logical grouping over perfect categories.*



### Segment 5 – I Do: Introduction to Testing (15 min)

**Explain why we test:**

"Tests are like having a safety net. They catch bugs before users do."

**Show a simple test:**

```jsx
// App.test.jsx
import { render, screen } from '@testing-library/react';
import { expect, it } from 'vitest';
import App from './App';

it('renders the app without crashing', () => {
  render(<App />);
  expect(screen.getByRole('main')).toBeInTheDocument();
});
```

**Break down the parts:**

1. **Import testing tools:** `render`, `screen`, `expect`, `it`
2. **Write a test case:** `it('description', () => { ... })`
3. **Render the component:** `render(<App />)`
4. **Find elements:** `screen.getByRole('main')`
5. **Make assertions:** `expect(...).toBeInTheDocument()`

**Three types of tests:**

**Unit tests:** Test one component in isolation
- Fast, focused
- "Does this button work?"
- Most common type

**Integration tests:** Test multiple components together
- More realistic
- "Does the form submit and update the list?"

**End-to-end (E2E) tests:** Test entire user flows
- Slow, complex
- "Can a user add, edit, and complete a todo?"
- We won't cover these

**CFU Questions:**
- "What's the difference between unit and integration tests?"
- "Why test at all? Can't we just manually check?"
- "What does `screen.getByRole('main')` do?"

*Mentor Tip: Testing can feel abstract. Emphasize it's like having a checklist that runs automatically every time you change code.*



### Segment 6 – We Do: Write a Test Together (5 min)

**Test a simple component:**

```jsx
// Button.jsx
function Button({ onClick, children }) {
  return <button onClick={onClick}>{children}</button>;
}

// Button.test.jsx
import { render, screen } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import { expect, it } from 'vitest';
import Button from './Button';

it('renders button text', () => {
  render(<Button>Click me</Button>);
  expect(screen.getByRole('button')).toHaveTextContent('Click me');
});

it('calls onClick when clicked', async () => {
  const handleClick = vi.fn();
  const user = userEvent.setup();
  
  render(<Button onClick={handleClick}>Click</Button>);
  await user.click(screen.getByRole('button'));
  
  expect(handleClick).toHaveBeenCalled();
});
```

**Explain the new parts:**
- `vi.fn()` creates a "spy" function that tracks calls
- `userEvent.setup()` simulates user interactions
- `await user.click()` simulates clicking
- `toHaveBeenCalled()` checks if function was called

**CFU Questions:**
- "Why do we need `vi.fn()` for the onClick handler?"
- "What does `await` mean here?"
- "How is this better than manually clicking during development?"



### Segment 7 – Wrap-Up (5 min)

**Key takeaways:**
- **Reusable components** = Write once, use everywhere
- **Project organization** = Group by feature and purpose
- **Testing** = Automatic checking that code works
- **Two assignments** this week!

**Assignment preview:**
- Assignment 1: Refactor todo app (organize files, create reusable TextInput, add edit feature)
- Assignment 2: Fix failing tests in provided repo

**Important notes:**
- Start with Assignment 1 (refactoring) first
- Assignment 2 is shorter but requires understanding tests
- Don't change test files—only fix the components!

*Mentor Tip: Emphasize this is a heavy week. Encourage students to start early and ask questions.*



## Apply Session (60 minutes)

**Purpose:** Guide students through the refactoring assignment and introduce the testing assignment.  
**Materials:** Todo app codebases, testing repo, screen sharing for debugging.

### Segment 1 – Assignment Overview (5 min)

**Walk through both assignments:**

**Assignment 1: Todo App Refactoring**
- Part 1: Reorganize files into features/shared structure
- Part 2: Create reusable TextInput component
- Part 3: Add edit functionality to todos

**Assignment 2: Fix Failing Tests**
- Clone provided repo
- Run tests (they'll fail)
- Fix components to make tests pass
- Don't change test files!

**Time management tip:** Assignment 1 is longer—spend most time there.



### Segment 2 – Walkthrough Part 1: File Organization (10 min)

**Live demo of reorganizing:**

```
Before:
src/
  App.jsx
  TodoForm.jsx
  TodoList.jsx
  TodoListItem.jsx

After:
src/
  features/
    TodoList/
      TodoList.jsx
      TodoListItem.jsx
    TodoForm.jsx
  App.jsx
```

**Steps to demonstrate:**

1. Create new folders
```bash
mkdir -p src/features/TodoList
mkdir -p src/shared
```

2. Move files
```bash
mv src/TodoList.jsx src/features/TodoList/
mv src/TodoListItem.jsx src/features/TodoList/
mv src/TodoForm.jsx src/features/
```

3. Fix imports in each moved file
```jsx
// TodoList.jsx - update relative imports
import TodoListItem from './TodoListItem';  // Now in same folder!

// App.jsx - update imports
import TodoList from './features/TodoList/TodoList';
import TodoForm from './features/TodoForm';
```

**Common gotchas:**
- VS Code sometimes auto-updates imports, sometimes doesn't
- Check the dev server for import errors
- Remember to update imports IN the moved files too

**CFU Questions:**
- "Why do imports change when we move files?"
- "How do you know if your imports are correct?"
- "What's `./` vs `../` in import paths?"

*Mentor Tip: Import errors are the #1 issue here. Teach students to read the error messages—they show exactly which file is trying to import what.*



### Segment 3 – Walkthrough Part 2: TextInput Component (15 min)

**Create the reusable component:**

```jsx
// src/shared/TextInput.jsx
function TextInput({ 
  elementId, 
  label, 
  value, 
  onChange, 
  inputRef 
}) {
  return (
    <>
      <label htmlFor={elementId}>{label}</label>
      <input
        type="text"
        id={elementId}
        value={value}
        onChange={onChange}
        ref={inputRef}
      />
    </>
  );
}

export default TextInput;
```

**Refactor TodoForm to use it:**

```jsx
// Before:
<label htmlFor="todo-input">Todo</label>
<input
  type="text"
  id="todo-input"
  ref={inputRef}
  value={workingTodoTitle}
  onChange={(e) => setWorkingTodoTitle(e.target.value)}
/>

// After:
import TextInput from '../shared/TextInput';

<TextInput
  elementId="todo-input"
  label="Todo"
  inputRef={inputRef}
  value={workingTodoTitle}
  onChange={(e) => setWorkingTodoTitle(e.target.value)}
/>
```

**Key points:**
- Props names don't have to match original attribute names
- We combined `htmlFor` and `id` into `elementId`
- `ref` prop renamed to `inputRef` to avoid confusion
- The `onChange` handler stays the same

**CFU Questions:**
- "Why did we rename `ref` to `inputRef`?"
- "Could we add a `placeholder` prop? How?"
- "What if we wanted this to work for password inputs too?"

*Mentor Tip: Students often struggle with ref forwarding. For now, just passing it as a regular prop is fine.*



### Segment 4 – Walkthrough Part 3: Edit Functionality (20 min)

**This is the most complex part. Break it down:**

**Step 1: Toggle between display and edit**

```jsx
// TodoListItem.jsx
const [isEditing, setIsEditing] = useState(false);

return (
  <li>
    {isEditing ? (
      <TextInput value={todo.title} />
    ) : (
      <span onClick={() => setIsEditing(true)}>
        {todo.title}
      </span>
    )}
  </li>
);
```

**Step 2: Add local state for editing**

```jsx
const [workingTitle, setWorkingTitle] = useState(todo.title);

<TextInput
  value={workingTitle}
  onChange={(e) => setWorkingTitle(e.target.value)}
/>
```

**Step 3: Add Cancel button**

```jsx
function handleCancel() {
  setWorkingTitle(todo.title);  // Reset to original
  setIsEditing(false);
}

<button type="button" onClick={handleCancel}>
  Cancel
</button>
```

**Step 4: Create updateTodo handler in App**

```jsx
// App.jsx
function updateTodo(editedTodo) {
  const updatedTodos = todoList.map((todo) => {
    if (todo.id === editedTodo.id) {
      return { ...editedTodo };
    }
    return todo;
  });
  setTodoList(updatedTodos);
}

// Pass it down
<TodoList todoList={todoList} onUpdateTodo={updateTodo} />
```

**Step 5: Connect update handler in TodoListItem**

```jsx
// TodoListItem.jsx
function handleUpdate(e) {
  e.preventDefault();
  if (!isEditing) return;
  
  onUpdateTodo({ ...todo, title: workingTitle });
  setIsEditing(false);
}

<button onClick={handleUpdate}>Update</button>
<form onSubmit={handleUpdate}>
  {/* ... */}
</form>
```

**This is a lot! Break for questions frequently.**

**CFU Questions:**
- "Why do we need `workingTitle` separate from `todo.title`?"
- "What happens if user edits but clicks Cancel?"
- "Why do we pass the handler down through TodoList instead of directly to TodoListItem?"

*Mentor Tip: Prop drilling can be frustrating. Acknowledge it's annoying but necessary for now. (Context API comes later!)*



### Segment 5 – Introduction to Testing Assignment (10 min)

**Show the testing repo structure:**

```
multicalc-tests/
  src/
    components/
      Calculator.jsx
      Display.jsx
      Form.jsx
    tests/
      Calculator.test.jsx
      Display.test.jsx
      Form.test.jsx
```

**Demo running tests:**

```bash
npm install
npm test
```

**Show failing test output:**

Students will see something like:
```
❌ Calculator › displays result
❌ Display › shows the correct value
❌ Form › disables button when empty
```

**Explain the approach:**

1. **Read the test** - Understand what it's checking
2. **Look at the component** - Find what's wrong
3. **Fix the component** - Make minimal changes
4. **Run tests again** - Check if it passes
5. **Repeat** for next test

**Show one example:**

```jsx
// Test says: "button should have text 'Calculate'"
it('renders calculate button', () => {
  render(<Form />);
  expect(screen.getByRole('button')).toHaveTextContent('Calculate');
});

// Component currently has:
<button>Submit</button>

// Fix:
<button>Calculate</button>
```

**Important rules:**
- Don't change test files
- Only fix components
- Read error messages carefully
- Tests tell you exactly what's expected

**CFU Questions:**
- "Why can't we change the test files?"
- "How do you know what to fix?"
- "What if you can't understand the test?"

*Mentor Tip: Reading tests is a skill. Encourage students to read them like specifications—they describe exactly what the code should do.*



### Segment 6 – Wrap-Up (5 min)

**Homework plan:**

**Priority 1:** Assignment 1 (todo app refactoring)
- Start with file organization (easier)
- Then create TextInput component
- Then add edit functionality (hardest)
- Test each part before moving on!

**Priority 2:** Assignment 2 (fix tests)
- Clone repo and run tests
- Fix one test at a time
- Use tests as your guide
- Stretch goal: Write your own test for Form

**Tips for success:**
- Commit after each working step
- Test in browser constantly
- Read error messages carefully
- Ask questions early—don't struggle for hours

**Submission checklist:**
- [ ] Todo app PR with all features working
- [ ] Testing repo PR with all tests passing
- [ ] Both PRs submitted in form

*Mentor Tip: This is a heavy week. Normalize that it's okay to not finish everything. Partial credit is better than giving up!*



## Key Teaching Points

### Reusable Components
- **Props make components flexible** - Same structure, different content
- **Children prop** - Powerful for wrapping content
- **Default props** - Make components easier to use
- **Balance** - Don't extract too early, but don't duplicate forever

### Project Organization
- **Features folder** - Components that belong together
- **Shared folder** - Components used everywhere
- **Layout folder** - Structural components
- **Consistency matters** more than perfection

### Refactoring
- **Small steps** - Refactor one thing at a time
- **Test constantly** - Make sure nothing breaks
- **Commit often** - Create safe rollback points
- **Imports are tricky** - Pay attention to relative paths

### Testing
- **Three types** - Unit, integration, E2E
- **Unit tests are fast** - Test one thing in isolation
- **Tests are specs** - They describe what code should do
- **Red-green-refactor** - Write test (red), make it pass (green), improve (refactor)



## Common Student Struggles

1. **Import path confusion** after moving files
   - `./` = same folder
   - `../` = up one folder
   - Count how many folders up, that's how many `../`

2. **Ref forwarding** in reusable components
   - For now, just pass as regular prop
   - React.forwardRef is advanced (not needed yet)

3. **Prop drilling** feels repetitive
   - Acknowledge it's annoying
   - This is why Context/Redux exist (later!)
   - For now, it's the React way

4. **Test error messages** are cryptic
   - Teach students to read carefully
   - "Expected X but got Y" tells you everything
   - Look at line numbers

5. **Edit functionality** has many moving parts
   - Local state vs prop state
   - When to update which
   - Form submission vs button click

6. **Two assignments** feels overwhelming
   - Validate their feelings
   - Prioritize todo app
   - Testing assignment is shorter
   - Partial completion is okay



## Questions to Assess Understanding

**Reusable Components:**
- "When should you extract code into a reusable component?"
- "What's the difference between props and children?"
- "How do you make a component flexible but still useful?"

**Project Organization:**
- "Where would you put a LoadingSpinner used in 3 different features?"
- "Why organize files at all?"
- "What's wrong with putting everything in one folder?"

**Refactoring:**
- "Why refactor instead of just adding features?"
- "When is the best time to refactor?"
- "How do you know your refactor didn't break anything?"

**Testing:**
- "What's the difference between unit and integration tests?"
- "Why write tests instead of manually checking?"
- "What does `vi.fn()` do and why do we need it?"

