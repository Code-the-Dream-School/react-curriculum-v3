# Week 10: Data Fetching & UI Updates – Group Mentor Guide

Welcome to Week 10 of the React course! This week, students are learning:

- Data fetching with `fetch()` and async/await
- Using `useEffect` for API calls
- CRUD operations (Create, Read, Update, Delete)
- Pessimistic vs optimistic UI update strategies
- Managing loading states and error handling
- Integrating with Airtable API

Students are working on updating their todo app to sync with Airtable.

## Warm-Up (5–10 minutes)

Choose one:

**Relationship-Building**  
- What's an app or website that feels really fast and responsive? What do you think makes it feel that way?
- Have you ever had an app lose your work or data? How did that feel?

**Check for Understanding (from last week)**  
- What does `useEffect` do?
- When does a `useEffect` run?
- What's the purpose of the dependency array?

## Explore vs. Apply – Session Formats

**Explore Sessions** → Demonstrate fetch requests, walk through async/await, explain pessimistic vs optimistic strategies  
**Apply Sessions** → Debug API calls, troubleshoot Airtable integration, help with error handling

## Sample Timing for 1-Hour Session

| Time      | Activity                                 |
|-----------|------------------------------------------|
| 0:00–0:10 | Warm-up + review last week               |
| 0:10–0:30 | Explore: fetch, async/await, strategies  |
| 0:30–0:50 | Apply: Airtable debugging + assignment   |
| 0:50–1:00 | Wrap-up + final questions                |

## Check for Understanding (Ask 2–3)

- What's the difference between `async/await` and `.then().catch()`?
- Why can't we use `async` directly in `useEffect`?
- What's a pessimistic UI update strategy?
- What's an optimistic UI update strategy?
- When would you use one strategy over the other?
- What are CRUD operations?

## Explore Prompts

Use these to demonstrate key concepts live:

- Let's make a simple fetch request together – what happens at each step?
- How do we handle errors when fetching data?
- What's the difference between updating the UI immediately vs waiting for the server?

*Mini-Demo Ideas:*  

```javascript
// Basic fetch with async/await
const fetchData = async () => {
  try {
    const response = await fetch('https://api.example.com/data');
    if (!response.ok) {
      throw new Error(response.status);
    }
    const data = await response.json();
    console.log(data);
  } catch (error) {
    console.error('Error:', error.message);
  }
};

// Fetch in useEffect (with IIFE pattern)
useEffect(() => {
  (async () => {
    try {
      const response = await fetch(url);
      const data = await response.json();
      setData(data);
    } catch (error) {
      setError(error.message);
    }
  })();
}, []);

// POST request
const addItem = async (newItem) => {
  const options = {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(newItem)
  };
  
  try {
    const response = await fetch(url, options);
    const data = await response.json();
    setItems([...items, data]);
  } catch (error) {
    console.error(error);
  }
};
```

---

## Apply Prompts (Live Coding & Troubleshooting)

### Assignment Hotspots
* **Environment variables:** Forgetting to prefix with `VITE_` or not restarting dev server
* **Airtable response shape:** Not understanding the `records` array and `fields` object structure
* **Missing `isCompleted`:** Not handling Airtable's omission of false/empty values
* **IIFE pattern:** Confusion about why we need `(async () => { ... })()` inside `useEffect`
* **Error handling:** Not checking `!response.ok` before parsing JSON
* **Optimistic updates:** Forgetting to save the original state before updating
* **CORS errors:** Can happen with incorrect Airtable setup

### Try This Live

**Let's walk through fetching from Airtable together:**

```javascript
// Setting up environment variables
const url = `https://api.airtable.com/v0/${import.meta.env.VITE_BASE_ID}/${import.meta.env.VITE_TABLE_NAME}`;
const token = `Bearer ${import.meta.env.VITE_PAT}`;

// Understanding Airtable's response structure
const response = {
  records: [
    {
      id: "rec123",
      fields: {
        title: "Buy groceries",
        isCompleted: true
      }
    }
    // Note: if isCompleted is false, it won't appear!
  ]
};

// Transforming Airtable data
const todos = response.records.map(record => ({
  id: record.id,
  ...record.fields,
  isCompleted: record.fields.isCompleted || false
}));
```

Ask:
* Why do we need to transform the data?
* What happens if `isCompleted` is missing from the response?
* Where does the `id` come from vs the other fields?

**Let's compare pessimistic vs optimistic updates:**

```javascript
// PESSIMISTIC: Wait for server confirmation
const addTodoPessimistic = async (newTodo) => {
  setIsLoading(true);
  try {
    const response = await fetch(url, options);
    const data = await response.json();
    // Only update UI after server responds
    setTodos([...todos, data]);
  } catch (error) {
    setError(error.message);
  } finally {
    setIsLoading(false);
  }
};

// OPTIMISTIC: Update UI immediately
const addTodoOptimistic = async (newTodo) => {
  const originalTodos = [...todos];
  // Update UI right away!
  setTodos([...todos, newTodo]);
  
  try {
    await fetch(url, options);
    // If successful, nothing else to do
  } catch (error) {
    // If failed, roll back the change
    setTodos(originalTodos);
    setError(error.message);
  }
};
```

Ask:
* Which approach feels faster to the user?
* Which approach guarantees data accuracy?
* When would you choose each strategy?

## Key Concepts to Emphasize

### The IIFE Pattern in useEffect
```javascript
useEffect(() => {
  // Why we need this pattern:
  // 1. useEffect can't be async directly
  // 2. But fetch needs await
  // 3. Solution: create and call an async function inside
  
  (async () => {
    // Your fetch code here
  })();
}, []);
```

### Handling the StrictMode Double-Render
```javascript
useEffect(() => {
  let isRan = false;
  
  (async () => {
    const data = await fetchData();
    
    if (!isRan) {
      setData(data);
    }
  })();
  
  return () => {
    isRan = true; // Cleanup sets this to true
  };
}, []);
```

### Airtable Quirks to Remember
- False/empty values don't appear in the response
- Data is nested in `records` array with `fields` objects
- `id` is at the top level, not in `fields`
- Need Bearer token in Authorization header

## Engagement Strategies (for quiet groups)

* **Live Network Tab:** Open browser DevTools together and watch the actual HTTP requests
* **Break It Together:** "Let's intentionally break the API call and see what error we get"
* **Strategy Debate:** "When should we use pessimistic vs optimistic? Let's vote!"
* **Error Scavenger Hunt:** "Everyone introduce one bug, then swap code and find it"

## Common Error Messages and Solutions

**"Cannot read property 'map' of undefined"**
- Data hasn't loaded yet. Initialize state with empty array: `useState([])`

**"CORS error"**
- Check Airtable token permissions and base access
- Make sure token is in Authorization header

**"401 Unauthorized"**
- Token is missing or incorrect
- Check that `VITE_PAT` is set correctly in `.env.local`

**"Environment variable is undefined"**
- Not prefixed with `VITE_`
- Dev server not restarted after changing `.env.local`

**"setState called on unmounted component"**
- Missing cleanup function in `useEffect`
- Use the `isRan` pattern shown above

## Optional Challenges

- Add a loading spinner instead of just text
- Implement a retry mechanism for failed requests
- Add toast notifications for success/error messages
- Create a custom hook `useFetch` to reuse fetch logic
- Add request debouncing for frequent updates

## Debugging Checklist for Students

When fetch isn't working:
1. Check browser Network tab – is the request being sent?
2. Check response status – is it 200 or an error code?
3. Check response body – what data is actually coming back?
4. Check state updates – are you calling the setter functions?
5. Check console – are there any error messages?

## Mentor To-Do
- [ ] Run a session using this guide  
- [ ] Help students set up Airtable properly
- [ ] Debug API integration issues together
- [ ] Discuss real-world use cases for each strategy
- [ ] Submit your [Mentor Session Report](https://airtable.com/appoSRJMlXH9KvE6w/shrp0jjRtoMyTXRzh)
