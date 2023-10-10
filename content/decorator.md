# Decorators in JavaScript

In this article, we will learn about decorators in JavaScript using a common task that most Javascript developers would have.

## What is a decorator?

A decorator is a function that takes another function and extends the behavior of the latter function without explicitly modifying it. They key point is that a decorator function returns a modified version of the original function.

## Decorating a function

Let's say we have a function that adds two numbers together.

```js

const addFn = (a, b) => {
  return a + b;
}

// We will now create a decorator that logs the arguments of the addFn function
const addLogs = (fn) => {
  return (...args) => {
    console.log(`Arguments: ${args}`);
    return fn(...args);
  }
}

// The normal log function
add(1, 2); // 3

const addWithLogs = addLogs(addFn);

// the number logging functionality is now added to the addFn function
addWithLogs(1, 2); // Arguments: 1, 2
                   // 3

```