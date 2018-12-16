# React
A JS library for for building user interfaces.
> Not a framework
- React is small and not a complete solution
- If the device we are trying to interface can understand JS, we can use React to describe a user interface. 
- Without React, we would need to manually build user interfaces with native web APIs in JS (React is declarative).
- Some people think of it as the V in Model View Controller.
- GraphQL
- Design concepts:
  - 1) Components
    - like functions
    - reuseable and composable
    - but they can manage a private state
  - 2) Reactive updates
    - react will react
    - take updates to the browser
> When the state of the component, the input, changes, the user interface it represents, the output, changes as well. So reacts to state changes and automatically updates the DOM.
  - 3) Virtual Views in memory
    - we write HTML using JS (using JS to render HTML)
      - allows react to have a virtual representation of HTML in-memory (**Virtual DOM**)
        - renders an html tree first, then every time a state changes, instead of writing the whole tree, it will only write the difference (**tree reconciliation**)

## 2 types of Components

- Functional
  - stateless
  - receives properties (props)
  - returns what look like HTML (JSX)
  - does not `extend React.Component`
  - often presentational, where no interactive will happen
- Class
  - stateful
  - receives props
  - also considers a private internal state 
    - when this state changes, is when re-render is triggered
  - only use when you need to manage state   
- **State vs Props**
  - the state could be changed
  - while props are all fixed values

> React can be used without JSX.

React components must be capital.

> Props is an object that holds all the values that were passed when the component was rendered.
