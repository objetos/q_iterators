---
weight: 2
title: for...of
---

Loops all cells (i.e., defined as an object literal {row, col, value}) in the `quadrille`. Only cells passing the cells(filter) condition are yielded. Pass no filter to collect all cells.

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
  q = createQuadrille(8, 8).rand(15, '🐸').rand(15, '🐯').rand(15, '🐮');
  hint = createQuadrille(8, 8);
  for (const cell of q.cells(value => Quadrille.isFilled(value))) {
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
  q = createQuadrille(8, 8).rand(15, '🐸').rand(15, '🐯').rand(15, '🐮');
  hint = createQuadrille(8, 8);
  for (const cell of q.cells(value => Quadrille.isFilled(value))) {
    hint.fill(cell.row, cell.col, color(0, 140));
  }
}
```
{{% /details %}}

{{< callout type="info" >}}  
Also possible would have been simply `q.cells(Quadrille.isFilled)` which is equivalent to: `q.cells(value => Quadrille.isFilled(value)))`
{{< /callout >}}

## Example: Filtering by Predicate Value, Row, and Column

(move mouse to highlight 🐸 in the hovered column, and click to randomize `q`)  
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
  for (const cell of q.cells({ value: v => v === '🐸', 
                               col: c => c === q.mouseCol })) {
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
  for (const cell of q.cells({ value: v => v === '🐸', 
                               col: c => c === q.mouseCol })) {
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

| Param | Description                                                                         |
|-------|-------------------------------------------------------------------------------------|
| `fx`  | Function: A function of the form `fx(row, col)` to be executed on all visited cells |