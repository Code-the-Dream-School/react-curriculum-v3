---
title: Week-09
dateModified: 2024-12-27
dateCreated: 2024-08-20
tags: [react]
parent: "[[Intro to React V3]]"
week: 9
topics: [data caching, refetching, useCallback, useMemo]
content: lesson plan
---

# Week-09

## Introduction

> [!drafting note] #drafting-note
> - let's also include updating the todo object with a completed bool so we can work with longer lists in future lessons
> - focus on url params for searching and filtering

> [!drafting note] #drafting-note
> - **An authenticated user should stay logged into CTD-Swag between sessions**
	- existing JWT cookie should be valid for the length of time a user should remain logged in. This app will use 7 days.
	- application automatically includes the JWT as a cookie on all requests to the SPA's domain.
		- Navigating to CTD-Swag's home page fires off a GET request for the products. Since the API will also see this cookie on the request it will be able to identify that the request came from a logged in user and will send user and cart information.

### Topics Covered

- Limiting Network Requests
- Refetching
- useMemo and useCallback

### Lesson Objectives

By the end of this lesson, we will:

#### Objective 1: Limiting Network Requests

- explain the importance of limiting fetch requests to APIs
- use useCallback to prevent unnecessary data fetching

#### Objective 2: Refetching

- explain the importance of fresh data in an application
- construct a fetch request to retrieve filtered or sorted data

> [!left off here]
> This is about doing fresh fetches for filters and sorts as opposed to re-querying locally stored data.

#### Objective 3: useCallback vs useMemo

- compare and contrast useCallback and useMemo

## Discussion Topics

### Limiting Network Requests

- reasons for caching
	- even on fast networks, large data transfer is still slow compared to the instantaneous updates in desktop applications - minimize expensive transfers
	- APIs may have rate limitations & eventually throttle requests
- using useCallback instead of useEffect allows us to prevent network calls every time the component refreshes
	- function is saved across renders
	- only called when item in its dependency array is updated
		- unlike useEffect that also fires every time a component re-renders
- race conditions
	- search query sends request every during each key press -> cannot guarantee that each request will return in order

### Refetching

> [!drafting note] #drafting-note
> pagination is discussed in [[Week-12]]

- data becomes outdated
	- social media feeds always adding new
	- stores merchandise runs out
	- news articles get updates and corrections
- not all data may be sent at once
	- filters and searches don't need to send all system entries
- use query value as useCallback dependency

### useCallback vs useMemo

- useCallback
	- saves function across renders
	- called when dependency is updated
- useMemo
	- saves the results returned from a function call
	- returns result again only when dependency is updated
	- optimization tool for expensive functions or components
	- can wrap a component with useMemo (React.memo() HOC is preferred for this though)
	- not full memoization! - only compares current state to previous state
		- re-renders only if props change - does not save all previous states

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
