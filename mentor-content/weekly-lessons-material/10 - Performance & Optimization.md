### **React v3: Performance & Optimization — Group Mentor Guide**

Welcome to the lesson on **Performance and Optimization** in React! This week, students focus on making their applications more efficient by limiting network requests through **caching, throttling, and debouncing**, as well as mastering the **`useMemo`** and **`useCallback`** hooks.

#### **Warm-Up (5–10 minutes)**
Choose one:
**Relationship-Building (Mindset: Problem Solving)**
*   Have you ever looked at a new coding problem and thought, “I have no idea how to even get started”? How did you break it down into subproblems?
*   Do you have a personal problem-solving process that you find effective in other hobbies or activities?

**Check for Understanding (from Previous Lessons)**
*   Why do we use the `useEffect` hook? (Answer: To handle side effects like data fetching).
*   What happens to functions defined inside a component body when that component re-renders? (Answer: They are redefined, which can cause unnecessary re-renders in child components).

#### **Explore vs. Apply — Session Formats**
*   **Explore Sessions** → Demonstrate how **lookup objects** store cached data and explain the syntactic requirements of `useMemo` and `useCallback`.
*   **Apply Sessions** → Help students implement **debouncing** for search inputs or troubleshoot **missing dependencies** in hook arrays.

#### **Sample Timing for 1-Hour Session**
| Time | Activity |
| ------ | ------ |
| 0:00–0:10 | Warm-up + Review mindset: Problem Solving process |
| 0:10–0:30 | Explore: Caching logic, `useMemo` (values) vs. `useCallback` (functions) |
| 0:30–0:50 | Apply: Live coding a **debounce** with `useEffect` and `setTimeout` |
| 0:50–1:00 | Wrap-up: Discussion on "judicious" use of optimization hooks |

#### **Check for Understanding (Ask 2–3)**
*   What is the benefit of **caching** search results? (Answer: It saves user bandwidth and speeds up repeated searches).
*   How does **throttling** (rate limiting) protect an API? (Answer: It prevents over-taxing the API infrastructure by ensuring a set amount of time elapses between requests).
*   What is the difference between `useMemo` and `useCallback`? (Answer: `useMemo` returns the **results** of a memoized function, while `useCallback` caches the **function definition** itself).
*   Why do we need a **cleanup function** when using `setTimeout` for debouncing? (Answer: To delete the previous timeout each time the user types a new character).

#### **Explore Prompts**
*   **The Lookup Object:** Let’s visualize a JavaScript object used for caching. If a user searches for "chicken," how do we check the `searchCache` before firing a `fetch`?
*   **useMemo Constraints:** Let's look at the requirements for `useMemo`. Why must it be at the top level, and why can the function definition take **no arguments**?
*   **Network Activity:** Let’s open the browser's **Network tab**. How can we use the "Waiting for server response" time to calculate an ideal throttling delay?

#### **Apply Prompts (Assignment Hotspots)**
*   **Functional State Updates:** When using `useCallback`, how does using a **setter function** (e.g., `setNextOffset((prev) => prev + paginationSize)`) help keep our dependency array short?
*   **Debounce Logic:** In `TodosViewForm.jsx`, let's practice setting up a `localQueryString`. Why do we wait **500ms** before calling the main `setQueryString`?
*   **Dependency Warnings:** If **ESLint** warns about missing dependencies in a hook, how do we decide if a value (like `queryString` or `sortField`) belongs in the array?
*   **Over-Optimization:** Discuss why the sources warn that overusing these hooks can lead to **unnecessary complexity** and hinder **readability**.

#### **Optional Challenges**
*   **Throttling Buttons:** Try to disable a "Page Forward" button for exactly 1000ms after a click to prevent rapid-fire API requests.
*   **Refactoring Utilities:** Move a utility function like `encodeUrl` into the component and wrap it in `useCallback`—what happens to the arguments when you do this?

#### **Mentor To-Do**
*   [ ] Demonstrate the "lookup object" caching flow visually.
*   [ ] Guide students through identifying "expensive calculations" that might need `useMemo`.
*   [ ] Submit your Mentor Session Report.
