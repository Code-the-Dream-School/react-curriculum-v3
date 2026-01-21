### **React v3: Performance & Optimization — Group Mentor Guide**

Welcome to this lesson on **React performance and network efficiency**. This week, students are moving beyond basic functionality to focus on optimizing their apps using **memoization**, limiting network requests, and mastering advanced hooks like **`useMemo`** and **`useCallback`**.

#### **Warm-Up (5–10 minutes)**
Choose one:
**Relationship-Building (Mindset: Problem Solving)**
* Have you ever looked at a coding problem and thought, “I have no idea how to even get started”? How did you eventually break it down?
* Can you think of a time when a solution didn't work on the first try but was more satisfying once you finally solved it?

**Check for Understanding (from Previous React Lessons)**
* Why do we use the `useEffect` hook? (Answer: To handle side effects like data fetching).
* What happens to a component's internal functions when it re-renders? (Answer: They are redefined, which can cause child components to re-render unnecessarily).

#### **Explore vs. Apply — Session Formats**
* **Explore Sessions** → Walk through the concept of **memoization** using a JavaScript "lookup object" and explain the syntax differences between `useMemo` and `useCallback`.
* **Apply Sessions** → Practice implementing **throttling** or **debouncing** to limit API requests and debug dependency arrays in hooks.

#### **Sample Timing for 1-Hour Session**
| Time | Activity |
| ------ | ------ |
| 0:00–0:10 | Warm-up + Review mindset: The Problem-Solving Process |
| 0:10–0:30 | Explore: Caching logic, `useMemo` (values) vs. `useCallback` (functions) |
| 0:30–0:50 | Apply: Live coding a **debounce** with `useEffect` and `setTimeout` |
| 0:50–1:00 | Wrap-up: Discussion on when *not* to over-optimize code |

#### **Check for Understanding (Ask 2–3)**
* Why is it important to limit fetch requests to an API? (Answer: To meet API rate limits/quotas and save user bandwidth).
* What is the difference between `useMemo` and `useCallback`? (Answer: `useMemo` returns a memoized **value**, while `useCallback` returns a memoized **function**).
* How does a "lookup object" help with caching search results?.
* What is **debouncing**, and why is it useful for search inputs? (Answer: It prevents processing events until they stop for a set time, reducing unnecessary network requests).

#### **Explore Prompts**
* **The Lookup Object:** Let’s look at a simple JS object used for caching. If we search for "chicken" twice, how can we use this object to avoid a second API call?
* **useMemo Live:** Let's take a complex string, like a `pendingQuery`, and wrap it in `useMemo`. What should go in the dependency array?
* **useCallback Syntax:** Let's convert a standard utility function into a `useCallback`. Why do we need to move it inside the component?

#### **Apply Prompts (Assignment Hotspots)**
* **Missing Dependencies:** ESLint often warns about missing dependencies in `useCallback` or `useMemo`. Let’s practice identifying which reactive values (like `queryString` or `sortField`) need to be added.
* **Debounce Cleanup:** When using `setTimeout` inside `useEffect`, why must we use a **cleanup function** to call `clearTimeout`? (Answer: To delete the previous timeout every time the user types a new character).
* **Infinite Loops:** Students sometimes add a state-setter to a dependency array that then updates that same state. Let's debug how to use the functional update pattern (e.g., `setNextOffset(prev => prev + 1)`) to keep dependency lists short.

#### **Optional Challenges**
* **Throttling vs. Debouncing:** Try to implement a simple throttle (rate limiting) for a "Next Page" button so it can only be clicked once per second.
* **Network Tab Analysis:** Open the browser's Network tab and average out the server response times to determine the ideal delay for a timeout.
* **Over-optimization Discussion:** Discuss why the sources say hooks should be used "judiciously" and how overusing them can hinder code readability.

#### **Mentor To-Do**
* [ ] Demonstrate the difference between `useMemo` and `useCallback` visually.
* [ ] Help students troubleshoot their `useEffect` cleanup logic for debouncing.
* [ ] Submit your Mentor Session Report.
