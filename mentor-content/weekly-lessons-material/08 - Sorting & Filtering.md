# React: Sorting & Filtering — Group Mentor Guide

Welcome to this week's React lesson! This week, students are learning:

- How to implement sorting functionality using `.toSorted()`
- How to use API query parameters for sorting and filtering
- How to build controlled forms for view controls
- How to work with URL encoding and Airtable's query syntax
- How to manage multiple state values that affect data display

Students are working with their Todo app, adding sorting by title/date and search filtering.

## Warm-Up (5–10 minutes)

Choose one:

**Relationship-Building**  
- What's an app feature that makes your life easier? (sorting emails, filtering search results, etc.)
- When shopping online, do you sort by price, rating, or something else?

**Check for Understanding (from last week)**  
- What's the difference between props and state?
- When does a `useEffect` run?
- What does the dependency array in `useEffect` control?

## Explore vs. Apply — Session Formats

**Explore Sessions** → Walk through sorting logic, URL encoding, and API query parameters  
**Apply Sessions** → Debug student implementations, test sorting/filtering together, troubleshoot API calls

## Sample Timing for 1-Hour Session

| Time      | Activity                                    |
|-----------|---------------------------------------------|
| 0:00–0:10 | Warm-up + review useEffect/state            |
| 0:10–0:30 | Explore: sorting arrays & query parameters  |
| 0:30–0:50 | Apply: build/debug forms & API integration  |
| 0:50–1:00 | Wrap-up + final questions                   |

## Check for Understanding (Ask 2–3)

- What's the difference between `.sort()` and `.toSorted()`? Why use `.toSorted()`?
- How do query parameters get added to a URL?
- Why do we need to encode URLs?
- What happens when state in the dependency array changes?
- Why use a utility function for `encodeUrl`?

## Explore Prompts

Use these to demonstrate key concepts live:

- Let's sort an array together — what does `.toSorted()` return?
- How do we build a URL with query parameters? Let's decode one together.
- What happens when we add state to a `useEffect` dependency array?

*Mini-Demo Ideas:*  

```javascript
// Basic sorting comparison
const products = [
  { name: "Zebra", price: 20 },
  { name: "Apple", price: 10 }
];

// .toSorted() returns NEW array (doesn't mutate original)
const sorted = products.toSorted((a, b) => {
  if (a.name > b.name) return 1;
  if (a.name < b.name) return -1;
  return 0;
});

// Number sorting
const byPrice = products.toSorted((a, b) => a.price - b.price);

// URL encoding example
const params = 'sort[0][field]=title&sort[0][direction]=asc';
const encoded = encodeURI(params);
console.log(encoded); // see the %5B encoding
console.log(decodeURI(encoded)); // back to readable
```

---

## Apply Prompts (Live Coding & Troubleshooting)

### Assignment Hotspots
* Forgetting to add new state values to `useEffect` dependency array (ESLint will warn!)
* Not passing all required params to `encodeUrl()` in every fetch call
* Confusing when to use `sort()` vs `.toSorted()` — always use `.toSorted()` in React!
* URL encoding errors — missing template literal syntax or wrong query format
* Form not updating state — missing `onChange` handlers or `value` props
* Accidental form submission causing page refresh — need `preventDefault()`
* Not updating the `createdTime` field in Airtable properly

### Try This Live

**Let's build a simple sorting utility together:**

```javascript
function sortByPrice({ productItems, isSortAscending }) {
  return productItems.toSorted((a, b) => {
    if (isSortAscending) {
      return a.price - b.price;  // ascending
    } else {
      return b.price - a.price;  // descending
    }
  });
}
```

Ask:
* Why return a function to `setInventory` instead of calling it directly?
* What would happen if we used `.sort()` instead?
* How can we make this work for any field, not just price?

## Mentor To-Do
- [ ] Run a session using this guide  
- [ ] Review students' Airtable setup (createdTime field exists and is configured correctly)
- [ ] Check that ESLint warnings are being addressed
- [ ] Submit your [Mentor Session Report](https://airtable.com/appoSRJMlXH9KvE6w/shrp0jjRtoMyTXRzh)
