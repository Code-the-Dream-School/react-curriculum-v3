### **Group Mentor Guide: Advanced State, useReducer, and useContext**

#### **About This Week**
This week, students transition from managing simple state with `useState` to handling the complexities of growing applications. The lesson introduces the **reducer pattern**—centralizing state logic into a single function to make troubleshooting easier and features more stable. Students also learn to use **`useContext`** to solve the problem of **"prop drilling,"** allowing data to be shared across deeply nested components without explicitly passing props through every level of the tree.

#### **Explore Session (60 minutes)**
**Purpose:** Reinforce the concepts of centralized state management and the "Inversion of Control" before students begin refactoring their code.
**Materials:** Mentor slides, code editor (to demonstrate the Reducer pattern), and chat for participation.

##### **Segment 1 – Warm-Up (5 min)**
*   **Ask:** “Have you ever had a component with so many `useState` hooks that it became hard to read?”.
*   **Quick poll:** “What is **prop drilling**?”.
*   **Mentor Tip:** Remind students that as apps grow, orchestrating numerous state updates becomes a major challenge.

##### **Segment 2 – I Do (10 min)**
**Mentor demonstrates the Reducer Pattern:**
*   **Explain:** The three key elements: **reducer function** (central logic), **action** (data describing the change), and **dispatch** (the function that sends the action).
*   **Demonstrate:** Show a simple reducer with a `switch/case` statement.
*   **CFU Questions:** 
    *   “Why do we use an object with a `type` property for actions instead of just a string?”.
    *   “What happens if the reducer doesn't find a matching case?” (Answer: It should return the current `state`).

##### **Segment 3 – We Do (20 min)**
**Collaborative example: Inversion of Control.**
*   Discuss shifting perspectives from "what event happened" (button click) to "what the user did" (added a product to the cart).
*   Work together to write a dispatch call: `dispatch({ type: 'increment' })`.
*   **CFU Questions:**
    *   “Where should we define our reducer function—inside or outside the component?” (Answer: Outside, often in a separate file).
    *   “How does `useReducer` know what the initial state is?”.

##### **Segment 4 – You Do (20 min)**
**Short Task: Action Identification.**
*   Students examine a list of state update functions (like `setIsLoading` or `setTodoList`) and group them into logical **actions**.
*   **Task:** “Write the `initialState` object for a Todo app that tracks a list, a loading status, and an error message”.

##### **Segment 5 – Wrap-Up (5 min)**
*   Summarize: `useState` is for local, simple state; `useReducer` is for complex, related state logic.
*   **Remind:** Review the documentation on **React Context** before the Apply session.

#### **Apply Session (60 minutes)**
**Purpose:** Support students as they refactor their Todo app to use a centralized reducer and implement `useContext` for shared state.

##### **Segment 1 – Quick Recap (5 min)**
*   **Ask:** “What was the hardest part of moving your logic from the component into the reducer file?”.
*   Address the **ESLint warning** regarding Vite’s fast-refresh and named exports in component files.

##### **Segment 2 – Guided Coding (25 min)**
**Mentor demonstrates refactoring one CRUD operation:**
*   Show how to move the logic for a **Pessimistic UI** update (like `addTodo`) into the reducer.
*   Demonstrate wrapping the application in a **`Context.Provider`** and passing a value.
*   **CFU Questions:**
    *   “Why do we use `...state` (the spread operator) in every return statement of our reducer?”.
    *   “How does `useContext` help us remove props from 'wrapper' components?”.

##### **Segment 3 – Peer or Solo Debugging (20 min)**
*   Troubleshoot common issues: mismatched **action types**, forgetting to export/import the reducer, or failing to pass the **`dispatch`** function through context if needed.
*   Encourage students to use **React Developer Tools** to inspect the `Context.Provider` and reducer state.

##### **Segment 4 – Wrap-Up and Reflection (10 min)**
*   **Discussion Prompt:** “This week's mindset assignment is about **Design and Accessibility**. How can visual cues and layout choices make a site more accessible?”.
*   **Remind:** Assignments (including the design prompts) are due **Feb 10, 2026**.
*   **Mentor Tip:** Celebrate the milestone—refactoring to a reducer is one of the most complex patterns in React.
