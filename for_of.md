---
weight: 1
title: for...of
---

Iterates lazily in row-major order (top to bottom, left to right) over all matching cells in a `quadrille`. Each iteration yields a cell object of the form `{ row, col, value }`. This pattern mirrors JavaScript’s [`for...of`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...of), which is used to loop over iterable objects like arrays and generators.

To yield only a subset of cells, use the [`cells(filter)`]({{< relref cells >}}) method within the loop. The [`visit(callback, filter?)`]({{< relref visit >}}) method offers a concise alternative by applying a `callback` function to each yielded cell.

## Example: Filtering by Value Collection

(click on canvas to randomize `q`)  
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
  for (const { row, col } of q) {
    q.fill(row, col, random(emojis));
  }
  hint = createQuadrille(8, 8);
  for (const { row, col } of q.cells(monkeys)) {
    hint.fill(row, col, color(0, 140));
  }
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
  for (const { row, col } of q) {
    q.fill(row, col, random(emojis));
  }
  hint = createQuadrille(8, 8);
  for (const { row, col } of q.cells(monkeys)) {
    hint.fill(row, col, color(0, 140));
  }
}
```
{{% /details %}}

{{< callout type="info" >}}  
* The first loop (`for (const { row, col } of q)`) iterates **all cells** to fill them randomly.
* The second loop (`for (const { row, col } of q.cells(monkeys))`) filters cells containing monkey emojis.
{{< /callout >}}

## Example: Filtering by Predicate (Value)

(click on canvas to randomize `q`)
{{< p5-global-iframe quadrille="true" width="425" height="425" >}}
'use strict';
Quadrille.cellLength = 50;
let q, hint;

function setup() {
  createCanvas(8 * Quadrille.cellLength, 8 * Quadrille.cellLength);
  highlight();
}

function draw() {
  background('#D6EAF8');
  drawQuadrille(q);
  drawQuadrille(hint);
}

function mousePressed() {
  highlight();
}

function highlight() {
  q = createQuadrille(8, 8).rand(15, '🐸').rand(15, '🐯').rand(15, '🐮');
  hint = createQuadrille(8, 8);
  for (const { row, col } of q.cells(({ value }) => Quadrille.isFilled(value))) {
    hint.fill(row, col, color(0, 140));
  }
}
{{< /p5-global-iframe >}}

{{% details title="code" open=true %}}
```js
Quadrille.cellLength = 50;
let q, hint;

function setup() {
  createCanvas(8 * Quadrille.cellLength, 8 * Quadrille.cellLength);
  highlight();
}

function draw() {
  background('#D6EAF8');
  drawQuadrille(q);
  drawQuadrille(hint);
}

function mousePressed() {
  highlight();
}

function highlight() {
  q = createQuadrille(8, 8).rand(15, '🐸').rand(15, '🐯').rand(15, '🐮');
  hint = createQuadrille(8, 8);
  for (const { row, col } of q.cells(({ value }) => Quadrille.isFilled(value))) {
    hint.fill(row, col, color(0, 140));
  }
}
```
{{% /details %}}

## Example: Filtering by Combined Predicates (Value, Row, Col)

(move mouse to highlight 🐸 in hovered column; click to randomize `q`)
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
  for (const { row, col } of q.cells(({ value, col }) => value === '🐸' && col === q.mouseCol)) {
    hint.fill(row, col, color(0, 140));
  }
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
  for (const { row, col } of q.cells(({ value, col }) => value === '🐸' && col === q.mouseCol)) {
    hint.fill(row, col, color(0, 140));
  }
}
```
{{% /details %}}

## Syntax

```js
for (const cell of q)
for (const cell of q.cells(filter))
for (const { row, col, value } of q)
for (const { row, col, value } of q.cells(filter))
```

## Parameters

| Param    | Description                                                                                                                                                                                                     |
| -------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `filter` | Optional filter to restrict cells visited. Can be:<ul><li>A value collection (`Array` or `Set`)</li><li>A predicate function (`({ row, col, value }) => boolean`)</li></ul> If omitted, all cells are included |