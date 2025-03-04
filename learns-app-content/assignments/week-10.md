## Weekly Assignment Instructions

### Expected App Capabilities

After completing this week's assignment, your app should:

- Have custom styling applied through css modules and styled components.
- Include some custom imagery

This assignment differs from the previous ones in that it's open-ended. The instructions will provide some minimum requirements and designate which components should use which approaches. We encourage you to explore the styling and compare how the two technologies fit into your coding styles.

### Instructions Part 1: Foundation Styles

- Use App.css to provide some style changes such as:
  - Assign font-families for headings and other textual elements
  - Change the background color of the body and/or `#root`.
  - Common styles for buttons, text inputs.
- If you've used any styling up to this point, refactor that styling into the module or styled-component associated with the targeted element.

### Instructions Part 2: CSS Modules

- Use css modules for App, TodoList, and TodoListItem. Create these files alongside the components as described by the lesson and import them into their respective component.
- Use class-based styling in the module and assign the classNames using dot-notation on the imported styles.
- Don't change the element structure in the JSX.
- At minimum, make the following changes:
  - App.module.css:
    - Center the app in the body. Hint: use flex or grid.
    - Create a border on the div containing the error message
  - TodoList.module.css:
    - Eliminate the extra padding on the unordered list
    - Remove the list item bullets
  - TodoListItem.module.css:
    - Add small amount of padding to the bottom of the list item.

### Instructions Part 3: Styled-Components

- Use Styled-Components inside TodosForm, TodosViewForm, and TextInputWithLabel.
- Keep the styled components inside the main component file unless they are shared between components.
- Keep naming simple - use a prefix "Styled" for each element that is replaced with a styled-component: eg: StyledForm, StyledButton, etc.
- Make minimal changes to the JSX just to swap elements out with the styled components you define.
- At a minimum, make the following changes:
  - Add a small amount of padding on the items in each form to give them some spacing.
  - Make the font in the TodoForm's button italic when it is disabled.

### Instructions Part 4: Imagery

This part of the exercise is completely optional. You may add imagery so long as it doesn't disrupt the functionality of the app and it's appropriate for public sharing.

Some ideas:

- background image for the app
- logo alongside the title
- replace the checkbox input with an svg icon
- error icon beside error message

### Closing Notes
