# Week-04

> [!recorder's note]
> Getting state right is one of the most important parts of this class. Most of working with React revolves around fetching, working with, or displaying state. This week's topics are tightly coupled and introduce rules and best practices. The React docs are vague on some of the overlapping terminology around handler functions and props. A great deal of effort has been put into disentangling terms and definitions. Although the content is outlined into 3 videos, recorders should look over the lesson instructions before planning their videos.

## Video 1 of 3: Basic Hooks

- introduction to hooks
  - definition
  - added features to functional components
  - briefly describe hook categories categories of hooks
  - dive deeper into useState
    - immutability
    - common usage convention: `const [state, updateState] = useState(initialState);`
    - updating primitive values - updating objects, arrays
  - deeper dive into useEffect
    - synchronizing with external systems - (we will be using useEffect for fetching in this course)
    - optionally returns cleanup function
    - dependency array - empty, omitted, etc.
  - deeper dive into useRef
    - track property independent of render cycle - rules for `useRef`
    - don't use in UI rendering
    - don't read or update during rendering cycle (access it at the top level of the component function)
    - ^^ unless results are predictable

## Video 2 of 3: Events and Handlers

- communication of children to parent through callback invocation
- synthetic events
  - wrapper around browser's event object
  - meant to normalize event APIs across different browser vendors
  - common properties: target, currentTarget
  - common methods: preventDefault(), stopPropogation()
- handler props
  - ALOT of them! - organized by event type
  - identified in React docs by "on" prefix
  - adds relevant properties/methods based on event type
  - Focus on MouseEvent, TouchEvent, InputEvent

## Video 3 of 3: Updating State

- handler composition options
  - pass state update function
  - compose helper in parent
  - compose helper in child
  - compose helper inline in props - eg `onClick={() => setCount(count + 1)}`
- putting it all together
  - styling should be clean enough - using class selectors and adding everything to App.css
  - load inventory with useEffect rather than adding as initial state
  - create cart feature
    - manage with useState in App
    - remove promoted card (if it's there)
    - create helper functions to add/ remove items from cart
    - need distinct ids for cart items using Date.now()
    - add props to ProductList, ProductCard (not using context)
    - pass handleAddItemToCard down to card
    - add button to ProductCard
    - wire handler to button onClick
  - review state changes in React Dev Tools
  - add shopping cart icon to header
  - add props to pass cart to header
  - add cart.length to display over cart
  - `removeItemFromCart` will be wired in a later lesson - need to cover conditional rendering first
  - add console statement to print each time item is added to cart
