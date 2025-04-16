---
weight: 2
title: for...of
---

Loops all cells (i.e., defined as an object literal {row, col, value}) in the `quadrille`. Only cells passing the cells(filter) condition are yielded. Pass no filter to collect all cells.

## Example: Filtering by Value Collection

(click on canvas and press any key to randomize `q`)  
{{< p5-global-iframe quadrille="true" width="425" height="425" >}}
'use strict';
Quadrille.cellLength = 50;
let q;
const monkeys = ['🙈', '🙉', '🙊'];
const birds = ['🐍', '🦜', '🦚', '🐤'];
const emojis = [...monkeys, ...birds, '🐸', '🐯', '🐱', '🐶', '🐮'];
let monkeyCount, birdCount;

function setup() {
  createCanvas(8 * Quadrille.cellLength, 8 * Quadrille.cellLength);
  q = createQuadrille(8, 8);
  fillQuadrille();
}

function draw() {
  background('#EBDEF0');
  drawQuadrille(q);
  text(`monkey count: ${monkeyCount}, bird count: ${birdCount}`, 20, 30);
}

function mousePressed() {
  fillQuadrille();
}

function fillQuadrille() {
  for (const { row, col } of q) {
    q.fill(row, col, random(emojis));
  }
  monkeyCount = 0;
  for (const cell of q.cells(monkeys)) {
    monkeyCount++;
  }
  birdCount = 0;
  for (const cell of q.cells(birds)) {
    birdCount++;
  }
}
{{< /p5-global-iframe >}}

{{% details title="code" open=true %}}
```js
Quadrille.cellLength = 50;
let q;
const monkeys = ['🙈', '🙉', '🙊'];
const birds = ['🐍', '🦜', '🦚', '🐤'];
const emojis = [...monkeys, ...birds, '🐸', '🐯', '🐱', '🐶', '🐮'];
let monkeyCount, birdCount;

function setup() {
  createCanvas(8 * Quadrille.cellLength, 8 * Quadrille.cellLength);
  q = createQuadrille(8, 8);
  fillQuadrille();
}

function draw() {
  background('#EBDEF0');
  drawQuadrille(q);
  text(`monkey count: ${monkeyCount}, bird count: ${birdCount}`, 20, 30);
}

function mousePressed() {
  fillQuadrille();
}

function fillQuadrille() {
  for (const { row, col } of q) {
    q.fill(row, col, random(emojis));
  }
  monkeyCount = 0;
  for (const cell of q.cells(monkeys)) {
    monkeyCount++;
  }
  birdCount = 0;
  for (const cell of q.cells(birds)) {
    birdCount++;
  }
}
```
{{% /details %}}

## Example: Filtering by Predicate Value

(click on canvas to to replace 🐍 with fuchsia and press any key to randomize `q`)  
{{< p5-global-iframe quadrille="true" width="425" height="425" >}}
'use strict';
Quadrille.cellLength = 50;
let q;
let fuchsia;
let emoji = false;

function setup() {
  createCanvas(8 * Quadrille.cellLength, 8 * Quadrille.cellLength);
  fuchsia = color('Fuchsia');
  q = createQuadrille(8, 8).rand(32, fuchsia);
}

function draw() {
  background('#6495ED');
  drawQuadrille(q);
}

function mousePressed() {
  emoji = !emoji;
  for (const { row, col } of q.cells(value => Quadrille.isFilled(value))) {
    q.fill(row, col, emoji ? '🐍' : fuchsia);
  }
}

function keyPressed() {
  q.randomize();
}
{{< /p5-global-iframe >}}

{{% details title="code" open=true %}}
```js
Quadrille.cellLength = 50;
let q;
let fuchsia;
let emoji = false;

function setup() {
  createCanvas(8 * Quadrille.cellLength, 8 * Quadrille.cellLength);
  fuchsia = color('Fuchsia');
  q = createQuadrille(8, 8).rand(32, fuchsia);
}

function draw() {
  background('#6495ED');
  drawQuadrille(q);
}

function mousePressed() {
  emoji = !emoji;
  for (const { row, col } of q.cells(value => Quadrille.isFilled(value))) {
    q.fill(row, col, emoji ? '🐍' : fuchsia);
  }
}

function keyPressed() {
  q.randomize();
}
```
{{% /details %}}

{{< callout type="info" >}}  
Also possible would have been simply `q.cells(Quadrille.isFilled)` which is equivalent to: `q.cells(value => Quadrille.isFilled(value)))`
{{< /callout >}}

## Example: Filtering by Predicate Value, Row, and Column

(move mouse to display the column magnitude of 🐸 and click to randomize `q`)  
{{< p5-global-iframe quadrille="true" width="425" height="425" >}}
'use strict';
Quadrille.cellLength = 50;
let q;
let fuchsia;

function setup() {
  createCanvas(8 * Quadrille.cellLength, 8 * Quadrille.cellLength);
  fuchsia = color('Fuchsia');
  q = createQuadrille(8, 8).rand(16, '🐸').rand(30, fuchsia);
}

function draw() {
  background('#6495ED');
  drawQuadrille(q);
  let magnitude = 0;
  const col = q.mouseCol;
  for (const cell of q.cells({ value: v => v === '🐸', col: c => c === col })) {
    magnitude++;
  }
  const label = 'emoji magnitude';
  text(`${label} at column ${col}: ${magnitude}`, 20, 20);
}

function mousePressed() {
  q.randomize();
}
{{< /p5-global-iframe >}}

{{% details title="code" open=true %}}
```js
Quadrille.cellLength = 50;
let q;
let fuchsia;

function setup() {
  createCanvas(8 * Quadrille.cellLength, 8 * Quadrille.cellLength);
  fuchsia = color('Fuchsia');
  q = createQuadrille(8, 8).rand(16, '🐸').rand(30, fuchsia);
}

function draw() {
  background('#6495ED');
  drawQuadrille(q);
  let magnitude = 0;
  const col = q.mouseCol;
  for (const cell of q.cells({ value: v => v === '🐸', col: c => c === col })) {
    magnitude++;
  }
  const label = 'emoji magnitude';
  text(`${label} at column ${col}: ${magnitude}`, 20, 20);
}

function mousePressed() {
  q.randomize();
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