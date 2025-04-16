---
weight: 2
title: visit(callback, filter?)
---

A [syntactic sugar](https://en.wikipedia.org/wiki/Syntactic_sugar) for the [`for...of`]({{< relref for_of >}}) loop. Internally defined as:

```js
visit(callback, filter) {
  for (const cell of this.cells(filter)) {
    callback(cell);
  }
}
```

It simplifies iteration over cells by calling `callback` on each `{ row, col, value }` cell object. Ideal for concise [arrow functions](https://www.w3schools.com/Js/js_arrow_function.asp) and inline operations.

{{< callout type="warning" >}}  
Use [`for...of`]({{< relref for_of >}}) instead if you need to break early from the loop.
{{< /callout >}}

## Example

(move mouse to highlight 🐸 filled cells in the hovered col, and click to randomize `q`)  
{{< p5-global-iframe quadrille="true" width="425" height="425" >}}
'use strict';
Quadrille.cellLength = 50;
let q, hint;

function setup() {
  createCanvas(8 * Quadrille.cellLength, 8 * Quadrille.cellLength);
  // Create q with random 🐸, 🐯 and 🐮 emojis, 15 each
  q = createQuadrille(8, 8).rand(15, '🐸').rand(15, '🐯').rand(15, '🐮');
  highlight();
}

function draw() {
  background('#D7BDE2');
  drawQuadrille(q);
  drawQuadrille(hint);
}

function mousePressed() {
  q.randomize();
  highlight();
}

function mouseMoved() {
  highlight();
}

function highlight() {
  hint = createQuadrille(8, 8);
  // Visit all 🐸 cells in the current mouse column,
  // filling the hint cell with translucent black for visual feedback
  q.visit(({ row, col }) => hint.fill(row, col, color(0, 140)), {
    value: v => v === '🐸',    // Only 🐸 values
    col: c => c === q.mouseCol // Only current column
  });
}
{{< /p5-global-iframe >}}

{{% details title="code" open=true %}}
```js
Quadrille.cellLength = 50;
let q, hint;

function setup() {
  createCanvas(8 * Quadrille.cellLength, 8 * Quadrille.cellLength);
  // Create q with random 🐸, 🐯 and 🐮 emojis, 15 each
  q = createQuadrille(8, 8).rand(15, '🐸').rand(15, '🐯').rand(15, '🐮');
  highlight();
}

function draw() {
  background('#D7BDE2');
  drawQuadrille(q);
  drawQuadrille(hint);
}

function mousePressed() {
  q.randomize();
  highlight();
}

function mouseMoved() {
  highlight();
}

function highlight() {
  hint = createQuadrille(8, 8);
  // Visit all 🐸 cells in the current mouse column,
  // filling the hint cell with translucent black for visual feedback
  q.visit(({ row, col }) => hint.fill(row, col, color(0, 140)), {
    value: v => v === '🐸',    // Only 🐸 values
    col: c => c === q.mouseCol // Only current column
  });
}
```
{{% /details %}}

{{< callout type="info" >}}  
This example is a direct adaptation of the corresponding [for...of example]({{< relref "for_of#example-filtering-by-predicate-value-row-and-column" >}}).  
Additional examples from the `for...of` section can be rewritten using `visit` following the same pattern.
{{< /callout >}}

## Syntax

> `visit(callback, filter?)`

## Parameters

| Param      | Description                                                                                    |
|------------|------------------------------------------------------------------------------------------------|
| `callback` | Function to execute on each matching cell. Receives a `{ row, col, value }` object as argument |
| `filter`   | Restricts which cells are visited. All cells are included if this parameter is omitted or `undefined`. It can be: <ul><li>a value collection (`Array` or `Set`)</li><li>a predicate function (`value => boolean`)</li><li>an object with optional `value`, `row`, and/or `col` predicates</li></ul> |