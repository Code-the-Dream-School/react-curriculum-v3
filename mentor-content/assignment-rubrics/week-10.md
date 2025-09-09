# Assignment Rubric, Week 10

## Expected App Capabilities

After completing this week's assignment, the student's app should:

- Have custom styling applied through css modules and styled components.
- Include some custom imagery

## Technical Details

- [ ] **App.css** is used for global styles (font families, background color, common button/input styles).
- [ ] Any previous inline or global styles have been refactored into CSS modules or styled-components as appropriate.
- [ ] **CSS Modules** are used for App, TodoList, and TodoListItem components:
  - [ ] Each component has an associated `.module.css` file imported and used.
  - [ ] App.module.css centers the app in the body (using flex or grid).
  - [ ] App.module.css adds a border to the error message container.
  - [ ] TodoList.module.css removes extra padding from the unordered list and removes list item bullets.
  - [ ] TodoListItem.module.css adds padding to the bottom of each list item.
  - [ ] Class-based styling is applied using dot-notation from the imported styles object.
- [ ] **Styled-Components** are used in TodoForm, TodosViewForm, and TextInputWithLabel:
  - [ ] Styled-components are defined in the same file as the component (unless shared).
  - [ ] Styled-components are named with a "Styled" prefix (e.g., StyledForm, StyledButton).
  - [ ] Minimal JSX changes—elements are swapped for styled components.
  - [ ] Form items have padding for spacing.
  - [ ] The TodoForm button font is italic when disabled.
- [ ] **Imagery** (optional):
  - [ ] Custom imagery is included (e.g., background image, logo, SVG icon for checkbox, error icon) if appropriate and does not disrupt app functionality.
- [ ] No changes to the element structure in JSX except for swapping in styled components.
- [ ] All required files are present and correctly imported.

## Details to Watch For

- [ ] all style properties are valid and make sense for the selected element
- [ ] ensure interface is text is still readable

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
