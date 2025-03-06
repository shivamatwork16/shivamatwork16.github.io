+++
date = '2025-03-01T18:18:30+05:30'
draft = false
title = 'Unveiling the Power of Promise.allSettled'
tags = ["Node.js", "Promises", "Async/Await", "JavaScript"]
+++

## Unveiling the Power of Promise.allSettled

<!--more-->
In the world of asynchronous JavaScript, especially within the Node.js environment, Promises have become an indispensable tool. They provide a clean and efficient way to handle asynchronous operations, making code more readable and maintainable. While `Promise.all` has been a staple for managing multiple Promises, a lesser-known but equally powerful sibling, `Promise.allSettled`, deserves our attention.

### Understanding `Promise.all` vs. `Promise.allSettled`

Before diving into `Promise.allSettled`, let's briefly revisit `Promise.all`. `Promise.all` takes an array of Promises and returns a single Promise that resolves when **all** of the input Promises have resolved. However, if even one Promise rejects, `Promise.all` immediately rejects, short-circuiting the entire operation. This behavior can be problematic when you need to know the outcome of all Promises, regardless of individual rejections.

Enter `Promise.allSettled`. Introduced in ES2020, `Promise.allSettled` also takes an array of Promises and returns a single Promise that resolves when **all** input Promises have settled, meaning they have either resolved or rejected. Importantly, `Promise.allSettled` never rejects.

### How `Promise.allSettled` Works

When the returned Promise from `Promise.allSettled` resolves, it provides an array of result objects. Each result object corresponds to an input Promise and has a `status` property, which is either `"fulfilled"` or `"rejected"`.

* For fulfilled Promises, the result object also contains a `value` property with the resolved value.
* For rejected Promises, the result object contains a `reason` property with the rejection reason.

Here's a simple Node.js example:

```javascript
const promise1 = Promise.resolve(3);
const promise2 = new Promise((resolve, reject) => setTimeout(reject, 100, 'foo'));
const promises = [promise1, promise2];

Promise.allSettled(promises).then((results) => {
  results.forEach((result) => {
    if (result.status === 'fulfilled') {
      console.log('Fulfilled:', result.value);
    } else {
      console.log('Rejected:', result.reason);
    }
  });
});
```

### Use Cases in Node.js

`Promise.allSettled` is particularly useful in scenarios where you need to:

1.  **Collect Results from Multiple Independent Operations:** When running several independent tasks concurrently, you might want to know the outcome of each task, even if some fail.
2.  **Log or Report on All Operations:** You can use `Promise.allSettled` to log or report on the success or failure of multiple operations, providing a comprehensive overview.
3.  **Perform Cleanup Tasks:** Even if some Promises reject, you might need to perform cleanup tasks based on the results of other Promises.

### Benefits

* **Robustness:** Prevents a single Promise rejection from derailing the entire operation.
* **Comprehensive Results:** Provides detailed information about the outcome of all Promises.
* **Improved Error Handling:** Simplifies error handling by allowing you to process rejections alongside resolutions.

### Conclusion

`Promise.allSettled` is a valuable addition to the Node.js developer's toolkit. By understanding its behavior and use cases, you can write more robust and reliable asynchronous code. Remember that while `Promise.all` is suitable for scenarios where all Promises must succeed, `Promise.allSettled` shines when you need to know the outcome of all Promises, regardless of individual failures.
