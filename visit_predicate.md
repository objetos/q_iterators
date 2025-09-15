---
weight: 3
title: visit(callback, predicate)
---

Iterates cells in **row-major** order and calls `callback({ row, col, value })` for each cell where `predicate({ row, col, value })` returns `true`. Return `false` from the callback to stop early. Reading and mutating the grid inside the callback is allowed.

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

* The `predicate` decides **which cells are visited** (e.g., only `🐸` in the current column).
* The `callback` defines **what to do** with those visited cells (e.g., fill them with semi-transparent color).
{{< /callout >}}

## Syntax

```js
visit(callback, predicate)
```

## Parameters

| Param       | Description                                                                                                  |
| ----------- | ------------------------------------------------------------------------------------------------------------ |
| `callback`  | Receives `{ row, col, value }`. Return `false` to stop early; any other return is ignored                    |
| `predicate` | A function `({ row, col, value }) => boolean`. Only cells for which the predicate returns `true` are visited |