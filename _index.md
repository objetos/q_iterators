---
bookCollapseSection: true
title: "Iterators"
weight: 2
draft: true
---

Iterators offer a concise and expressive way to traverse a quadrille, especially when combined with filters. Both [for...of]({{< relref for_of >}}) and [visit]({{< relref visit >}}) rely on the lazy [cells()]({{< relref cells >}}) generator to yield filtered cells, enabling functional-style iteration without the verbosity and pitfalls of manual indexing.

## Manual Iteration Using Nested Loops

The classic approach to traversing a grid involves nested `for` loops:

```js
function fx(row, col) {
  /* fx body */
}

for (let row = 0; row < quadrille.height; row++) {
  for (let col = 0; col < quadrille.width; col++) {
    fx(row, col);
  }
}
```

While straightforward, this pattern is prone to [off-by-one](https://en.wikipedia.org/wiki/Off-by-one_error) and [indexing errors](https://en.wikipedia.org/wiki/Array_data_structure#Indexing), and doesn't support filtering without additional logic. It's a good fallback—but modern alternatives are often cleaner and safer.

{{< callout type="warning" >}}  
For quick prototypes or direct indexing, manual loops are still fine. But for filtering, clarity, and maintainability, use the iterator methods.
{{< /callout >}}

## Method Overview

- [`for...of`]({{< relref for_of >}}): modern, efficient, and expressive. Supports full control flow (`break`, `continue`, etc.).
- [`visit(callback, filter?)`]({{< relref visit >}}): syntactic sugar for `for...of`, perfect for inline [arrow functions](https://www.w3schools.com/Js/js_arrow_function.asp).
- [`cells(filter?)`]({{< relref cells >}}): low-level generator behind both methods. Rarely used directly—unless you're chaining with [`map`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map), [`reduce`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce), or [`Array.from`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/from).