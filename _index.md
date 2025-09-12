---
bookCollapseSection: true
title: Iterators
weight: 2
draft: false
---

Iterators offer a concise and expressive way to traverse a quadrille, especially when combined with filters. Both [`for...of`]({{< relref for_of >}}) and [`visit`]({{< relref visit >}}) rely on the lazy [`cells()`]({{< relref cells >}}) generator to yield filtered cells, enabling functional-style iteration without the verbosity and pitfalls of manual indexing.

Filter functions can access the `{ row, col, value }` cell data, allowing you to express complex cell selections in a single inline predicate.

## Manual Iteration Using Nested Loops

The [imperative programming](https://en.wikipedia.org/wiki/Imperative_programming) style to traversing a grid—often seen with 2D arrays or matrices—involves nested `for` loops:

```js
function callback(row, col) {
  /* callback body */
}

for (let row = 0; row < quadrille.height; row++) {
  for (let col = 0; col < quadrille.width; col++) {
    callback(row, col);
  }
}
```

While straightforward and likely familiar, this pattern is prone to [off-by-one](https://en.wikipedia.org/wiki/Off-by-one_error) and [indexing errors](https://en.wikipedia.org/wiki/Array_data_structure#Indexing), and doesn't support filtering without additional logic. It's a good fallback—but modern alternatives are often cleaner and safer.

{{< callout type="warning" >}}  
For quick prototypes or direct indexing, manual loops are still fine. But for filtering, clarity, and maintainability, use the iterator methods introduced here.
{{< /callout >}}

## Method Overview

* [`for...of`]({{< relref for_of >}}): modern, efficient, and expressive. Supports full control flow (`break`, `continue`, etc.).
* [`visit(callback, filter?)`]({{< relref visit >}}): syntactic sugar for `for...of`, perfect for inline [arrow functions](https://www.w3schools.com/Js/js_arrow_function.asp). Also supports full cell predicates.
* [`cells(filter?)`]({{< relref cells >}}): generator used by both methods for yielding filtered cells. Use directly when chaining with methods like [`map`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map), [`reduce`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce), or [`Array.from`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/from).