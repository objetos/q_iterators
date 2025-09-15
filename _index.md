---
bookCollapseSection: true
title: Iterators
weight: 2
draft: false
---

The **`visit` family** offers a single high-level mechanism to traverse a quadrille in **row-major** order. The callback receives `{ row, col, value }`, may read or mutate cells, and iteration **stops early** if the callback returns `false`. Traversal can apply to **all cells**, or be restricted by a **collection** of allowed values or a **predicate** function, encouraging a [declarative programming](https://en.wikipedia.org/wiki/Declarative_programming) approach—*specify what to process, not how to loop*.

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

While straightforward and likely familiar, this pattern is prone to [off-by-one](https://en.wikipedia.org/wiki/Off-by-one_error) and [indexing errors](https://en.wikipedia.org/wiki/Array_data_structure#Indexing). It also requires extra logic for filtering. It’s a good fallback—but modern alternatives are cleaner and safer.

## Method Overview

* [`visit(callback)`]({{< relref visit >}}): iterates all cells in row-major order. Return `false` to stop early.
* [`visit(callback, collection)`]({{< relref "visit_collection" >}}): iterates only cells whose value is in the given `Array` or `Set`.
* [`visit(callback, predicate)`]({{< relref "visit_predicate" >}}): iterates only cells where the predicate function returns `true`.