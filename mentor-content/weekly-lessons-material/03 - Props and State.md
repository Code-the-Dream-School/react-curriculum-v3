# Week 3: Props, Common Component Props, and State — Group Mentor Guide

Welcome to Week 3 of the React course! This week, students are learning:

- How `state` allows components to update and respond dynamically
- How `props` let parent components pass data to children
- How to use common built-in props (`className`, `style`, `onClick`, `children`, etc.)

Students are working with the `App.jsx`, `ProductList.jsx`, `ProductCard.jsx`, and `TodoListItem.jsx` files.

## 🧊 Warm-Up (5–10 minutes)

Choose one:

**👋 Relationship-Building**  
- What's one thing you've collected or been obsessed with before (hats, Pokémon, houseplants)?  
- What's your favorite thing you've ever bought online?

**💡 Check for Understanding (from last week)**  
- What’s a component in React?
- How do you create a new component file?
- What is `JSX`?

## 🧭 Explore vs. Apply — Session Formats

**Explore Sessions** → Talk through `useState`, props, and component structure. Live-code examples of passing props and updating state.  
**Apply Sessions** → Help students refactor their app, debug prop/state issues, or build small custom components together.

## ⏱️ Sample Timing for 1-Hour Session

| Time      | Activity                            |
|-----------|-------------------------------------|
| 0:00–0:10 | Warm-up + quick review              |
| 0:10–0:30 | Explore: code walkthrough of state/props |
| 0:30–0:50 | Apply: refactor into components or debug code |
| 0:50–1:00 | Wrap-up + final questions           |

## ❓ Check for Understanding (Ask 2–3)

- What’s the difference between `props` and `state`?
- How do we update a piece of state?
- Why do we pass a `key` when rendering lists?
- What happens when state changes in a component?

## 🧑‍🏫 Explore Prompts

Use these to help students understand how props and state work:

- Let’s live-code a small `useState` example — what happens when we click a button to update it?
- What would happen if we updated a regular variable, instead of using `useState`?
- Let’s look at how `props` help us break down a large component into smaller ones.

🧑‍💻 *Mini-Demo Ideas:*  

    // Simple useState example
    const [count, setCount] = useState(0);
    <button onClick={() => setCount(count + 1)}>{count}</button>

    // Props with destructuring
    function ProductCard({ baseName, baseDescription }) {
      return (
        <li>
          <h2>{baseName}</h2>
          <p>{baseDescription}</p>
        </li>
      );
    }

## 🛠️ Apply Prompts (Live Coding & Troubleshooting)

### 🔧 Assignment Hotspots
- Forgetting to use the `useState` hook correctly (e.g. not destructuring the returned array)
- Using props like `props.baseName` after destructuring — should just use `baseName`
- Missing `key` prop when mapping over a list
- Forgetting to pass `inventory` into `ProductList` as a prop
- Accidentally mutating state instead of using the setter function

### ✅ Try This Live

**Walk through promoting an item using `children`:**

    <ProductList inventory={inventory}>
      <ProductCard
        baseName="Limited Edition Tee"
        baseDescription="Glows in the dark and includes a handwritten note from the dev team."
      />
    </ProductList>

Ask:
* What does `{children}` mean in the `ProductList` return statement?
* What happens if we put the promoted item *after* the `.map()` call?

## 💬 Engagement Strategies (for quiet groups)

* “Who can explain the difference between `props` and `state` to a brand-new learner?”  
* “Try removing a prop — what breaks? Why?”
* “Let’s intentionally break something and debug it together.”

## 💡 Optional Challenges

- Add a new component called `ProductPrice` that displays the item’s price in bold.
- Create a `FeaturedProduct` component that uses `children` and custom styling.
- Modify `ProductList` so that it filters only items that are `inStock === "TRUE"`

✅ Mentor To-Do  
- [ ] Run a session using this guide  
- [ ] Let students debug, explore, or build on their code  
- [ ] Submit your [Mentor Session Report](https://airtable.com/appoSRJMlXH9KvE6w/shrp0jjRtoMyTXRzh)
