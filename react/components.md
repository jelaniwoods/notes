# Components

## Mapping Components

A Component can render an Array of components.

```jsx
// jokesData is an Array of Hashes
function App() {
  const jokeComponents = jokesData.map(joke => <Joke key={joke.id} question={joke.question} punchLine={joke.punchLine} />) 
    return (
      <div>
        {jokeComponents}            
      </div>
    )
}
```

## Class Based Compenents

Using ES6 classes.

```jsx
// functional Component
function App() {
  return (
    <div>
      <h1>Code goes here</h1>
    </div>
  )
}

// class based Component
class App extends React.Component {
  render() {
    <div>
      <h1>Code goes here</h1>
    </div>
  }
}
```

Any sort of logic like calculating time of date or formating data can go inside the `render` method or in a class method.

```jsx
// ...
yourMethodHere() {

}

render() {
  const style = this.yourMethodHere()
// ...
```

When using `props` in the `render` method, we need to use `this.props`.
