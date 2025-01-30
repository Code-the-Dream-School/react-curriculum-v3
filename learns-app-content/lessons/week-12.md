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

To continue on with the discussion, we'll build out a phonebook. An SPA phonebook can have hundreds of entries stored locally, especially when we can take advantage of localStorage in the browser. For such an app, it's beneficial to implement data pagination with pagination controls in the UI. Here's an animated screen capture of the final interface in action:

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

Our Phonebook app now behaves like it did previously but now we are fetching the pages from our API:

![Paginated data loaded from API](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/lessons/assets/week-12/server-pagination-demo.gif)

### Routing

- SPAs
  - don't have page history but can behave similar to traditional website
    - pages doesn't reload like a traditional website when we navigate
    - fetch happens in the background + React re-renders interface
  - can have full screens which users navigate
    - think of all the aspects of a social media website
      - we'll look at Facebook since they developed and maintain React
        - news feed
        - marketplace
        - settings and privacy
    - tends to feel like a traditional website
      - would be better for users if they can continue to use familiar tools like back and forward buttons
- starting to work with data that spans "pages"
- browser session history
  - keeps track of pages visited in a browser window
  - allows us to navigate back and forward quickly
  - each tab or window opened has its own session history
- can emulate some of this with the browser's History API
  - properties:
    - **length**: number of elements in session history
    - **scrollRestoration**: auto | manual - allows web apps to explicitly set scroll restoration
    - **state**: null until pushState or replaceState are used - can contain anything that can be serialized
    - NOTE: for security reasons, the API does not give us access to urls
      - This would allow a site to see where all we have been previous to that site. Major privacy concern!
  - methods:
    - **back**: navigates back 1 page
    - **forward**: navigates forward 1 page
    - **go**: takes a number and navigates to a page in history relative to current one open
    - **pushState**: adds an entry to the session history stack
      - takes
        - **state object**
          - mandatory
          - anything serializable
        - **unused**
          - parameter is not used but still need it
          - programmers sometimes make mistakes that are hard to get out of software!!
          - pass it an empty string
        - **url**
          - optional
          - url for new history entry
            - relative urls resolved relative to the current url
            - absolute urls must be of same origin as current URL
          - if unspecified, automatically set to current URL
    - **replaceState**: replaces current history entry
      - takes state object, unused, and url - same as defined in pushState
- complex problem, especially to take full advantage of state object

### React Router

- 3rd party tool that emulates page navigation in SPAs
- makes use of the History API and history.state
- take some time to familiarize yourself with [React Router Terminology](https://reactrouter.com/en/main/start/concepts#definitions)
- a lot to learn about this library but we are going to focus on:
  - matching
    - defining routes using component trees
    - dynamic segments
    - ranking routes and making matches
  - rendering
    - outlets
    - index route
    - grouping routes with a layout route
  - navigating
    - user directed navigation
    - programmatic navigation
