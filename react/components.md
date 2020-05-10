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
