# Node JS Notes

>  Node.js is a JavaScript runtime, or an environment that allows us to execute JavaScript code outside of the browser. A “runtime” converts code written in a high-level, human-readable, programming language and compiles it down to code the computer can execute.

## Concepts

### Arrow functions

### Promises

### Async / Await

### `setInterval`/`setTimeout`

### Modules

`require` modules (libraries) to use them.

```js
// Require in the 'events' core module:
const events = require('events');
```

## Predefined objects

### `global`

> The Node.js environment has a global object that contains every Node-specific global property. Access with `global`. It is mutable.

### `process`

> A process is the instance of a computer program that is being executed. Node has a global process object with useful properties. One of these properties is NODE_ENV which can be used in an if/else statement to perform different tasks depending on if the application is in the production or development phase. 

#### `process.argv`

> process.argv is a property that holds an array of command-line values provided when the current process was initiated. The first element in the array is the absolute path to the Node, followed by the path to the file that’s running and finally any command-line arguments provided when the process was initiated.


```js

```


## Modules in Node vs Browser

In JavaScript, there are two runtime environments and each has a preferred module implementation:

- The Node runtime environment and the `module.exports` and `require()` syntax.
- The browser’s runtime environment and the ES6 `import`/`export` syntax.

- https://www.codecademy.com/courses/learn-intermediate-javascript/articles/implementing-modules-using-es-6-syntax
- https://www.codecademy.com/articles/introduction-to-javascript-runtime-environments

---

> Every JavaScript file that runs in a Node environment is treated as a distinct module.

### module.exports

```js
/* converters.js */
function celsiusToFahrenheit(celsius) {
  return celsius * (9/5) + 32;
}

// either way works
module.exports.celsiusToFahrenheit = celsiusToFahrenheit;

module.exports.fahrenheitToCelsius = function(fahrenheit) {
  return (fahrenheit - 32) * (5/9);
};

```

`module.exports` is an object that is built-in to the Node.js runtime environment.

When you use `require()`, the entire `module.exports` object is returned and stored in the variable 

### Object destrcuturing with `require()`

```js
const { celsiusToFahrenheit } = require('./converters.js');
```


### Code modules

> The events module provides EventEmitter objects used to assign listener functions triggered on specified events.

> The buffer module is used to handle binary data.

> The timer module provides the setImmediate() function which runs immediately after the current poll phase is completed. 


---

### Node is often described as having event-driven architecture. 


**What is imperative programming?**

> Imperative programming is a paradigm of computer programming where the program describes steps that change the state of the computer. Unlike declarative programming, which describes "what" a program should accomplish, imperative programming explicitly tells the computer "how" to accomplish it. Programs written this way often compile to binary executables that run more efficiently since all CPU (central processing unit) instructions are imperative statements.

Nodejs heavily uses event emmiters


>  the console.log() method is a “thin wrapper” on the .stdout.write() method of the process object. 

### Receiving user input

> In Node, we can also receive input from a user through the terminal using the stdin.on() method on the process object

```js
process.stdin.on('data', (userInput) => {
  let input = userInput.toString()
  console.log(input)
});
```

> process.stdin is an instance of EventEmitter
