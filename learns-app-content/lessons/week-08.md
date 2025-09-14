## Discussion Topics

### Sorting

#### Default Sorting

After some (pretend) success, CTD has decided to add more items to its eCommerce store:

![scrolling product list](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/lessons/assets/week-08/long-scroll.gif)

What is readily apparent with the growth is the store's page is much longer and products are out of order. We can improve the user's experience by immediately sorting the products alphabetic order and giving the them other sorting options.

To get started, we'll declare a new utility function above the App component to keep our sorting logic. To respect to React's functional programming approach we choose methods that return data instead of update some existing value. When sorting an array, we have two methods available to help us sort `.toSorted()` and`.sort()`. We choose `.toSorted()` because it returns a new array.

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

To finish up this first iteration of sorting, we then add it to the `useEffect` that processes our initial fetch request. `sortByBaseName` is then called with the response to alphabetize the array we pass `setInventory`.

```js
// extract from App.jsx
//...code
useEffect(() => {
  (async () => {
    try {
      const resp = await fetch(`${baseUrl}/products`);
      if (!resp.ok) {
        throw new Error(resp.status);
      }
      const products = await resp.json();
      const sortedProducts = sortByBaseName(products);
      setInventory([...sortedProducts]);
    } catch (error) {
      console.error(error);
    }
  })();
}, []);
//code continues...
```

CTD-Swag's product list now shows in alphabetic order. That is a helpful start but I think we can do better.

#### Sort by Field

CTD Swag's users may also want to browse the shop in different ways. For, example, a user may want to sort by price. To allow the user to browse by price, we'll create a form at the top of the page. For now, we just construct the basic form with static content to visualize the purpose of the form.

![sort options](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/lessons/assets/week-08/sort-options.png)

We then update the utility function `sortByBaseName` by adding `isSortAscending` to the arguments and updating the sorting logic.

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

We create another utility function, `sortByPrice`, that behaves similar `sortByBaseName` but is much shorter since we are working with numbers:

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

We we now employ 2 `useState` hooks so that we can make that form into a controlled form.

```js
// extract from App.jsx
//...code
const [isSortAscending, setIsSortAscending] = useState(true);
const [sortBy, setSortBy] = useState('baseName');
//code continues...
```

This form isn't going to be re-used but to keep the codebase organized we refactor the form out of App and into a `ProductViewForm` located at `src/features/ProductViewForm/ProductViewForm.jsx`.

In the new `ProductViewComponent` we update the placeholder values to work with state, converting the form to a controlled form. Since HTML stores values as either strings or numbers, we need a helper function to convert the value used to update `isSortAscending` back into a boolean. Here is the final `ProductViewForm`:

```js
// extract from ProductViewForm.jsx
function ProductViewForm({
  setSortBy,
  setIsSortAscending,
  sortBy,
  isSortAscending,
}) {
  //helper to convert text back into a boolean
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
          onChange={(e) => setSortBy(e.target.value)}>
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
          onChange={handleSortDirectionChange}>
          <option value={true}>Ascending</option>
          <option value={false}>Descending</option>
        </select>
      </div>
    </form>
  );
}

export default ProductViewForm;
```

Finally, we'll create a `useEffect` in `App` that watches for changes on either of the new state values. This `useEffect` ties in our utility functions to sort the product list. Note that if we were to work with `inventory` directly into the useEffect, that state value would become a dependency of that `useEffect`. We can avoid this by passing in a function to update state rather than setting it directly. The reason why we are able to do this is that the state update function provides the old state value as an argument that we can use to return a new value for the state. This behavior can be made more apparent through by naming the argument `previous`, similar to how we would use `item` in a map going over `listItems`: eg `listItems.map((item)=> {…`

```js
// extract from App.jsx
//...code
useEffect(() => {
  if (sortBy === 'baseName') {
    setInventory((previous) =>
      sortByBaseName({ productItems: previous, isSortAscending }),
    );
  } else {
    setInventory((previous) =>
      sortByPrice({ productItems: previous, isSortAscending }),
    );
  }
}, [isSortAscending, sortBy]);
//code continues...
```

These updates results in a product list that the user can sort by the product name or the price:

![changing sort options](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/lessons/assets/week-08/sort-options.gif)

### Filtering

Filters are useful for showing only a a sub-set of product items that contain the term. This can shorten the list of items the user needs to look. Not surprisingly, array's `.filter` method is central to this feature. Let's get started with a basic filter function. We'll allow the user to filter on a match in `baseName`, `baseDescription`, and the `variantDescription` properties on each item.

```js
const items = [
  //contains an offline copy of listItems
];
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
const filteredItems = filterByQuery({
  productItems: items,
  searchTerm: 'pillow',
});
console.log(filteredItems);
```

This initial `filterQuery` when ran on its own results in the following output:

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

Our now utility function works so now we need to create the rest of the filter feature before we integrate it. For now, we place it with the other utility functions at the top of the App component file. We next need to add filter UI elements to the form we made while implementing sort.

![filter field added](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/lessons/assets/week-08/filter-field.png)

At this point, we have to consider how this filter should interact with our global state. We don't want this this function removing anything from the `inventory` so we will have to create an intermediate working state to provide a `filteredInventory` to the product list. Its associated `setFilteredInventory` state function is called alongside `setInventory` in the `useEffect` that fetches the inventory. We then replace the `inventory` prop with `filteredInventory` in ProductList instance.

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
  setFilteredInventory(filterByQuery({ productItems: inventory, searchTerm }));
}, [searchTerm, inventory]);
//code continues...
```

Here is the update interface in action:

![filter live-updates search for terms](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/lessons/assets/week-08/filter-pillow.gif)

Sort and search implemented locally are appropriate for applications that work with a limited amount of data. We already have all product data on hand so these approaches great for improving the experience users have with CTD Swag.

Applications that use larger datasets commonly paginate the data returned from an API into digestible chunks. Think of how many products Amazon.com has. It would be impossible to send all of their product information is set to the UI. In these cases, sorting and filtering are handled by the API. Just like the sort and search feature we just implemented, we don't need any other React tools to create an SPA that relies on an API for search and filtering.
