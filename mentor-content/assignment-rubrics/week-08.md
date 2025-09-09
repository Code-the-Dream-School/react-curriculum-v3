# Assignment Rubric, Week 08

## Expected App Capabilities

After completing this week's assignment, the student's app should:

- [ ] Use the API to sort todos by `title` or `createdTime`
- [ ] Use the API to search for todos based on title contents

## Technical Details

- [ ] Fetch todos from the API and display them in the app.
- [ ] Implement sorting functionality for todos by `title` and `createdTime` using the API.
- [ ] Implement search functionality to filter todos by title using the API.
- [ ] Ensure API requests handle loading and error states gracefully.
- [ ] Update the UI based on sorting and search results.

## Details to Watch For

- [ ] Sorting and search are both accessible via the UI (e.g., dropdowns, input fields)
- [ ] No duplicate todos displayed after API operations
- [ ] Edge cases (e.g., empty search results, API errors) are handled gracefully

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
