
### MUI
* [Bundle size](https://mui.com/guides/minimizing-bundle-size/)
  * Prefer `import Button from '@mui/material/Button';`
  * Not `import { Button, TextField } from '@mui/material';`
* After creating react app, run `npm install @mui/material` and the dependencies will be added to `package.json`
  - also need `npm install @emotion/react`, `npm install @emotion/styled`
  - `npm install @mui/styles`
  - `npm install @material-ui/core`

## React
* [Devtool](https://github.com/facebook/react/tree/main/packages/react-devtools): `npm install -g react-devtools`

### Component and Elements
* Components are made of elements
* Elements are immutable. To update it, new element is created.
* Components are similar to function,
  - they receive params as `props`
  - return a React element
* Component name must start with capital. Lower case elements are HTML tags
* Component must never modify props (must be `pure`)
* Component can return `null` so it won't be rendered
* Controlled component: have value directly tied to state:
  * text
  * text area
  * select
* Use composition instead of inheritance

### State
* State is similar to props, but fully managed by the compoennt internally
* State is setup in `constructor`, after calling `super(props);`
* Call `setState` to trigger re-render component
  - cannot directly assign state after `constructor`, must use `setState`
* if new state depends on current state, pass current state as param:
* State can be passed down to child component as props, but never upward (like waterfall)

```
this.setState((state, props) => ({
    counter: state.counter + props.increment
  }));
```

### Lift State
* Remember the top-down data flow
* When two component needs to access the same state, lift the state to the closest common ancestor
* Ancestor declares the state, and pass the state value and event handler as props

### Hooks
* Hooks are a way to reuse stateful logic, not state itself.
* Tutorial: https://reactjs.org/docs/hooks-overview.html
* Hooks start with `useXXX` by convention
* Function component is a function that uses state
* `useState` is a Hook that lets you add React state to function components
  - return a getter and setter pair
* `useEffect`:

### Events
* Pass function as event handler, such as `onClick={activateAccount}`
* JavaScript does not bind `this` by default, must manually bind:
  - example: `this.handleClick = this.handleClick.bind(this);`
  - alternatively, use lambda to define a function
  - or use lambda function within callback

```
handleClick = () => {
  console.log('this is:', this);
}  

<Button onClick={this.handleClick} ?>
<Button onClick={() => this.handleClick()} ?>
```

### Desctucuring
* Documentation: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment#array_destructuring
* From object:

```
const user = {
    id: 42,
    isVerified: true
};

const {id, isVerified} = user;
```

* Rename variables:

```
const o = {p: 42, q: true};
const {p: foo, q: bar} = o;
```

### Handson
* Clear cache: `npx clear-npx-cache` (see [link](https://exerror.com/you-are-running-create-react-app-4-0-3-which-is-behind-the-latest-release-5-0-0/))
