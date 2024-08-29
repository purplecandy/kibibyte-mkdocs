---
date: 2024-08-30
authors: [nadeem]
title: "Why Async Functions Don't Belong in Promise Executors"
description: >
  Understanding why passing an async function to the `new Promise` constructor is a mistake
categories:
  - JavaScript
  - Pitfalls
comments: true
---

When working with JavaScript, you might feel inclined to use an async function inside a Promise executor. While this may appear convenient, it is actually discouraged for several reasons. Doing so can lead to subtle bugs and potential memory leaks. Let's delve into why this practice is problematic.


### Uncaught Exceptions

One of the main issues with using an `async` function in a `Promise` executor is that exceptions thrown inside the `async` function are automatically converted into rejected promises. 

However, since the `Promise` constructor is designed to handle synchronous code, it doesn't catch these rejections.

**Example:**
```javascript
new Promise(async (resolve, reject) => {
  throw new Error("This error is not caught!");
});
```
In this example, the error is not caught by the `Promise` constructor, leading to an unhandled rejection or worse the error will be lost.


### But why does this happen?

In the early days of JavaScript, asynchronous operations were handled using callbacks. The `Promise` constructor is callback-based, whereas the `async function` is return-based. Although both `async/await` and Promises are used for handling asynchronous code, they operate differently under the hood.

Here’s a simple table showing the difference between `Promise` and `async function`:

| Method  | Promise  | async function  |
|---------|----------|-----------------|
| Data    | resolve  | return          |
| Error   | reject   | throw           |

When you use an `async function` inside a `Promise` constructor, if you return data or throw an error, the `Promise` constructor does not recognize that the asynchronous operation is complete. This is because the `Promise` constructor expects the provided callbacks—`resolve` and `reject`—to be invoked to indicate the completion of the operation.

**Example:**

Here’s a simple example that many developers might try at some point in their careers:

```javascript
new Promise(async function() {
    await delay(...);
    throw new Error(...);
});
```

At first glance, nothing seems wrong with this code, but in reality, the promise will never be resolved, and the error will never be caught. This is because the `async function` is implicitly returning a promise, but the `Promise` constructor is not aware of it. The code above essentially translates into the following:

```javascript
new Promise(function() {
    return delay(...).then(function() {
        throw new Error(...);
    });
});
```

In this translation, it’s clear that the error is inside another asynchronous callback, and since we aren't using the provided `resolve` and `reject` callbacks, the promise does not know what is happening inside the executor function.

### You may not need to `await` inside a `new Promise`

It’s generally unnecessary to use `await` inside a `new Promise` constructor because promises are just chained callbacks. Once the initial promise is resolved, any other promises in the chain will get resolved. 

Here’s an example of incorrect code:
```javascript hl_lines="11-13"
const foo = new Promise(async (resolve, reject) => {
  readFile('foo.txt', function(err, result) {
    if (err) {
      reject(err);
    } else {
      resolve(result);
    }
  });
});

const result = new Promise(async (resolve, reject) => {
  resolve(await foo);
});
```

The **correct** way to do this would be
```javascript hl_lines="11 15" 
const foo = new Promise((resolve, reject) => {
  readFile('foo.txt', function(err, result) {
    if (err) {
      reject(err);
    } else {
      resolve(result);
    }
  });
});

const result = Promise.resolve(foo);

// An async wait implementation
const getFoo = async () => {
    const result = await foo()
}
```

In some cases, you might find it easier to use an async function as a Promise executor, especially when you need to introduce an artificial delay or handle asynchronous operations.

Instead of using async directly in the Promise constructor, it’s better to refactor your code by separating the asynchronous logic into another function. This keeps your promises clean and avoids pitfalls associated with mixing async/await and Promise constructors.

Example of **incorrect** code

```javascript
new Promise(async function(resolve) {
    await delay(...);
    resolve("Lorem Ipsum")
});
```

Example of **correct** code

```javascript
new Promise((resolve) => {
    async function delayedResolve(resolve) {
        await delay(...);
        resolve("Lorem Ipsum")
    }
    delayedResolve()
});
```

### Conclusion

The key takeaway is that while `async/await` and Promises are closely related, they are not interchangeable within the `Promise` constructor. The `Promise` constructor expects synchronous code that explicitly uses `resolve` and `reject` to indicate completion, whereas `async functions` return promises that resolve or reject implicitly. Misusing these can lead to unhandled errors and unresolved promises, which can cause bugs in your code.



[^1]: I'm writing this to share my recent with JavaScript promises and a note for myself in future.
[^2]: JavaScript, Promises, Async/Await, Asynchronous Programming, Error Handling, Code Best Practices, Promise Executor, JavaScript Tips, Programming Pitfalls, Code Refactoring, Async Functions, JavaScript Performance, Callback Functions, Software Development