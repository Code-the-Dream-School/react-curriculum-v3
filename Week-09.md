---
title: Week-09
dateModified: 2025-01-06
dateCreated: 2024-08-20
tags: [react]
parent: "[[Intro to React V3]]"
draftStatus: draft
content: lesson plan
topics: [data caching, refetching, useCallback, useMemo]
week: 9
---

# Week-09

## Introduction

### Topics Covered

- Limiting Network Requests
- useMemo and useCallback

### Lesson Objectives

By the end of this lesson, we will:

#### Objective 1: Limiting Network Requests

- explain the importance of limiting fetch requests to APIs
- implement caching on search results
- throttle fetch requests to meet API constraints

#### Objective 2: useMemo

- define useMemo and useCallback
- demonstrate how each can be used to improve an app's performance

## Discussion Topics

### Limiting Network Requests

> [!note]
> The repo for this discussion topic can be found here: https://github.com/royemosby/ctd-ingredient-recipes #placeholder/update-link #placeholder/move-repo

Taking a step back from CTD Swag for this topic, we need to discuss how to work efficiently with an API. To work through the next several sections, we'll use [Spoonacular's API](https://spoonacular.com/food-api) to update a simple recipe finder that searches for recipes based on ingredients. The app contains a form for the user to enter a search term. When submitted, a fetch request returns a list of results containing that ingredient. These results are shown as a list below the form. Each recipe title is linked to its source page that opens in a new tab.

At the time of their writing, [Spoonacular's introductory tier for API access](https://spoonacular.com/food-api/pricing) includes a 150 requests per day restriction and a 1 request per second limitation. We'll employ several techniques to help us make the most of this API. We'll implement caching for searches and throttling to prevent a requests don't happen in rapid succession. We'll approach these tasks using the React tools that we've already been introduced to. After we've completed caching and throttling, we'll introduce 2 React hooks we have not seen yet to help make sure our app is running efficiently.

#### Caching Search Results

Caching is a technique used to store data fetched from the server in a temporary storage location, such as the browser's memory or local storage. This stored data can be quickly accessed when needed without making repeated network requests. In our case, if a user searches for "chicken" several times, the app uses API quotas fetching data that we've already had access to. Even without the 150 request per day limitation, we can still employ caching to save the user's bandwidth and speed up repeated searches.

One approach to this is to store the query and its search results in a lookup object. A lookup object is just a normal JavaScript object that uses the search query as a key to hold the search results. The example object below contains 2 searches, one for chicken and one for spaghetti. If a user searches for "spaghetti, tomato" again, we can access the previous search's results in the object with `searchCache["spaghetti, tomato"]`.

```javascript
//example lookup object with cached search results

const searchCache = {
	"chicken": [
	{id: 123, title: 'red lentil soup with chicken and turnips', sourceUrl:'... recipe URL'},
	{id: 456, title: 'chicken enchilada quinoa cassserole', sourceUrl:'...recipe URL'}
		],
	"spaghetti, tomato": [
	{id: 789, title: 'spaghetti pomodoro', sourceUrl:'...recipe URL'},
	{id: 234, title: 'spaghetti caronara', sourceUrl:'...recipe URL'},
	{id: 567, title: 'baked spaghetti', sourceUrl:'...recipe URL'}
	]
}
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
      }
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
          options
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

![[202501_1147AM-Brave Browser.gif]]

#### Throttling Request Rates

Another type of common API requirement is that a set amount of time must elapse before it will accept another request for processing. This use of throttling, also known as rate limiting, is a protective measure to prevent over-taxing the API's infrastructure. When we update the search results to let the user page through all the results, we have to be mindful of this constraint, we need to prevent the app from submitting a request until at least one second has elapsed since the previous one is sent.

We are going to look closer at pagination in [[Week-12|week 12]] but we need to know some basic details about the API response that enable us to paginate. Each API response includes a portion of the search results, an offset value(how many recipes that have been skipped), and a total number of results in a search. By updating the fetch logic and updating the UI, the app ends up with a group of buttons below the results that allow a user to page through all the search results.

![[202501_0550PM-Brave Browser.gif]]

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

![[202501_0800PM-Brave Browser.gif|500]]

That is not the sort of delay that a user would want on their application so we can refine timeout duration. We can look at the network requests in the network activity tab in our browser to figure out how long it's taking the request to process. We see in the following request, that we're waiting around 160 milliseconds for the server to respond.

![[202501_0942AM-Brave Browser.png|500]]

We'll send off several requests and then average out the wait times between each. For this API, the response time averages out to 350ms. We can then reduce the timeout delay by this amount, making the interface a little friendlier to use without running against the API request speed limitations.

![[202501_0950AM-Brave Browser.gif|400]]

### useMemo and useCallback

Caching and throttling are 2 approaches that increases the efficiency of API usage in an application. There are other optimizations that we can make to a React codebase as well. This week we will introduce `useMemo` and `useCallback`.

#### useMemo

The useMemo hook memoizes slow or complex functions so that they are only invoked when their inputs have changed. The memoized function behaves the normally when first called but then React also stores the results associated with its dependency values. If the function is invoked again and the dependency values have not changed, React will return the results rather than re-run the memoized function. While

This helps us optimize performance by avoiding unnecessary recalculations.

Some example use cases for the useMemo hook in React include:

- **Memoizing Computations**: Memoize complex calculations or data transformations to avoid redundant calculations on every render.
- **Optimizing Component Rendering**: Memoize the result of component rendering logic based on specific input props.
- **Preprocessing Data**: Process data before rendering components and memoize the processed data to improve performance.
- **Conditional Rendering**: Memoize conditional logic to determine when certain components should render based on dependencies.
- **Caching API Responses**: Cache and reuse API responses to prevent unnecessary network requests.

#### useCallback

When a component re-renders, it re-defines functions found in it. Normally, this does not cause a problem. However, if too many many child components or useEffects are dependent on a function, it can cause performance issues. Recall that React's approach to re-rendering is to do so so when a dependency changes. `useCallback` works by memoizing the function definition and then returning for use. That now memoized function remains unchanged between re-renders unless its dependencies are changed.

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

### Data Caching and Refetching

- [How and when to debounce or throttle in React (LogRocket Blog)](https://blog.logrocket.com/how-and-when-to-debounce-or-throttle-in-react/)

### useMemo and useCallback

- [useMemo (React docs)](https://react.dev/reference/react/useMemo#usememo)
- [useCallback (React docs)](https://react.dev/reference/react/useCallback)
- [Understanding useMemo and useCallback (Josh Comeau)](https://www.joshwcomeau.com/react/usememo-and-usecallback/)
- [Mastering React Memo - YouTube](https://www.youtube.com/watch?v=DEPwA3mv_R8&t=1423s)
