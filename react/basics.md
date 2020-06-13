# React (in a Rails Lense)

## Basic Hello World

```jsx
// index.js
import React from "react"
import ReactDom from "react-dom"

// Using JSX
ReactDom.render(<h1>Hey</h1>, document.getElementById("root")
```

###### `ReactDom.render` can only render one element. If you have sibling elements, you _must_ wrap them in a parent element first. 

## Reusable Web Components

Sort of a fusion between View partials and an Object.

```jsx
// index.js
import React from "react"
import ReactDOM from "react-dom"

function MyApp) {
  return(
    <ul>
      <li>1</li>
      <li>2</li>
      <li>3</li>
    </ul>
  )
}

ReactDOM.render(
  <MyApp />,
  document.getElementById("root")
)
```

###### Components can _also_ only return one element. If you have sibling elements, you _must_ wrap them in a parent element first.

### Externalize Components

1. Make a file with the **exact** name as your Component (`MyApp.js` for example).
1. In the external component import `react`.
1. Move your component from `index.js` into the file you just created.
1. Export component (`export default MyApp`).
1. Import component in `index.js` (`import MyApp from "./MyApp"`), use relative path for component, & can leave out file extension.

## Add some CSS

You need to use `className` instead of `class` because React uses the JavaScript DOM API, which has things like

```js
document.getElementById("root").className
```

So you can style things like:

```jsx
//...
return (
  <header className="navbar">This is the header</header>
)
//...
```

###### You can't add `className` on JSX elements (the ones that _look_ like HTML). You can't add it to a Component


## Interpolate JS in JSX

Just add the `{ }`

```jsx
function App() {
  const firstName = "Jelani"
  const lastName = "Woods"
  
  return (
    <h1>Hello {firstName + " " + lastName}!</h1>
  )
}
```

Or in ES6

```jsx
  return (
    <h1>Hello {`${firstName} ${lastName}`}!</h1>
  )
```

## Props

Conceptually similar to attribute/value pairs of an HTML element.

You give an `<img>` element a `src=""` to some value. That value can change and will dramatically differentiate one `<img>` from another.

Relating more to components, `props` are arguments to a component.

```jsx
<ContactCard 
  name="Mr. Whiskerson" 
  imgUrl="http://placekitten.com/300/200" 
  phone="(212) 555-1234" 
  email="mr.whiskaz@catnap.meow"
/>
```

Use the `props` in the Component.

```jsx
function ContactCard(props) {
    console.log(props)
    return (
      <div className="contact-card">
        <img src={props.imgUrl}/>
        <h3>{props.name}</h3>
        <p>Phone: {props.phone}</p>
        <p>Email: {props.email}</p>
      </div>
    )
}
```

## State

— is data that Component maintains

`props` are immutable. Components recieve them, but they cannot change them.

State _can_ be changed.

1. Component must be Class based.
1. Add Constructor and call `super()`
1. Initialize `state`.

```jsx
class App extends React.Component {
  constructor() {
    super()
    this.state = {
      answer: "Yes" // initial value
    }
  }
  ...
```

Can pass state from Parent to Child Component through props.

```jsx
render() {
  return (
    <div>
      <h1>Is state important to know? {this.state.answer}</h1>
      <ChildComponent answer={this.state.answer}/>
    </div>
  )
}
```

Changing the state with `setState()` will _automatically_, update Child Components that received the state.

## Event handling

Pretty much like regular JS. Camel-cased event HTML [triggers](https://reactjs.org/docs/events.html#supported-events), except you can write the function right there.

```jsx
function App() {
  return (
    <div>
      <img src="https://www.fillmurray.com/200/100"/>
      <br />
      <br />
      <button onClick={() => console.log("I was clicked!")}>Click me</button>
    </div>
 )
}
```

## setState

You can't modify `state` directly and probably don't want to.

```jsx
// BAD
handleClick() {
  this.state.count++
}
```
Use `setState`

You can pass `setState` an object that represents the new state.

```jsx
this.setState({count: 1})
```

You can also pass a function to `setState`. 

```jsx
this.setState(prevState => {
  // ...
})
```

The function must return an Object that represents the new `state`.

```jsx
this.setState(prevState => {
  return {
    count: prevState.count + 1
  }
})
```

This way you have a guarenteed way to accesss the previous state (`this.state` is not guaranteed to be accurate).

> TypeError: Cannot read property 'setState' of undefined

Annoyingly, if you're using a function to call `setState` you first need to bind `this` to the current class.

```jsx
constructor() {
  super()
  this.state = {
    count: 0
  }
  this.handleClick = this.handleClick.bind(this)
}
// ...
// in render()
<button onClick={this.handleClick}>Change!</button>
```

## Lifecycle Methods

- `render()` - not usually labelled as such. How the Component is displayed to the world. Can be called anytime the `state` or `props` change.

- `componentDidMount()` - The very first time the Component shows up, React runs this method. Will only run **once**. Re-renders will _not_ trigger this. Usually used for API call.

- `componentWillReceiveProps()` - DEPRECATED: Run everytime Component receives `props`. Also run when Parent Component sends `props` to Child Component.

  ```jsx
  componentWillReceiveProps(nextProps) {
    if (nextProps.whatever !== this.props.whatever) {
      // do something important here
    }
  }
  ```

- `shouldComponentUpdate(nextProps, nextState)` - return `true`/`false` if you want Component to re-render.

- `componentWillUnmount()` - Used to teardown or clean up code before Component disappears.

```jsx
static getDerivedStateFromProps(props, state) {
  // return the new, updated state based upon the props
  // https://reactjs.org/blog/2018/06/07/you-probably-dont-need-derived-state.html
}

getSnapshotBeforeUpdate() {
  // create a backup of the current way things are
  // https://reactjs.org/docs/react-component.html#getsnapshotbeforeupdate
}
```

- `componentDidUpdate()` - 

```jsx
componentDidUpdate() {
  const newColor = randomcolor()
  this.setState({color: newColor})
}

// Uncaught Invariant Violation: Maximum update depth exceeded. This can happen when a component repeatedly calls setState inside componentWillUpdate or componentDidUpdate. React limits the number of nested updates to prevent infinite loops.
```

Since the `render` is called anytime the `state` is changed and `componentDidUpdate` is called after `render`, changing the `state` will trigger a new `render` which triggers a new `componentDidUpdate` which updates the `state` which triggers the `render`... infinitely. **Must only update `state` based on condition**.

```jsx
componentDidUpdate(prevProps, prevState) {
    if(prevState.count !== this.state.count) {
        const newColor = randomcolor()
        this.setState({color: newColor})
    }
}
```

## Conditional Rendering

### Ternary

Of course, there are a LOT of different ways to do conditionals. Ternary operators, however, are pretty commonly used in React.

expression ? when true do this : otherwise do this


```jsx
render() {
    return (
        <div>
            {this.state.isLoading ?
            <h1>Loading...</h1> :
            <Conditional />}
        </div>
    )
}
```

### Logical AND Operator

Usually, we think of `&&` when writing some conditional and you only want the condition to be `true` if both expressions are `true`.

How JavaScript works under the hood in regards to `&&`:
- the first expression on the left will be evaluated
- if the first expression is _not_ true the whole condition is `false`
- if the first expression **is** `true`, the second expression is **immediately** returned

This allows us to do some cleverness with conditional Component rendering.


Say we have a list of unread messages in `state` and we only want to display them if the list has messages.

Using a Ternary we could have something like

```jsx
{
    this.state.unreadMessages.length > 0 ? 
    <h2>You have {this.state.unreadMessages.length} unread messages!</h2> :
    null
}
```

But using the logical AND, we can do

```jsx
render() {
    return (
        <div>
            {
                this.state.unreadMessages.length > 0 && 
                <h2>You have {this.state.unreadMessages.length} unread messages!</h2>
            }
        </div>
    )
}
```

Since if `this.state.unreadMessages.length > 0` is `true`, then `<h2>You have {this.state.unreadMessages.length} unread messages!</h2>` will immediately be returned.
