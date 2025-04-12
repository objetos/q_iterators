---
bookCollapseSection: true
title: "visitQuadrille(args)"
weight: 3
draft: false
---

The `visitQuadrille` p5 function is designed to iterate over `quadrille` cells and execute a specified `fx` function on each cell. It provides a concise, less error-prone approach that aligns well with functional programming principles.

## Manual Iteration Using Nested Loops

The first and most familiar way to iterate over a `quadrille` is with standard `for` loops, a common method used for looping through matrices:

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

This approach, while widely understood, requires precise indexing of `row` and `col`, which can sometimes lead to errors, particularly in setting index limits.

## Iteration Using `for...of` Loops

A more modern approach uses a [for...of loop](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...of) since a `quadrille` is an [iterable object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols#the_iterable_protocol). Each `cell` is an object with `value`, `row`, and `col` properties, enabling direct access to its position:

```js
function fx(row, col) {
  /* fx body */
}

for (let cell of quadrille) {
  fx(cell.row, cell.col);
}
```

This method simplifies access to `row` and `col` without explicit indexing, reducing errors. However, it still requires manual iteration logic, which can be cumbersome in more complex scenarios.

## Simplified Iteration with `visitQuadrille`

The `visitQuadrille` function makes it easy to apply a function (`fx`) to each cell in a `quadrille`. By removing the need for manual indexing, it simplifies the process and reduces the chance of errors. Additionally, `visitQuadrille` supports an optional `values` array to selectively process specific cells, adding flexibility.

```js
function fx(row, col) {
  /* fx body */
}

visitQuadrille(quadrille, fx, values);
```

This approach keeps the code clean and allows the logic to be reused and organized effectively. The optional `values` array makes it possible to limit iteration to specific cell values.

## Concise Iteration with `visitQuadrille`

The `visitQuadrille` function also supports inline definitions of the `fx` function using modern [arrow functions](https://www.w3schools.com/js/js_arrow_function.asp). These anonymous functions enable a more concise and [declarative](https://en.wikipedia.org/wiki/Declarative_programming) style, removing the need for explicit loops:

```js
visitQuadrille(quadrille, (row, col) => { /* fx body */ }, values);
```

This concise approach is preferred for its simplicity, reducing potential errors and improving readability while fully leveraging modern JavaScript features.