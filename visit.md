---
weight: 1
title: visit(callback)
---

Iterates **all cells** in row-major order and calls `callback({ row, col, value })` for each. Return `false` from the callback to stop early. Reading and mutating the grid inside the callback is allowed.

## Example

(click to randomly refill `q` with `emojis`)
{{< p5-global-iframe quadrille="true" width="425" height="425" >}}
'use strict';
Quadrille.cellLength = 50;
let q;
const emojis = ['🐸', '🐯', '🐱', '🐶', '🐮'];

function setup() {
  createCanvas(8 * Quadrille.cellLength, 8 * Quadrille.cellLength);
  q = createQuadrille(8, 8);
  update();
}

function draw() {
  background('#DAF7A6');
  drawQuadrille(q);
}

function mousePressed() {
  update();
}

function update() {
  q.visit(({ row, col }) => q.fill(row, col, random(emojis)));
}
{{< /p5-global-iframe >}}

{{% details title="code" open=true %}}
```js
Quadrille.cellLength = 50;
let q;
const emojis = ['🐸', '🐯', '🐱', '🐶', '🐮'];

function setup() {
  createCanvas(8 * Quadrille.cellLength, 8 * Quadrille.cellLength);
  q = createQuadrille(8, 8);
  update();
}

function draw() {
  background('#DAF7A6');
  drawQuadrille(q);
}

function mousePressed() {
  update();
}

function update() {
  q.visit(({ row, col }) => q.fill(row, col, random(emojis)));
}
```
{{% /details %}}

{{< callout type="info" >}}

* All cells are visited in row-major order.
* The `callback` decides what to do with each cell (e.g., refill with a random emoji).
{{< /callout >}}

## Syntax

```js
visit(callback)
```

## Parameters

| Param      | Description                                                                               |
| ---------- | ----------------------------------------------------------------------------------------- |
| `callback` | Receives `{ row, col, value }`. Return `false` to stop early; any other return is ignored |
