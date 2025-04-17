---
weight: 1
title: for...of
---

Iterates lazily in row-major order (top to bottom, left to right) over all matching cells in a `quadrille`. Each iteration yields a cell object of the form `{ row, col, value }`. This pattern mirrors JavaScript’s [`for...of`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...of), which is used to loop over iterable objects like arrays and generators.

To yield only a subset of cells, use [quadrille.cells(filter)]({{< relref cells >}}) within the loop. The [visit(callback, filter?)]({{< relref visit >}}) method offers a concise alternative by applying a `callback` function to each yielded cell.

## Example: Filtering by Value Collection

(click on canvas to randomize `q`)  
{{< p5-global-iframe quadrille="true" width="425" height="425" >}}
'use strict';
Quadrille.cellLength = 50;
let q, hint;

// Emoji sets to populate and filter the quadrille
const monkeys = ['🙈', '🙉', '🙊', '🦧'];
const birds = ['🐍', '🦜', '🦚', '🐤'];
// Use spread operator to aggregate arrays
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
  // Fill the main quadrille q with random emojis
  q = createQuadrille(8, 8);
  // Iterates all cells in q
  for (const { row, col } of q) {
    q.fill(row, col, random(emojis));
  }
  // Create a new quadrille to highlight monkey cells
  hint = createQuadrille(8, 8);
  // Iterates only cells containing monkeys
  for (const { row, col } of q.cells(monkeys)) {
    // Highlight selected cells with translucent black
    hint.fill(row, col, color(0, 140));
  }
}
{{< /p5-global-iframe >}}

{{% details title="code" open=true %}}
```js
Quadrille.cellLength = 50;
let q, hint;

// Emoji sets to populate and filter the quadrille
const monkeys = ['🙈', '🙉', '🙊', '🦧'];
const birds = ['🐍', '🦜', '🦚', '🐤'];
// Use spread operator to aggregate arrays
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
  // Fill the main quadrille q with random emojis
  q = createQuadrille(8, 8);
  // Iterates all cells in q
  for (const { row, col } of q) {
    q.fill(row, col, random(emojis));
  }
  // Create a new quadrille to highlight monkey cells
  hint = createQuadrille(8, 8);
  // Iterates only cells containing monkeys
  for (const { row, col } of q.cells(monkeys)) {
    // Highlight selected cells with translucent black
    hint.fill(row, col, color(0, 140));
  }
}
```
{{% /details %}}

{{< callout type="info" >}}  
This example shows two uses of `for...of`:  
- The first loop (`for...of q`) iterates **all cells** in the quadrille to randomly fill them with emojis.  
- The second loop (`for...of q.cells(monkeys)`) uses a **value array filter** to select only monkey emoji cells to be highlighted.  
Both loops access the cell `{ row, col }` directly through [JavaScript object destructuring](https://www.w3schools.com/js/js_destructuring.asp).  
{{< /callout >}}

{{< callout type="info" >}}  
The [spread operator](https://www.w3schools.com/howto/howto_js_spread_operator.asp) `...` is used to **combine multiple arrays** into one.  
```js
const monkeys = ['🙈', '🙉', '🙊', '🦧'];
const birds = ['🐍', '🦜', '🦚', '🐤'];
const emojis = [...monkeys, ...birds, '🐸', '🐯', '🐱', '🐶', '🐮'];
```
This creates a new array containing all monkey and bird emojis, followed by additional individual emojis.
{{< /callout >}}

## Example: Filtering by Predicate Value

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
  // Fill q with random 🐸, 🐯 and 🐮 emojis, 15 each
  q = createQuadrille(8, 8).rand(15, '🐸').rand(15, '🐯').rand(15, '🐮');
  hint = createQuadrille(8, 8); 
  // Iterate only over filled cells in q using for...of and a predicate filter
  for (const cell of q.cells(Quadrille.isFilled)) {
    // Highlight filled cells with a translucent overlay
    hint.fill(cell.row, cell.col, color(0, 140));
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
  // Fill q with random 🐸, 🐯 and 🐮 emojis, 15 each
  q = createQuadrille(8, 8).rand(15, '🐸').rand(15, '🐯').rand(15, '🐮');
  hint = createQuadrille(8, 8); 
  // Iterate only over filled cells in q using for...of and a predicate filter
  for (const cell of q.cells(Quadrille.isFilled)) {
    // Highlight filled cells with a translucent overlay
    hint.fill(cell.row, cell.col, color(0, 140));
  }
}
```
{{% /details %}}

{{< callout type="info" >}}  
The expression `q.cells(Quadrille.isFilled)` is equivalent to `q.cells(value => Quadrille.isFilled(value))`. This shorthand works because [`Quadrille.isFilled`]({{< relref is_filled >}}) already expects a single argument.  
{{< /callout >}}

## Example: Filtering by Predicate Value, Row, and Column

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
  // Iterate over all 🐸 cells in the current mouse column
  for (const cell of q.cells({ 
    value: v => v === '🐸',    // Only 🐸 values
    col: c => c === q.mouseCol // Only current column
  })) {
    // Fill the hint cell with translucent black for visual feedback
    hint.fill(cell.row, cell.col, color(0, 140));
  }
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
  // Iterate over all 🐸 cells in the current mouse column
  for (const cell of q.cells({ 
    value: v => v === '🐸',    // Only 🐸 values
    col: c => c === q.mouseCol // Only current column
  })) {
    // Fill the hint cell with translucent black for visual feedback
    hint.fill(cell.row, cell.col, color(0, 140));
  }
}
```
{{% /details %}}

## Syntax

> `for(const cell of q)`  
> `for(const cell of q(filter))`  
> `for(const ({ row, col, value }) of q)`  
> `for(const ({ row, col, value }) of q(filter))`  

## Parameters

| Param    | Description |
|----------|-------------|
| `filter` | Restricts which cells are visited. All cells are included if this parameter is omitted or `undefined`. It can be: <ul><li>a value collection (`Array` or `Set`)</li><li>a predicate function (`value => boolean`)</li><li>an object with optional `value`, `row`, and/or `col` predicates</li></ul> |