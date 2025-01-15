# Difference between Promise and Class Callback Handler

## Code 1: Promise Example

```javascript
const promise = new Promise((resolve, reject) => {
  console.log(1);
  resolve();
  console.log(2);
});
promise.then(() => {
  console.log(3);
});
console.log(4);
```

## Execution Flow:

### Promise Execution:

1.**When you create a new Promise**:
   - The executor function (the function inside the `new Promise()` constructor) runs immediately and synchronously.

2. **Inside the executor**:
   - `console.log(1)` is executed synchronously.
   - `resolve()` resolves the promise, but the `.then()` callback is not executed immediately.
   - `console.log(2)` is executed synchronously after `resolve()`.

### Promise Resolution:

- The `.then()` callback is added to the **microtask queue** after the promise is resolved.

### Event Loop:

- After the synchronous code finishes executing, the JavaScript engine processes the **microtask queue**.
- The `.then()` callback is executed, and `console.log(3)` is printed as part of the `.then()` callback.

### Logging 4:

- After the `.then()` callback is processed, `console.log(4)` is executed.

### Final Output:
```
1
2
4
3
```

## Code 2: Class Example

```javascript
class CallbackHandler {
  constructor(callback) {
    this.callback = callback;
  }

  executeCallback() {
    this.callback();
  }
}
```

## Execution Flow:

1. **Class Instantiation**:
   - The constructor is called immediately, and the callback is stored.
   - `console.log(4)` is executed synchronously.

2. **Callback Execution**:
   - The callback is executed when `executeCallback()` is called.
   - `console.log(1)` and `console.log(2)` are executed synchronously.

### Final Output:
```
4
1
2
```

### Key Differences:

- **Synchronous vs Asynchronous**:

  - **Promise Example**: The code inside the promise executor runs synchronously, but the .then() callback is added to the microtask queue and is executed asynchronously after the synchronous code finishes.
  - **Class Example**: The callback is stored synchronously and doesn't execute unless explicitly invoked.

- **Microtask Queue**:
  - **Promise Example**: The .then() callback is added to the microtask queue and executed asynchronously after synchronous code execution.
  - **Class Example**: There is no microtask queue involved; the callback is executed only when explicitly invoked.
