---
title: Week-08
dateModified: 2024-12-30
dateCreated: 2024-08-20
tags: [react]
parent: "[[Intro to React V3]]"
week: 8
topics: [searching, sorting]
content: lesson plan
---

# Week-08

## Introduction

### Topics Covered

- Sorting
- Searching

### Lesson Objectives

By the end of this lesson, we will:

#### Objective 1: Sorting

- Implement sort functionality to organize lists that are presented to the user

#### Objective 2: Filtering

- Implement a filtering feature to return desired items to a user

## Discussion Topics

We've been introduced to many React's features since the start of the course. We've also worked with network requests and syncing state with an API. In this lesson, we'll implement sorting and filtering display data. We'll turn to basic JavaScript to allow a user to customize how information is being presented to them. This is an opportunity to emphasize the importance of JavaScript knowledge and its place in developing React SPAs.

### Sorting

#### Default Sorting

After some (pretend) success, CTD has decided to add more items to its eCommerce store. What is readily apparent is the store's page is much longer. Either of these can frustrate a user who is browsing for specific items. Perhaps they are skimming the store or are browsing for a specific kind of product. We can we help the user find items faster by by sorting the cards in the product list by their name in alphabetic order.

![[202412_1059AM-Firefox Developer Edition.gif]]

Our data takes form of an array but we shouldn't rely on the API (yet) to sort it for the interface. We can can implement an array sorting function but need decide which array method is most appropriate for a React codebase. Array's `.sort()` or `.toSorted()` methods can be employed to sort an array but differ in how the consume data. `.sort` mutates the array in place which we want to avoid. `.toSorted` returns a new updated array which conforms to React' functional approach.

We'll declare this new utility function above the App component.

```js
// extract from App.jsx
//...code
function sortByBaseName(productItems) {
  return productItems.toSorted((a, b) => {
    const baseNameA = a.baseName.toLowerCase();
    const baseNameB = b.baseName.toLowerCase();
    if (baseNameA > baseNameB) {
      return 1;
    }
    if (baseNameA < baseNameB) {
      return -1;
    }
    return 0;
  });
}
const baseUrl = import.meta.env.VITE_API_BASE_URL;
function App() {
//code continues...
```

#### Sort by Field

Of course, CTD Swag's users may want to browse the shop in different ways. For, example, a user has limited finances and want to sort by price. We can implement another sorting feature that can sort by price, ascending or descending. For the user to make a choice, we'll add a form at the top of the page that contains a field selector and an ascending/descending option selector.

First we update `sortByBaseName`, adding `isSortAscending` to the function arguments and update the logic to sort by ascending or descending.

```js
// extract from App.jsx
//...code
function sortByBaseName({ productItems, isSortAscending }) {
  return productItems.toSorted((a, b) => {
    const baseNameA = a.baseName.toLowerCase();
    const baseNameB = b.baseName.toLowerCase();
    if (baseNameA > baseNameB) {
      if (isSortAscending) {
        return 1;
      } else {
        return -1;
      }
    }
    if (baseNameA < baseNameB) {
      if (isSortAscending) {
        return -1;
      } else {
        return 1;
      }
    }
    return 0;
  });
}
//code continues...
```

We then create a sortByPrice utility function that behaves similarly to `sortByBaseName` but works with numerical values instead of strings.

```js
// extract from App.jsx
//...code
function sortByPrice({ productItems, isSortAscending }) {
  return productItems.toSorted((a, b) => {
    if (isSortAscending) {
      return a.price - b.price;
    } else {
      return b.price - a.price;
    }
  });
const baseUrl = import.meta.env.VITE_API_BASE_URL;
function App() {
}
//code continues...
```

We next create state values so we have options that can be passed into the utility functions when they are integrated into our component code. We can anticipate the need for `sortBy` which represents the property being sorted and `isSortAscending` which indicates the sort direction.

```js
// extract from App.jsx
//...code
const [isSortAscending, setIsSortAscending] = useState(true);
const [sortBy, setSortBy] = useState('baseName');
//code continues...
```

With the state values in place, we can then create a form component to allow the user to update the sort options. This form will not be re-used and is referenced in App, so to keep the codebase organized, we create a new features folder to contain the ProductViewForm component: `src/features/ProductViewForm/ProductViewForm.jsx`. The `ProductSupportForm` consists of a form containing two select fields which correspond to the values represented in `sortBy` and `isSortAscending`.

Since `isSortAscending` is a boolean and all event target values are expressed in strings or numbers, we'll need a helper function to convert the value used to update `isSortAscending` back into a boolean. Here is the final `ProductViewForm`:

```js
// extract from ProductViewForm.jsx
function ProductViewForm({
  setSortBy,
  setIsSortAscending,
  sortBy,
  isSortAscending,
}) {
  const handleSortDirectionChange = (e) => {
    const sortDirection = e.target.value;
    if (sortDirection === 'false') {
      setIsSortAscending(false);
    } else {
      setIsSortAscending(true);
    }
  };

  return (
    <form className="filterForm">
      <div className="filterOption">
        <label htmlFor="sortBy">Sort by: </label>
        <select
          name="sortBy"
          id="sortBy"
          value={sortBy}
          onChange={(e) => setSortBy(e.target.value)}
        >
          <option value="baseName">Product Name</option>
          <option value="price">Price</option>
        </select>
      </div>
      <div className="filterOption">
        <label htmlFor="sortDirection">Direction: </label>
        <select
          name="sortDirection"
          id="sortDirection"
          value={isSortAscending}
          onChange={handleSortDirectionChange}
        >
          <option value={true}>Ascending</option>
          <option value={false}>Descending</option>
        </select>
      </div>
    </form>
  );
}

export default ProductViewForm;

```

Finally, we'll create a `useEffect` in `App` that watches for changes on either of the new state values. This `useEffect` ties in our utility functions to sort the product list. Note that if we were to feed the productItems directly into the useEffect, they would become a dependency of that function. We can get around this by using returning a function inside the state update function. The state update function provides the old state value as an argument that we can use to return a new value for the state.

```js
// extract from App.jsx
//...code
useEffect(() => {
	if (sortBy === 'baseName') {
	  setInventory((previous) =>
		sortByBaseName({ productItems: previous, isSortAscending })
	  );
	} else {
	  setInventory((previous) =>
		sortByPrice({ productItems: previous, isSortAscending })
	  );
	}
}, [isSortAscending, sortBy]);
//code continues...
```

These updates results in a product list that the user can sort by the product name or the price:

![[202412_0916PM-Firefox Developer Edition.gif|500]]

### Filtering

We can also provide a filter for the user show only a sub-set of product items that contain the term. Not surprisingly, array's `.filter` method is central to this feature. Let's get started with a basic filter function. We'll focus on searching the `baseName`, `baseDescription`, and the `variantDescription` to determine which to items to show to the user.

```js
const items = [
//contains an offline copy of listItems
]
function filterByQuery({ productItems, searchTerm }) {
  const term = searchTerm.toLowerCase();
  return productItems.filter((item) => {
    if (item.baseName.toLowerCase().includes(term)) {
      return item;
    } else if (item.baseDescription.toLowerCase().includes(term)) {
      return item;
    } else if (item.variantDescription.toLowerCase().includes(term)) {
      return item;
    }
  });
}
const filteredItems = filterByQuery({ productItems: items, searchTerm: "pillow" });
console.log(filteredItems);
```

Running the code results in:

```terminal
#terminal output from running code above:
[
  {
    id: 179,
    baseName: 'Throw Pillow',
    variantName: 'Peach',
    price: 44.99,
    baseDescription: 'Comfortable throw pillow and an excellent conversation starter',
    variantDescription: 'Peach cotton with large pale peach logo',
    image: 'throw-pillow-peach.png',
    inStock: true
  },
  {
    id: 180,
    baseName: 'Throw Pillow',
    variantName: 'Turquoise',
    price: 44.99,
    baseDescription: 'Comfortable throw pillow and an excellent conversation starter',
    variantDescription: 'Turquoise cotton with large dark blue logo',
    image: 'throw-pillow-turquoise.png',
    inStock: true
  },
  {
    id: 185,
    baseName: 'Pillow Case',
    variantName: 'Orange',
    price: 23.99,
    baseDescription: 'Comfortable pillow case and an excellent conversation starter',
    variantDescription: 'Orange cotton with large pale orange logo',
    image: 'pillow-case-orange.png',
    inStock: true
  },
  {
    id: 186,
    baseName: 'Pillow Case',
    variantName: 'Turquoise',
    price: 23.99,
    baseDescription: 'Comfortable pillow case and an excellent conversation starter',
    variantDescription: 'Turquoise cotton with large dark blue logo',
    image: 'pillow-case-turquoise.png',
    inStock: true
  }
]
```

Our filtering function works so now we need to create the rest of the filter feature before we integrate it. For now, we place it with the other utility functions at the top of the App component file. We next need to add filter UI elements to the form we made while implementing sort.

![[202412_1140AM-Brave Browser.png|500]]

At this point, we have to consider how this filter should interact with our global state. We don't want this this function removing anything from the `inventory` so we will have to create an intermediate working state to provide a filteredInventory to the product list. Its associated `setFilteredInventory` state function is called alongside `setInventory` in the `useEffect` that fetches the inventory. We then replace the `inventory` prop with `filteredInventory` in ProductList instance.

```jsx
{/*extract from App.jsx*/}
{/*...code*/}
<ProductList
	  inventory={filteredInventory}{/*no need to change the prop key name*/}
	  handleAddItemToCart={handleAddItemToCart}
></ProductList>
{/*code continues...*/}
```

Whenever a user adds a filter term, we filter `inventory` and provide the returned value to the `setFilterInventory`. We employ another `useEffect` to coordinate the live updating query results. Since we want the filter to run every time `query` changes, we add that to the `useEffect`'s dependency array. ESLint will also provide a warning that `inventory` needs to be added to the dependency array too. Since it remains unchanging after it is set, we can add it to that array. We end up with a `useEffect` that allows ProductList to generate ProductItems on the fly that include any variant that matches the filter.

```js
// extract from App.jsx
//...code
useEffect(() => {
    setFilteredInventory(
	    filterByQuery({ productItems: inventory, searchTerm })
    );
}, [searchTerm, inventory]);
//code continues...
```

Here is the update interface in action:

![[202412_0247PM-Brave Browser.gif|500]]

Sort and search implemented locally are appropriate for applications that work with a limited amount of data. We already have all product data on hand so these approaches great for improving the experience users have with CTD Swag.

Applications that use larger datasets commonly paginate the data returned from an API into digestible chunks. Think of how many products Amazon.com has. It would be impossible to send all of their product information is set to the UI. In these cases, sorting and filtering are handled by the API. Just like the sort and search feature we just implemented, we don't need any other React tools to create an SPA that relies on an API for search and filtering.

## Weekly Assignment Instructions

### Expected App Capabilities

After completing this week's assignment, the app should be able to:

- capability 1
- capability 2
- capability n

### Instructions Part 1

 1. [ ] step 1
 2. [ ] step 2
 3. [ ] step n

### Instructions Part 2

 1. [ ] step 1
 2. [ ] step 2
 3. [ ] step n

### Instructions Part n

 1. [ ] step 1
 2. [ ] step 2
 3. [ ] step n

## References and Further Reading

### Sorting

- [Array.prototype.sort() (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)

### Filter

- [Array.prototype.filter() (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter)
