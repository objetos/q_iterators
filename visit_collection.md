---
weight: 2
title: visit(callback, collection)
---

Iterates cells in **row-major** order and calls `callback({ row, col, value })` for each cell whose value is contained in `collection` (`Array` or `Set`). Return `false` from the callback to stop early. Reading and mutating the grid inside the callback is allowed.

## Example

(move mouse to highlight `🐸` filled cells in the hovered col, and click to randomize `q`)
{{< p5-global-iframe quadrille="true" width="425" height="425" >}}
'use strict';

Quadrille.cellLength = 50;
let q, hint;

const monkeys = ['🙈', '🙉', '🙊', '🦧'];
const birds = ['🐍', '🦜', '🦚', '🐤'];
const emojis = [...monkeys, ...birds, '🐸', '🐯', '🐱', '🐶', '🐮'];

function setup() {
  createCanvas(8 * Quadrille.cellLength, 8 * Quadrille.cellLength);
  highlight();
}

function draw() {
  background('#DAF7A6');
  drawQuadrille(q);
  drawQuadrille(hint);
}

function mousePressed() {
  highlight();
}

function highlight() {
  q = createQuadrille(8, 8);
  // fill q with random emojis
  q.visit(({ row, col }) => q.fill(row, col, random(emojis)));
  hint = createQuadrille(8, 8);
  // mark monkey positions
  q.visit(({ row, col }) => hint.fill(row, col, color(0, 140)), monkeys);
}
{{< /p5-global-iframe >}}

{{% details title="code" open=true %}}
```js
Quadrille.cellLength = 50;
let q, hint;

const monkeys = ['🙈', '🙉', '🙊', '🦧'];
const birds = ['🐍', '🦜', '🦚', '🐤'];
const emojis = [...monkeys, ...birds, '🐸', '🐯', '🐱', '🐶', '🐮'];

function setup() {
  createCanvas(8 * Quadrille.cellLength, 8 * Quadrille.cellLength);
  highlight();
}

function draw() {
  background('#DAF7A6');
  drawQuadrille(q);
  drawQuadrille(hint);
}

function mousePressed() {
  highlight();
}

function highlight() {
  q = createQuadrille(8, 8);
  // fill q with random emojis
  q.visit(({ row, col }) => q.fill(row, col, random(emojis)));
  hint = createQuadrille(8, 8);
  // mark monkey positions
  q.visit(({ row, col }) => hint.fill(row, col, color(0, 140)), monkeys);
}
```
{{% /details %}}

{{< callout type="info" >}}
* The first loop fills all cells with random emojis using [visit(callback)]({{< relref visit >}}).
* The second loop visits only monkey cells, marking them in a separate quadrille.
{{< /callout >}}

## Syntax

```js
visit(callback, collection)
```

## Parameters

| Param        | Description                                                                               |
| ------------ | ----------------------------------------------------------------------------------------- |
| `callback`   | Receives `{ row, col, value }`. Return `false` to stop early; any other return is ignored |
| `collection` | An `Array` or `Set` of values. Only cells whose value is in the collection are visited    |