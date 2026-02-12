### **React v3: React Router, Routing, and Pagination — Group Mentor Guide**

Welcome to Week 14! This week, students are transforming their Single Page Applications (SPAs) to mimic the intuitive navigation of traditional websites using **React Router**. They will also implement **pagination** to handle large datasets effectively.

#### **Warm-Up (5–10 minutes)**
Choose one:
**Relationship-Building (Mindset: Imposter Syndrome)**
*   A 2018 study reported that **58% of tech workers** feel like they don't deserve their success or that others know significantly more than they do. Have you ever felt this "Imposter Syndrome," and if so, how did it manifest for you?
*   How can we, as a group, help each other manage these feelings of self-doubt as we move toward the final weeks of the course?

**Check for Understanding (Routing & Pagination Basics)**
*   In a React SPA, what happens to the **browser history** and **URL** by default when we change the view? (Answer: Nothing; the URL stays the same and the back button may exit the app entirely).
*   What is the difference between **pagination size** and an **offset value**? (Answer: Size is the number of items per page; offset is the number of items to skip).

#### **Explore vs. Apply — Session Formats**
*   **Explore Sessions** → Demonstrate the **History API**, the different components of **React Router** (Routes, Link, NavLink), and how **URL parameters** work.
*   **Apply Sessions** → Help students refactor their `App.jsx` into a `TodosPage`, configure **active styling** for navigation links, and calculate **pagination logic** using `useSearchParams`.

#### **Sample Timing for 1-Hour Session**
| Time | Activity |
| ------ | ------ |
| 0:00–0:10 | Warm-up + Reviewing **Imposter Syndrome** mindset |
| 0:10–0:30 | Explore: **React Router** components and the **History stack** |
| 0:30–0:50 | Apply: Implementing `useSearchParams` for **Todo List pagination** |
| 0:50–1:00 | Wrap-up: Discussion on **catch-all routes** (404 pages) |

#### **Check for Understanding (Ask 2–3)**
*   Why is **server-side pagination** often preferred over client-side for very large datasets (like 70,000 Amazon results)? (Answer: Sending all data at once is impractical and slow).
*   What is the purpose of the **`BrowserRouter`** component? (Answer: It provides the context that manages browser history and in-app navigation).
*   What is the difference between the **`Link`** and **`NavLink`** components? (Answer: `NavLink` can track if it is currently "active" and apply specific styles).
*   How does **`useNavigate(-1)`** behave? (Answer: It programmatically moves the user back one step in the history stack, like the browser back button).

#### **Explore Prompts**
*   **Routing Logic:** Let's look at the **`Routes`** and **`Route`** components. How does the router decide which element to render based on the URL path?.
*   **Dynamic Segments:** Let's practice with **`useParams`**. If our path is `/products/:id`, and the URL is `/products/123`, what will the `params` object look like?.
*   **Wildcards:** Let's try to enter a fake URL. Why is a **"catch-all" route (`path="*"`)** important for user experience?.

#### **Apply Prompts (Assignment Hotspots)**
*   **Refactoring to `TodosPage`:** Students must move `TodoForm`, `TodoList`, and `TodosViewForm` from `App` to a new page component. Ensure they are destructuring and passing the correct props.
*   **Active Link Styling:** In the **`Header`**, the `className` for `NavLink` should be a function that checks the **`isActive`** property to return "active" or "inactive" styles.
*   **URL Pagination:** Unlike the lesson's local state example, the assignment uses **`useSearchParams`**. Remind students to use **`parseInt`** on the page parameter, as URL params are always strings.
*   **The Loading/Empty Guard:** If students get redirected to the home page on refresh, check their **`useEffect`** logic. It should only navigate if **`totalPages > 0`** to account for the time it takes to fetch data from Airtable.

#### **Optional Challenges**
*   **Programmatic Navigation:** Use **`useNavigate`** to send a user to the "About" page automatically after they add a specific number of todos.
*   **Advanced Pagination:** Add a dropdown to let the user change the **`itemsPerPage`** value dynamically.

#### **Mentor To-Do**
*   [ ] Demonstrate the difference between a page refresh (using `<a>`) and a client-side transition (using `<Link>`).
*   [ ] Help students visualize the **pagination window** (calculating `indexOfFirstTodo`).
*   [ ] Submit your Mentor Session Report.
