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
