# Assignment Rubric, Week 07

>[!note]
>Students are using Airtable as an API. Don't ask to borrow the student's credentials! You should set up an Airtable base that matches the specs of the lesson and then use your own credentials in an .env.local file.

## Expected App Capabilities

After completing this week's assignment, the student's app should:

- [ ] Save todos to an Airtable base
- [ ] Load saved todos from Airtable on loading
- [ ] Display pending messages during network requests
- [ ] Display error messages on network or authentication error

## Technical Details

- [ ] App:
  - [ ] reference variables in the ENV file to construct fetch requests.
  - [ ] uses an `isLoading` state variable for use while loading the todo list
  - [ ] uses an `isSaving` state variable for use while saving a todo
  - [ ] uses a `useEffect` to send off fetch request and manage the `isLoading` state variable while request is in flight.
    - [ ] contains a `useEffect`
      - [ ] should have an empty dependency array
      - [ ] uses an async function defined and invoked inside the `useEffect`
      - [ ] it should include try/catch/finally blocks and update the UI pessimistically
  - [ ] should conditionally display an error message if a fetch fails
  - [ ] `addTodo` should be updated:
    - [ ] converted to async function
    - [ ] uses form data to construct a new todo that is nested in a `payload` object
    - [ ] uses a POST request to post new todo
    - [ ] sets `isSaving` to true while request is in flight and resets it in a `finally` block
    - [ ] updates the todo list on success
  - [ ] `updateTodo` should be updated
    - [ ] converted to async function
    - [ ] finds and replaces the original todo with the new version
    - [ ] sends PATCH request, throws if error
    - [ ] `catch` block is used to set `errorMessage` and revert to the previous version of the todo list
    - [ ] manages `isSaving`
  - [ ] `completeTodo` behaves similar to `updateTodo` but only sets `isCompleted` to true
- [ ] TodoList
  - [ ] should use the `isLoading` prop to conditionally render a paragraph or a loading message.
- [ ] TodoListItem
  - [ ] includes a `useEffect`
    - [ ] includes `todo` in deps array.
    - [ ] that resets the `workingTitle` back to the original value derived from `title` props.

## Details to Watch For

- [ ] Airtable base's columns should be identical across student projects - no custom fields or renamed fields. If it does not work with your base and your own credentials, then the project is not set up correctly and needs revisions.
- [ ] Students should account for falsy values that are omitted from all Airtable responses.
- [ ] Students may start refactoring to extract helper functions

## Code Hygiene Recommendations

*We leave the code hygiene details to the discretion of the reviewer but also recommend a few general considerations:*

- [ ] consistent formatting and block nesting
- [ ] short, meaningful value names
- [ ] repo contains no unused code
  - [ ] no commented out code
  - [ ] no unused imports/variables
  - [ ] no redundant imports eg: `import React from 'react'`
- [ ] comments should be short and accurate
- [ ] ESLint and Prettier can automate formatting and bad practice identification. This leaves more space to focus on learning React and improves student/mentor interaction. Implementation instructions are in [week 1's stretch goals](https://github.com/Code-the-Dream-School/react-curriculum-v3/blob/main/learns-app-content/assignments/week-01.md#stretch-goals-instructions-optional).
