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

Changing the state with `setState()` will _automatically, update Child Components that received the state.

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
