## Discussion Topics

### Limiting Network Requests

> [!note]
> The repo for this discussion topic can be found here: [ctd-ingredient-recipes (GitHub repo)](https://github.com/Code-the-Dream-School/ctd-ingredient-recipes)

Taking a step back from CTD Swag for this topic, we need to discuss how to work efficiently with an API. To work through the next several sections, we'll use [Spoonacular's API](https://spoonacular.com/food-api) to update a simple recipe finder that searches for recipes based on ingredients. The app contains a form for the user to enter a search term. When submitted, a fetch request returns a list of results containing that ingredient. These results are shown as a list below the form. Each recipe title is linked to its source page that opens in a new tab.

At the time of their writing, [Spoonacular's introductory tier for API access](https://spoonacular.com/food-api/pricing) includes a 150 requests per day restriction and a 1 request per second limitation. We'll employ several techniques to help us make the most of this API. We'll implement caching for searches and throttling to prevent a requests don't happen in rapid succession. We'll approach these tasks using the React tools that we've already been introduced to. After we've completed caching and throttling, we'll introduce 2 React hooks we have not seen yet to help make sure our app is running efficiently.

#### Caching Search Results Using Memoization

Caching is a technique used to store data fetched from the server in a temporary storage location, such as the browser's memory or local storage. This stored data can be quickly accessed when needed without making repeated network requests. In our case, if a user searches for "chicken" several times, the app uses API quotas fetching data that we've already had access to. Even without the 150 request per day limitation, we can still employ caching to save the user's bandwidth and speed up repeated searches.

One approach to this is to store the query and its search results in a lookup object. A lookup object is just a normal JavaScript object that uses the search query as a key to hold the search results. The example object below contains 2 searches, one for chicken and one for spaghetti. If a user searches for "spaghetti, tomato" again, we can access the previous search's results in the object with `searchCache["spaghetti, tomato"]`.

```javascript
//example lookup object with cached search results

const searchCache = {
  chicken: [
    {
      id: 123,
      title: 'red lentil soup with chicken and turnips',
      sourceUrl: '... recipe URL',
    },
    {
      id: 456,
      title: 'chicken enchilada quinoa cassserole',
      sourceUrl: '...recipe URL',
    },
  ],
  'spaghetti, tomato': [
    { id: 789, title: 'spaghetti pomodoro', sourceUrl: '...recipe URL' },
    { id: 234, title: 'spaghetti carbonara', sourceUrl: '...recipe URL' },
    { id: 567, title: 'baked spaghetti', sourceUrl: '...recipe URL' },
  ],
};
```

After visualizing the cache, we'll create an empty state object to store queries and their associated search results.

```jsx
const [searchCache, setSearchCache] = useState {};
```

We next update the useEffect containing our search logic that fires every time a search term is submitted. Here is the original `useEffect` so we can see how caching fits in while we make updates:

```jsx
// extract from App.jsx
// ...code
useEffect(() => {
 //prevents fetch if term blank
    if (!term) {
      return;
    }
    async function getRecipes() {
      const options = {
        headers: {
          'Content-Type': 'application/json',
          'x-api-key': `${KEY}`,
        },
      };
      try {
        const resp = await fetch(
          `${BASE_URL}/complexSearch?includeIngredients=${term}&addRecipeInformation=true`,
          options
        );
        if (resp.ok) {
          // resp includes number, offset, totalResults
          const recipeList = await resp.json();
          setRecipes([...recipeList.results]);
          setTerm('');
        }
      } catch (e) {
        console.log(e);
      },
      [term, setRecipes, setTerm]
}
```

The newly updated state management flow can be summarized into the following events:

1. user submits term
2. useEffect containing fetch and caching logic executes
3. the term is compared to the cache object's keys

- **if match**: that value is used to update the `recipes` state array then the function exits, preventing network request.

4. without match, an async function containing the fetch is composed.

- **if response is okay**: set `recipes`, reset `term` to blank, and add term/response value to cache
- **if not okay**: handle errors

Knowing this flow of events we can then go back to the original `useEffect` to determine where to update the logic. After updating the `useEffect` logic to include caching, we get the following:

```js
// extract from App.jsx
//...code
useEffect(() => {
  if (!term) {
    return;
  }
  if (searchCache[term]) {
    console.log(`term ${term} found, returning cache...`);
    setRecipes([...searchCache[term]]);
    setTerm('');
    return;
  }
  async function getRecipes() {
    console.log(`getRecipes()`);
    const options = {
      headers: {
        'Content-Type': 'application/json',
        'x-api-key': `${KEY}`,
      },
    };
    try {
      const resp = await fetch(
        `${BASE_URL}/complexSearch?includeIngredients=${term}&addRecipeInformation=true`,
        options,
      );
      if (resp.ok) {
        console.log('response okay');
        // resp includes number, offset, totalResults
        const recipeList = await resp.json();
        setRecipes([...recipeList.results]);
        console.log(`caching search for "${term}"`);
        setSearchCache((prev) => ({
          ...prev,
          [term]: [...recipeList.results],
        }));
        setTerm('');
      }
    } catch (e) {
      console.log(e);
    }
  }
  getRecipes();
}, [term, searchCache]);
//code continues...
```

The console statements from the above statement illustrates how the caching logic prevents the request in this screen recording:

![searching for chicken then beef](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/lessons/assets/week-09/search-chicken-beef.gif)

#### Throttling Request Rates

Another type of common API requirement is that a set amount of time must elapse before it will accept another request for processing. This use of throttling, also known as rate limiting, is a protective measure to prevent over-taxing the API's infrastructure. When we update the search results to let the user page through all the results, we have to be mindful of this constraint, we need to prevent the app from submitting a request until at least one second has elapsed since the previous one is sent.

We are going to look closer at pagination in [[Week-12|week 12]] but we need to know some basic details about the API response that enable us to paginate. Each API response includes a portion of the search results, an offset value(how many recipes that have been skipped), and a total number of results in a search. By updating the fetch logic and updating the UI, the app ends up with a group of buttons below the results that allow a user to page through all the search results.

![paging through search results](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/lessons/assets/week-09/search-chicken-pagination.gif)

The easiest way to throttle the response is to limit the availability of the buttons. We can temporarily set state to disable the button and then use `setTimeout` to re-enable them after a certain time has elapsed. We'll look at the handler functions since they end up with the logic to perform the throttle:

```js
// extract from App.jsx
//...code
function pageForward() {
  setIsPaginationDisabled(true);
  const maxPages = Math.ceil(resultsCount / paginationSize);
  const currentPage = Math.ceil(currentOffset / paginationSize) + 1;
  if (currentPage >= maxPages) {
    return;
  }
  setNextOffset(currentOffset + paginationSize);
  setTimeout(() => setIsPaginationDisabled(false), 1000);
}

function pageBack() {
  setIsPaginationDisabled(true);
  if (currentOffset <= 0) {
    return;
  }
  setNextOffset(currentOffset - paginationSize);
  setTimeout(() => setIsPaginationDisabled(false), 1000);
}
//code continues...
```

![demo of button disabled by a timer](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/lessons/assets/week-09/search-lentils.gif)

That is not the sort of delay that a user would want on their application so we can refine timeout duration. We can look at the network requests in the network activity tab in our browser to figure out how long it's taking the request to process. We see in the following request, that we're waiting around 160 milliseconds for the server to respond.

![response time in network tab](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/lessons/assets/week-09/response-time-hilight.png)

We'll send off several requests and then average out the wait times between each. For this API, the response time averages out to 350ms. We can then reduce the timeout delay by this amount, making the interface a little friendlier to use without running against the API request speed limitations.

![delay timing minimized](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/lessons/assets/week-09/search-spinach.gif)

### useMemo and useCallback

Caching and throttling are 2 approaches that increases the efficiency of API usage in an application. There are other optimizations that we can make to a React codebase as well. This week we will introduce `useMemo` and `useCallback`.

#### useMemo

React’s `useMemo` hook uses memoization to address performance problems cause by expensive calculations and unnecessary re-rendering. `useMemo` returns the _results_ of the memoized function so we reference it similar to a variable. Since it is a hook, its usage does have some constraints:

- It must be used at the top level of the component. Stated differently, it cannot be nested into any other functions or conditional statements.
- The function definition added to useMemo to calculate return values cannot take any arguments.
- Any values that we need to work are added to `useMemo`'s dependency list.

The first portion of the code example below implements memoization in plain JavaScript using a closure. The end portion contains the React equivalent. They do same work but `useMemo` results in much cleaner code more appropriate to a React codebase.

```javascript
/* plain JavaScript */
// function to be memoized
const expensiveFunction(someInput){
 return //some new value based on someInput
}

//memoizing function returns the memoized function
const memoizeExpensiveFunction(){
 const cacheObject ={}
 //returned function retains access to `cacheObject` since
 //they are both defined in the same lexical scope
 return function (input) {
  if(cacheObject[input]){
   return cacheObject[input]
  } else {
   const result = expensiveFunction1(input);
   cachedObject[input] = result;
   return result;
  }
 }
}

// assigning the returned function
const importantOperation = memoizeExpensiveFunction()

// calling memoized function and assigning its results
const expensiveOutput = importantOperation(input)
/* end plain JavaScript */

/*----------------------*/

/* React's useMemo version*/

//function to be memoized
const slowFunction(someInput){
 return // some new value based in someInput
}

//0 argument function given to useMemo
//dependency of memoized function placed
//in useMemo's dependency array.
const slowFunctionMemo = useMemo(()=>{
 return slowFunction(someInput)
}, [someInput])

return (
<div>
 {/*only updates when `someInput` changes*/}
 <p>{slowFunctionMemo}</p>
</div>
)
```

> [!note]
> We say that they do the same work but `useMemo` also "hooks" into React's render cycle so does some other things behind the scenes.

Common scenarios that could benefit from `useMemo`:

- **Memoizing Computations**: Memoize complex calculations or data transformations to avoid redundant calculations on every render.
- **Optimizing Component Rendering**: Memoize the result of component rendering logic based on specific input props.
- **Preprocessing Data**: Process data before rendering components and memoize the processed data to improve performance.
- **Conditional Rendering**: Memoize conditional logic to determine when certain components should render based on dependencies.

The recipe finder app is small so are no performance issues but we can employ `useMemo` to save some data pre-processing. In the `useEffect` that handles the query, we can memoize a computed string used to save/retrieve previous responses in cache: ``const pendingQuery = `${term} offset ${nextOffset}`;``. The only time that this value would ever change is if `term` or `nextOffset` updated. To make the change to using use `useMemo`, we would:

1. Convert the string assignment into a function definition that accepts no arguments and returns the string.
2. Place that function into `useMemo`
3. Add `term` and `nextOffset` to the `useMemo`'s dependency list.
4. Update the locations where string was used with the memoized function's returned value.

```jsx
// extract from App.jsx
// ... component code
const pendingQuery = useMemo(
    () => `${term} offset ${nextOffset}`,
    [term, nextOffset]
);

useEffect(() => {
if (!term) {
      return;
    }
    if (nextOffset > 0 && nextOffset === currentOffset) {
      return;
    }
    if (searchCache[pendingQuery]) { //<--updated
      console.log('setting from cache: ', searchCache[pendingQuery]); //<--updated
      setCurrentOffset(nextOffset);
      setRecipes([...searchCache[pendingQuery]]); //<--updated
      return;
    }
    //continued useEffect code...
//continued component code...
}
```

#### useCallback

During a re-render, a component re-defines functions found inside the component body. Everywhere that function is referenced is then passed this new version which can cause a cascade of re-renders. For example, we may have a couple forms in our app that need the same field validation. We can define a validation function in a parent component and pass that function on as props to the form components. Each time that parent component re-renders, that new function will cause the form components to re-render too. Normally, the Render cycle is so fast that this does not cause a problem. Performance issues may arise when too many child components or useEffects are dependent on that function.

`useCallback` addresses this problem by caching the function definition between re-renders. That cached function now remains unchanged unless its dependencies change, preventing needless re-renders. As it is a hook, it can only be called at the top level of the component. The dependency array that this hook uses should include all reactive values referenced inside of the callback

We can see it in action by wrapping the handler methods the buttons use to page back and forth through the recipe list.

```js
const pageBack = useCallback(() => {
  setIsPaginationDisabled(true);
  setNextOffset((prevOffset) => Math.max(prevOffset - paginationSize, 0));
  setTimeout(() => setIsPaginationDisabled(false), 650);
}, [paginationSize]);

const pageForward = useCallback(() => {
  setIsPaginationDisabled(true);
  setNextOffset((prevOffset) => prevOffset + paginationSize); // uses a setter function
  setTimeout(() => setIsPaginationDisabled(false), 650); //
}, [paginationSize]);
```

Recall that the state update function will provide the previous state value to its setter function. This approach allows us to keep the dependency list short.

To close out this week's lesson, it's important to re-emphasize that the `useEffect` and `useCallback` hooks should primarily be used as optimization tools. These hooks are designed to control when effects are executed and a prevent unnecessary renders. While they can improve efficiency when re-renders and side effects need fine-tuning, overuse can lead to unnecessary complexity and hinder code readability. Apply `useEffect` and `useCallback` judiciously.
