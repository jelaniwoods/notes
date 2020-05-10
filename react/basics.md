# React (in a Rails Lense)

## Basic Hello World

```jsx
import React from "react"
import ReactDom from "react-dom"

// Using JSX
ReactDom.render(<h1>Hey</h1>, document.getElementById("root")
```


## Reusable Web Components

Sort of a fusion between View partials and an Object.

```jsx
import React from "react"
import ReactDOM from "react-dom"

function MyApp) {
  return (
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

#react
