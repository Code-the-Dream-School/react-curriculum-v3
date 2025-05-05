## Discussion Topics

### Pagination

Pagination is a design pattern commonly used to divide a large dataset into smaller, more manageable chunks or pages. This is something that can be accomplished on the client on a relatively small data set, like CTD-Swag's product list. More often though, pagination is done on the API since response containing all results would be too large to be practical. A search for "gamer mouse" from Amazon.com reveals over 70,000 results!

![Amazon search results for "gamer mouse"](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/lessons/assets/week-12/mouse-search.png)

Examples of where pagination in websites and web applications include:

- **Search results** - web search, store catalog search, news website articles search
- **Product listings** - web store department landing pages
- **Article-based websites** - blogs, professional journals, news
- **Image Galleries** - social media platforms, art/photography forums, art galleries

In the UI, pagination typically includes previous and next buttons, and page numbers to allow the user to systematically navigate the date. The underlying state revolves around whether it is working with a local dataset or whether an API is responding with already paginated data.

#### Paginating a Local Dataset

This is the simpler of the two to work with since we implement the pagination ourselves. Similar to the filtering that we did in week 8, paginating involves passing along a part of an original list. Rather than filtering it though, we slice portions out of the list based on a pagination size and an offset value. Pagination size is the number of items or amount of text that would be in a typical chunk or page. The offset value is the number of items to skip. Depending on the API, pagination works with individual items skipped or with a page index. Going with 20 items per page we end up with the following chunks:

- Page 1: items 1-20, offset = 0 (don't forget that arrays start at index 0 so it's more accurately items 0-19)
- Page 2: items 21-40, offset = 20
- Page 3: items 41-60, offset = 40
- ...and so on...

To continue on with the discussion, we'll build out a phonebook, Here is a link to the [completed demo app](https://github.com/Code-the-Dream-School/phonebook). An SPA phonebook can have hundreds of entries stored locally, especially when we can take advantage of localStorage in the browser. For such an app, it's beneficial to implement data pagination with pagination controls in the UI. Here's an animated screen capture of the final interface in action:

![Phonebook demo](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/lessons/assets/week-12/phonebook-pagination.gif)

This app can be broken down visually into the components that we'll be using:

![Phonebook interface components](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/lessons/assets/week-12/phonebook-interface.png)

- **App** - wraps the UI and manages state for the complete contacts lists
- **Phonebook**
  - child of App
  - manages pagination logic and state
  - wraps children
- **Page**
  - child of Phonebook
  - displays a table view of currently rendered contacts
- **Pagination**
  - child of Phonebook
  - buttons for paging forward and backwards through the contacts list
  - displays page numbers

Most of the logic is performed in the Phonebook component but first we need to get the simulated data into the application. We import a JSON file containing all of the contacts and then add it to state in App. From there, we pass as props into the Phonebook:

```jsx
{/*extract from App.jsx*/}
{/*...code*/}
import Phonebook from './Phonebook';

const sortedContacts = importedContacts.sort((a, b) =>
    a.lastName.localeCompare(b.lastName)
);

function App() {
    const [contacts] = useState(sortedContacts);
    return (
        <>
            <main>
                <h1>CTD Phone Book</h1>
                <Phonebook contacts={contacts} />
            </main>
        </>
    );
}

export default App;
```

Inside of Phonebook resides the logic that is needed to paginate the contacts list. It also has to manage the UI state so that the user can page forwards and backwards through the contacts. We start with a `currentPage` state value of 1 and set `entriesPerPage` to 20. These two values can be used to calculate the indices of the first and last element we want to include on the page.

```jsx
{/*extract from Phonebook.jsx*/}
{/*...code*/}
const Phonebook = ({ contacts }) => {
const [currentPage, setCurrentPage] = useState(1);
const entriesPerPage = 20;

//index of the last item in the current pagination
const indexOfLastEntry = currentPage * entriesPerPage;

//index of the first item in the current pagination
const indexOfFirstEntry = indexOfLastEntry - entriesPerPage;

//uses previous values to slice out the appropriate subset to pass to `Page`
const currentEntries = contacts.slice(indexOfFirstEntry, indexOfLastEntry);

return (
    <div>
        <Page contacts={currentEntries} />
        <Pagination/>
    </div>
);

export default Phonebook;
```

The first page is displayed and experiment with pagination in the React Dev tools by updating `currentPage`:

![Toggling page state value](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/lessons/assets/week-12/toggle-state.gif)

We now create the handlers that page forward and back through the list. When the page number updates, React initiates the render cycle and the update then cascades through the pagination logic to provide `Page` with the current subset of contacts.

```jsx
{/*extract from Phonebook.jsx*/}
{/*...code*/}

const totalPages = Math.ceil(contacts.length / entriesPerPage);

const handleNextPage = () => {
    //only page forward if it is not the last page
    if (currentPage < totalPages) {
        setCurrentPage(currentPage + 1);
    }
};

const handlePreviousPage = () => {
    //only page backwards if it is not the first page
    if (currentPage > 1) {
        setCurrentPage(currentPage - 1);
    }
};
{/*code continues...*/}
```

We now pass these handlers, along with the current page number and a page count, into the Pagination component.

```jsx
{/*extract from */}
{/*...code*/}
return (
    <div>
        <Page contacts={currentEntries} />
        <Pagination
        currentPage={currentPage}
        totalPages={totalPages}
        handleNextPage={handleNextPage}
        handlePreviousPage={handlePreviousPage}
        />
    </div>
);
{/*code continues...*/}
```

In the `Pagination` component, we insert the `currentPage` and `totalPages` between two buttons. We can then wire the buttons to increment and decrement the current page number.

```jsx
{/*extract from Pagination.jsx*/}
import styles from './Pagination.module.css';

const Pagination = ({
    currentPage,
    totalPages,
    handlePreviousPage,
    handleNextPage,
}) => {
    return (
        <nav className={styles.pagination}>
            <button onClick={handlePreviousPage} className={styles.buttons}>
                Previous
            </button>
            <span>
                Page {currentPage} of {totalPages}
            </span>
            <button onClick={handleNextPage} className={styles.buttons}>
                Next
            </button>
        </nav>
    );
};

export default Pagination;
```

We have a functioning app but we are not quite done yet...

![Back button still enabled](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/lessons/assets/week-12/enabled-buttons.png)

While the logic is place to prevent the Previous or Next button from working after reaching the first or last page of the list, the user needs an indication that the button has been disabled. We can disable the buttons by comparing the current page number with the values of the first and last page.

```jsx
{/*extract from Pagination.jsx*/}
{/*...code*/}
<button
    disabled={currentPage === 1}
    onClick={handlePreviousPage}
    className={styles.buttons}
>
    Previous
</button>
<span>
Page {currentPage} of {totalPages}
</span>
<button
    disabled={currentPage === totalPages}
    onClick={handleNextPage}
    className={styles.buttons}
>
Next
</button>
{/*code continues...*/}
```

!["Previous" button disabled](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/lessons/assets/week-12/previous-disabled.png)
!["Next" button disabled](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/lessons/assets/week-12/next-disabled.png)

#### Working with a Paginated Data Response

> [!note]
> Normally, we would also implement response caching alongside server-based pagination. For the sake of clarity, we will leave it out of this discussion.

When pagination is done on an API, our main concern becomes using a fetch request with query params. We'll use [json-server](https://github.com/typicode/json-server#readme) as a locally API that serves the JSON file full of contacts on localhost port 3000. To receive a paginated response from `json-server` we have to add the following url params to a fetch:

- `_page=<<some number>>`
- `_per_page=<<some number>>`

Here is an example fetch from the browser:

![Example fetch from json-server](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/lessons/assets/week-12/browser-fetch.png)

The server also includes some handy values in the response body that we can incorporate into our navigation logic:

- `prev` - previous page number - `null` if already on page 1
- `next` - next page number - `null` of on last page
- `pages` - total page count

Our first step is to determine changes we need to make to our state. First off, since we are no longer working with a full contact list, we can safely remove any state from the App component:

```jsx
{/*extract from App.jsx*/}
{/*...code*/}
function App() {
    return (
        <>
            <main>
                <h1>CTD Phone Book</h1>
                <Phonebook />
            </main>
        </>
    );
}
export default App;
```

We can then remove the props from the Phonebook component then look at its pagination logic to determine what can be safely removed and what needs replaced with the API response data. We can get an idea of the state we absolutely need by looking to Phonebook's return statement. The Page and Pagination components, which we do not need to modify take the following:

```jsx
{/*extract from Phonebook.jsx*/}
{/*...code*/}
return (
    <div>
        <Page contacts={currentEntries} />
        <Pagination
        currentPage={currentPage}
        totalPages={totalPages}
        handleNextPage={handleNextPage}
        handlePreviousPage={handlePreviousPage}
        />
    </div>
);
{/*code continues...*/}
```

- `currentEntries`: we replace this with a new `contacts` state we update with API responses
- `currentPage`: re-usable as-is since it only changes via the buttons' event handlers
- `totalPages`: update to set the value from API responses
- `handleNextPage` and `handlePreviousPage`: re-usable as-is since they only change `currentPage`

```jsx
{/*extract from Phonebook.jsx*/}
{/*...code*/}
import { useState } from 'react';
import Page from './Page';
import Pagination from './Pagination';
import { useEffect } from 'react';

const testEntry = [{firstName: "Mister", lastName: "Developer", email:  "a@b.com", phone: "867-5309"}]
const entriesPerPage = 20;

const Phonebook = () => {
    //testEntry keeps the list visible
    //`contacts` replaces `currentEntries`
    const [contacts, setContacts] = useState(testEntry);
    const [currentPage, setCurrentPage] = useState(1);
    //will be updated with API response
    const [totalPages, setTotalPages] = useState(0);
    
    const handleNextPage = () => {
        if (currentPage < totalPages) {
            setCurrentPage(currentPage + 1);
        }
    };

    const handlePreviousPage = () => {
        if (currentPage > 1) {
            setCurrentPage(currentPage - 1);
        }
    };

    return (
        <div>
            <Page contacts={contacts} />
            <Pagination
            currentPage={currentPage}
            totalPages={totalPages}
            handleNextPage={handleNextPage}
            handlePreviousPage={handlePreviousPage}
            />
        </div>
    );
};

export default Phonebook;
{/*code continues...*/}
```

![Single entry in phonebook](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/lessons/assets/week-12/single-entry.png)

We are now ready to add in our fetch using `useEffect`. `currentPage` gets added to the dependency array. This way, any time a button is clicked, the event helpers will update `currentPage`, which will, in turn, send off another fetch with updated url params. The `useEffect` no sends off a fetch when the component first renders and then every time `currentPage` is updated afterwards.

```jsx
{/*extract from Phonebook.jsx*/}
{/*...code*/}
const baseUrl = 'http://localhost:3000/contacts';
const entriesPerPage = 20;
useEffect(() => {
    const fetchContacts = async () => {
        const url = `${baseUrl}?_page=${currentPage}&_per_page=${entriesPerPage}`
        try {
            const response = await fetch(url);
            const data = await response.json();
            setContacts(data.data);
            setTotalPages(data.pages);
        } catch (error) {
            console.error('Error fetching contacts:', error);
        }
    };
    fetchContacts();
  }, [currentPage]);
{/*code continues...*/}
```

Our Phonebook app now behaves like it did previously but now we are fetching the page data from our API:

![Paginated data loaded from API](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/lessons/assets/week-12/server-pagination-demo.gif)

### React Router and Routing

#### Navigating SPAs vs Traditional Websites

React SPAs differ from traditional websites in terms of routing and navigation. Each page of a traditional website has its own HTML file and associated URL. Navigating between pages involves making a request to the server to fetch a new HTML document. This process is managed by the browser, which updates the URL and maintains a [history stack](https://developer.mozilla.org/en-US/docs/Web/API/History_API) that users to use the back and forward buttons to navigate through their browsing history.

In contrast, a React SPA loads a single HTML file and and uses a script to dynamically update the content on the page. This means that navigation in the app does not involve fetching new HTML documents from the server. Instead, React components are rendered or hidden based on the application's state. This approach can lead to faster and fluid UI but it also means that the URL does not change when navigating around the app and the browser's history remains unchanged.

A major disadvantage of not leveraging browser history in a React SPA is the inability to use the back and forward buttons effectively. In a traditional website, these buttons allow users to navigate through their previous actions, providing a familiar and intuitive way to move through the site. A React SPA does not offer this functionality by default which can lead to a frustrating user experience. A user may lose their place in the application or be unable to return to a previous state easily. Even worse, if they hit the browser's back button, they may exit the application entirely!

![Pressing back button leaves app](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/lessons/assets/week-12/back-button-accident.gif)

Another issue with unchanging URLs in a React SPA is the difficulty in sharing specific views or states within the application. Look again at the URL above - it never changes while navigating the app. In a traditional website, the URL uniquely identifies the content being viewed, making it easy to bookmark or share links to specific pages. Without routing, a React SPA cannot provide this capability, as the URL remains the same regardless of the content being displayed. This can make it challenging for users to share specific parts of the application with others or to return to a particular view later.

To address these issues, developers often use routing libraries like React Router, which allow them to manage navigation and update the URL dynamically. These libraries enable React SPAs to mimic the behavior of traditional websites by updating the browser's history stack and changing the URL based on the application's state. This not only improves the user experience by enabling the back and forward buttons but also allows for better sharing and bookmarking of specific views within the application.

#### React Router

> [!note]
> There are two ways to use React Router. We will implement it as a library, not a full framework.

[React Router](https://reactrouter.com/home) is a popular library that emulates page navigation inside of React apps. After putting into place users will be able to take advantage of browser history to navigate the app and even share urls to specific products with other people. Before proceeding any further, we have to go over some of the components and hooks that the makes up the library.

##### Components

- **BrowserRouter** - provides context to the application that manages browser history and in-app navigation logic
  - internally, BrowserRouter is a higher-order component
    - sub-components NavigationContext.Provider and LocationContext.Provider provide the actual context
    - we don't work with these directly but they appear in Reacts dev tools
  - accepts children props
- **Routes** - performs pattern-matching on URL segments to determine the appropriate route to use
  - accepts 1 or more `Route` component as children props
- **Route** - defines a path segment and maps it to a corresponding UI element to be rendered
  - takes a `path` props that represents the segment
  - takes an `element` props that is the component (or regular html element) that gets rendered
- **Link** - replacement for navigation links that allows a user to navigate between defined routes without refreshing the page
- **NavLink** - similar to Link but also tracks if belongs to the current active route - automatically adds "active" to its class list

##### Hooks

- **useParams** - gives the developer an object that contains params from any dynamic segments in a URL
- **useSearchParams** - gives the developer access to url params that come after the `?` in a URL
- **useNavigate** - returns a function that allows the developer to programmatically navigate to a target route
  - takes a string that represents the destination of the link. Usually one of the route segments
  - or takes a number that navigates back or forward in the browser's history stack
- **useLocation** - provides the developer an object from the location context that contains information about the current location

##### Advanced Features not being Covered

- route nesting
- layout routes
- and some other hooks such as `useLocation`, `useSearchParams`, `useLocation`

#### Implement Routing using React Router

We will take a deeper look at each of the components and hooks as we build routing into the store. We will be adding in the following routes:

- **"/"** - index or default route
- **"/checkout"**
- **"/account"**
- **"/products/:id"**

##### Installation and Setup

To get started, we install React Router.

`npm install react-router`

From here, we wrap App with BrowserRouter to provide App the navigation and location context. This is very similar to the context provider we used last week.

```jsx
{/*extract from main.jsx*/}
{/*...code*/}
createRoot(document.getElementById('root')).render(
  <StrictMode>
    <BrowserRouter>
      <App />
    </BrowserRouter>
  </StrictMode>
);
```

Multi-page sites tend to have one or more repeating element that appears on every page. This gives the site a cohesive look and feel from one page to another. Reused navigation elements also help make navigation easier. For these reasons, CTD-Swag's header and footer should be shown on each page.

![header and footer highlighted](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/lessons/assets/week-12/header-footer-highlight.png)

The elements in the App component is already laid out in a way that will allow us to share the header and footer across pages. The App returns the the following element structure, minus all the props:

```jsx
{/*extract from App.jsx*/}
{/*...code*/}
return (
    <>
      {isDialogOpen && (
        <Dialog/>
      )}
      <Header/>
      <main>
        {isAuthDialogOpen && (
          <AuthDialog/>
        )}
        <ProductViewForm/>
        <ProductList/>
        {cartState.isCartOpen && (
          <Cart/>
        )}
      </main>
      <Footer />
    </>
  );
{/*code continues...*/}
```

The page can be split into three primary elements: **Header**, **main**, and **Footer**. The Dialog component is only a conditionally rendered component that doesn't affect the flow of the html document so we [can safely ignore](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_flow_layout/In_flow_and_out_of_flow) it. Main is the ideal element to make into the unique part of the pages since it's centrally located and takes up the majority of the browser's existing viewport.

![main element on page highlighted](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/lessons/assets/week-12/main-highlighted.png)

##### Refactor App to add Routes

We need to refactor the contents of `main` to make way for Route and Routes components. AuthDialog and Cart components are also out of flow and conditionally rendered so is the easiest item to move. We can move AuthDialog's condition block just above `main` and Cart's below `main`. Some styles need updated to slightly reposition these but other than that, we can continue the refactor.

```jsx
<Header/>
{isAuthDialogOpen && (
    <AuthDialog/>
)}
<main> 
    <ProductViewForm/>
    <ProductList/>
</main>
{cartState.isCartOpen && (
    <Cart/>
)}
```

This leaves ProductViewForm and ProductList. ProductViewForm works tightly with ProductList so we'll extract them together into another component, Shop.

```jsx
{/*extract from Shop.jsx*/}
import ProductListFilter from '../../features/ProductListFilter/ProductListFilter';
import ProductList from '../../features/ProductList/ProductList';

function Shop({
  filteredProducts,
  handleAddItemToCart,
  setSortBy,
  setIsSortAscending,
  sortBy,
  isSortAscending,
  searchTerm,
  setSearchTerm,
}) {
  return (
    <>
      <ProductListFilter
        setSortBy={setSortBy}
        setIsSortAscending={setIsSortAscending}
        sortBy={sortBy}
        isSortAscending={isSortAscending}
        searchTerm={searchTerm}
        setSearchTerm={setSearchTerm}
      />
      <ProductList
        products={filteredProducts}
        handleAddItemToCart={handleAddItemToCart}
      />
    </>
  );
}

export default Shop;
```

> [!note]
> Cleaning up all those props would be a good use case for `useReducer`! We'll add that to the task backlog.

##### Adding Routes and Route Elements

Since this component is specific to React-Router, we can create a new directory in our project for "pages" to keep Shop and other route-associated components organized neatly.

![pages directory in project](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/lessons/assets/week-12/pages-dir.png)

Our next step is to add in a Routes component and our Route components. We'll use Shop as the first Route's element and [stub](https://en.wikipedia.org/wiki/Method_stub) in the remaining routes using draft components that we'll flesh out for each route later.

![route components are placed into route](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/lessons/assets/week-12/routes-stubbed.png)

The Route using Shop also takes a `path="/"` which indicates this is an index page. We could optionally replace `path` props with an `index` attribute: `<Route index element={<Shop ...props />} />` which behaves the same way. Be careful to provide only one of the two items as they cannot be combined.

With the Routes in place and the index route completed, we can next work on the component for `"/account"`. This component shows the user's information and we'll also move the Logout button to this page to clean up the heading a little.

```jsx
{/*Account.jsx*/}
import { Link } from 'react-router';

function Account({ user, handleLogOut }) {
    return (
        <div className="account">
            <h2>Your Account</h2>
            <div className="accountDetails">
                <p>First Name: {user.firstName}</p>
                <p htmlFor="lastName">Last Name: {user.lastName}</p>
                <p htmlFor="email">Email: {user.email}</p>
            </div>
            <div className="buttonGroup">
                <Link className="linkButton" to={'/'}>
                Go back
                </Link>
                <button onClick={handleLogOut}>Log Out</button>
            </div>
        </div>
    );
}

export default Account;
```

We include the Link component to add navigation back to the home page. Link is designed to work with the contexts React-Router provides and allows for navigation without having the page refreshes normally associated with `<a></a>` anchors. We continue to use `<a></a>` when linking to external resources. We can then go back to App and add the `user` and `handleLogOut` props.

We then add the `user` and `handleLogOut` props into the Account instance passed to the `Route`.

```jsx
{/*extract from App.jsx*/}
{/*...code*/}
{user.id && (
    <Route
        path="/account"
        element={<Account user={user} handleLogOut={handleLogOut} />}
    />
)}
{/*code continues...*/}
```

 Finally, update the user salutation in the header to link to the account page.

```jsx
{/*extract from Header.jsx*/}
{/*...code*/}
<>
    <Link to="/account" className="linkButton">
        <span>Hi, {user.firstName}</span>
    </Link>
</>
{/*code continues...*/}
```

Now that we have 2 routes in place, let's view the site in the browser:

![navigate to account route](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/lessons/assets/week-12/nav-to-account.gif)

When a user navigates, the URL updates and they can use their back button.

Since the account route is only available to logged in users, we can't navigate to it directly by typing it in the URL. Let's implement the `"/checkout"` route to so we can see how React-Router responds to the contents of the URL a user adds to the URL bar. We can base this component off the cart component since they both work with `cart` state. We will also exclude the form that updates the item quantities since the user can work with the form already in cart.

```jsx
{/*extract from Checkout.jsx*/}
import { Link } from 'react-router';
import CheckOutItem from './CheckoutItem';

function Checkout({ cart }) {
  function getTotal() {
    return cart
      .reduce((acc, item) => acc + item.price * item.quantity, 0)
      .toFixed(2);
  }
    return (
        <>
            <h2>Checkout Page</h2>
            <ul>
                {cart.map((item) => {
                return <CheckOutItem key={item.id} item={item} />;
                })}
            </ul>
            <h2>Total: {getTotal()}</h2>
            {cart.length < 1 ? <p>cart is empty</p> : null}
            <div className="buttonGroup">
                <Link className="linkButton" to="/">
                Go Back
                </Link>
                {/* permanently disabled since this is just a demo*/}
                <button disabled>Checkout</button> 
            </div>
        </>
    );
}

export default Checkout;
```

Like the Cart, we will also iterate over the items in the cart to render preview cards. Unfortunately, their html elements will differ enough from CartCard so it cannot be reused.

```jsx
{/*extract from CheckoutItem.jsx*/}
import placeholder from '../../assets/placeholder.png';
import styles from './CheckoutItem.module.css';

function CheckOutItem({ item }) {
    return (
        <li className={styles.cartItem}>
            <img src={placeholder} alt="" />
            <div>
                <h2>{item.baseName}</h2>
                {item.variantName !== 'Default' ? <p>{item.variantName}</p> : null}
            </div>
            <div className={styles.subtotal}>
                <p>Count: {item.quantity}</p>
                <p>Subtotal: ${(item.price * item.quantity).toFixed(2) || '0.00'}</p>
            </div>
        </li>
    );
}

export default CheckOutItem;
```

Finally, we provide Checkout state it needs to render the checkout page.

```jsx
{/*extract from */}
<Route
    path="/checkout"
    element={<Checkout cart={cartState.cart} />}
/>
{/*...code*/}
{/*code continues...*/}
```

We now have a component state that React can render based on the URL segments:

![refreshing page with checkout route in address bar](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/lessons/assets/week-12/refresh-checkout.gif)

We can tell that the application has lost its state when the page refreshes because the user is not logged in and the cart is empty. What remains, though, shows that it's properly rendering an empty Checkout component.

##### Using URL params

Our next route, `"/products/:id"` includes a dynamic segment. `:id` represents the value that is parsed out of a segment. This is usually an identifier that can be used to look up some item kept in a state object. It can also represent some other data that the developer can incorporate into their app but the value extracted from the dynamic segment will be a string. We have to convert a value meant to be a number ourselves.

Now that we have an understanding of dynamic segments, we can begin developing the ProductDetails component. This page provides a user with more information about a product than could fit into a smaller card. It also includes each one of the variants some of the products have. For example, the bucket hat comes in two choices: black and peach.

![showing bucket hat variants](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/lessons/assets/week-12/bucket-hats.png)

We will also continue with the Page and Cards pattern so that we can neatly display all the product variants for each product. This time around, we'll focus only on the ProductDetails component since its card behaves the same as the other cards.

Our first step is to add a dynamic segment to the original `"/products"` route so it looks like `"/products/:id"`. We then import the `useParams` hook into ProductDetails. Calling `useParams` gives us an object containing all the parsed parsed values of dynamic segments found in the URL. For example, this URL:

```text
https://ctd-swag.com/admins/233/manage/stock/items/43989/
```

when matched up against a Route with a path written as

```jsx
{/*isolated example*/}
<Route
    to="/admins/:adminId/manage/inventory/item/:stockId"
    element={<InventoryItem />}
/>
```

would result in an a returned object that resembles:

```js
const paramsObj = {
    adminId: "233",
    stockId: "43989"
}
```

Don't forget to coerce the text into numbers if they are meant to be used in any calculations. JavaScript does this for us but it's a convenience that is falling out of favor because of TypeScript's growing popularity and a few bizarre coercion rules that can cause bugs.

Returning to CTD-Swag's products route, we can see that if a browser's URL bar contains the following: `http://localhost:5173/products/3e41a7cb-63d3-4e4f-a576-a27cc9953e3a`. Our Route's path props takes `"/products/:id"` . We can tell that when using `useParams` will result in an object that looks something like:

```js
cont paramsOjb = {
    id: "3e41a7cb-63d3-4e4f-a576-a27cc9953e3a"
} 
```

Let's put this into place in ProductDetails. We import it into our scaffolded ProductDetails component and then call it just below the component's function definition opening:

```jsx
{/*extract from ProductDetails*/}
{/*...code*/}
import { useParams } from 'react-router';
import styles from './ProductDetails.module.css';

const ProductDetail = () => {
  const { id } = useParams();

  return (
    <div>
        <h2>Product Details</h2>
        <p>Product id: {id}</p>
    </div>
  );
};

export default ProductDetail;
{/*code continues...*/}
```

![product id shown on page](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/lessons/assets/week-12/product-id.png)

Since we are working with local state and our ProductDetail component has to be able to render *any* single product, not just a product that is passed into it this `id` come in handy. We can use it to look up the product in a `products` props that we add to the component's props.

```jsx
{/*extract from ProductDetails*/}
{/*...code*/}
const ProductDetail = ({ products, handleAddItemToCart }) => {
    const { id } = useParams();
    
    const [product] = products.filter((product) => {
        return product.id === id;
    });
{/*code continues...*/}
```

Inside the component's return body, we can conditionally render the `product` if the lookup is successful:

```jsx
{/*extract from */}
{/*...code*/}
{/*code continues...*/}
```

![bucket hats variations shown](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/lessons/assets/week-12/bucket-hats2.png)

...or if the lookup fails:

![message of nothing found on product page](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/lessons/assets/week-12/nothing-found.png)

We've dealt with a missing product, but what if the user employs a url that doesn't match any Route's path? Since the Router fails to match the route, it fails to render anything from inside the `<Route>` component instance. We also get a series of console warnings that are useful during development but are unhelpful to the user.

![no routes component rendered](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/lessons/assets/week-12/blank-route-component.png)

To address this, we employ a special "catchall" route. It uses a path value of asterisk `"*"` which is a common [wildcard character](https://en.wikipedia.org/wiki/Wildcard_character).

```jsx
{/*extract from App.jsx*/}
{/*props removed for clarity*/}
{/*...code*/}
<Routes>
    <Route
        path="/"
        element={<Shop/>}
    />
    <Route
        path="/checkout"
        element={<Checkout/>}
    />
    {user.id && (
        <Route
            path="/account"
            element={<Account/>}
        />
    )}
    <Route
        path="/products/:id"
        element={
            <ProductDetail/>
        }
    />
    <Route path="*" element={<NotFound} />
</Routes>
{/*code continues...*/}
```

Here's the associated NotFound component:

```jsx
{/*extract from 404.jsx*/}
{/*...code*/}
function NotFound() {
  return <h2>404: Not Found</h2>;
}
export default NotFound;
{/*code continues...*/}
```

This improves the interface so the the user understands that something went wrong.

![404 message displayed](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/lessons/assets/week-12/404.png)

All of the routes are in place so now we need to improve a few details about navigation. Inside cart, we want to put a link that looks like a button that will navigate to the `"/checkout"` route.

![cart showing a checkout button](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/lessons/assets/week-12/cart-with-checkout.png)

We can use the hook, `useNavigate` for this task which we import into Cart. When we call it, it gives us a function that we can use to programmatically navigate the application. In our case, we create a `handleCheckout` event handler that calls `navigate` with `'/checkout'`:

```jsx
{/*extract from Cart.jsx*/}
{/*...code*/}
import { useNavigate } from 'react-router';
import CartItem from './CartItem';

function Cart({
    cart,
    handleCloseCart,
    handleUpdateCart,
    cartError,
    isCartSyncing,
}) {
    const navigate = useNavigate();

//...code continues...

    function handleCheckout(e) {
        e.preventDefault();
        handleCloseCart();
        navigate('/checkout');
    }
{/*code continues...*/}

```

Alternatively, we can `useNavigate` with a delta value which is a number that represents how far forward or back in the history stack to navigate. For example: `useNavigate(-1)` would be the equivalent of hitting the browser's back button. Negative numbers, especially -1 are the most common values seen but positive values would work if the back only if the current page is several layers below the top of the history stack.

Routing is a powerful tool that enhances the user experience by providing intuitive and efficient navigation within your applications. We've only touched on the highlights of the library and it contains other hooks, components, and methods. Even with just with what has been discussed this week, you have a robust and flexible set of tools that simplifies the implementation of routing in React.
