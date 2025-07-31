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

(move mouse to highlight `🐸` filled cells in the hovered col, and click to randomize `q`)
{{< p5-global-iframe quadrille="true" width="425" height="425" >}}
'use strict';
Quadrille.cellLength = 50;
let q, hint;

function setup() {
  createCanvas(8 * Quadrille.cellLength, 8 * Quadrille.cellLength);
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
  q.visit(
    ({ row, col }) => hint.fill(row, col, color(0, 140)),
    ({ value, col }) => value === '🐸' && col === q.mouseCol
  );
}
{{< /p5-global-iframe >}}

{{% details title="code" open=true %}}
```js
Quadrille.cellLength = 50;
let q, hint;

function setup() {
  createCanvas(8 * Quadrille.cellLength, 8 * Quadrille.cellLength);
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
  q.visit(
    ({ row, col }) => hint.fill(row, col, color(0, 140)),
    ({ value, col }) => value === '🐸' && col === q.mouseCol
  );
}
```
{{% /details %}}

{{< callout type="info" >}}
This example is a direct adaptation of the corresponding [`for...of` example]({{< relref "for_of#example-filtering-by-combined-predicates-value-row-col" >}}).
Additional examples from the `for...of` section can be rewritten using `visit` following the same pattern.
{{< /callout >}}

## Syntax

```js
visit(callback, filter?)
```

## Parameters

| Param      | Description                                                                                                                                                                                                        |
| ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `callback` | Function to execute on each matching cell. Receives a `{ row, col, value }` object as argument                                                                                                                     |
| `filter`   | Optional filter to restrict cells visited. It can be:<ul><li>a value collection (`Array` or `Set`)</li><li>a predicate function (`({ row, col, value }) => boolean`)</li></ul> If omitted, all cells are included |