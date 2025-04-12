---
weight: 1
title: visit(fx)
---

[p5.js](https://p5js.org/) function that visits all cells in the `quadrille`, executing the specified `fx` function on each cell. The function takes `(row, col)` as parameters to define the cell's position.

## Example

The example below uses a named function `fx` to visit each cell in the upper quadrille, filling its empty cells that have exactly three neighbors, and storing the result in the lower quadrille.

{{< p5-global-iframe quadrille="true" width="625" height="425" >}}
'use strict';
let source, target;
Quadrille.cellLength = 20;
const w = 600 / Quadrille.cellLength;
const h = 400 / Quadrille.cellLength;
let yellow, blue, red;

function setup() {
  createCanvas(600, 400);
  yellow = color('yellow');
  blue = color('blue');
  red = color('red');
  source = createQuadrille(w - 2, (h / 2) - 2, 50, yellow);
  source.rand(50, blue);
  target = createQuadrille(w - 2, (h / 2) - 2);
  source.visit(fx);
}

function fx({row, col}) {
  if (source.isEmpty(row, col) && source.ring(row, col).order === 3) {
    target.fill(row, col, red);
  }
}

function draw() {
  background('coral');
  drawQuadrille(source, { outline: 'white', row: 1, col: 1 });
  drawQuadrille(target, { outline: 'cyan', row: (h / 2) + 1, col: 1 });
}
{{< /p5-global-iframe >}}

{{% details title="code" open=true %}}
```js
let source, target;
Quadrille.cellLength = 20;
const w = 600 / Quadrille.cellLength;
const h = 400 / Quadrille.cellLength;
let yellow, blue, red;

function setup() {
  createCanvas(600, 400);
  yellow = color('yellow');
  blue = color('blue');
  red = color('red');
  source = createQuadrille(w - 2, (h / 2) - 2, 50, yellow);
  source.rand(50, blue);
  target = createQuadrille(w - 2, (h / 2) - 2);
  source.visit(fx);
}

function fx({row, col}) {
  if (source.isEmpty(row, col) && source.ring(row, col).order === 3) {
    target.fill(row, col, red);
  }
}

function draw() {
  background('coral');
  drawQuadrille(source, { outline: 'white', row: 1, col: 1 });
  drawQuadrille(target, { outline: 'cyan', row: (h / 2) + 1, col: 1 });
}
```
{{% /details %}}

{{< callout type="info" >}}
To implement the above sketch, the following quadrille methods are used: [isEmpty()]({{< ref "is_empty" >}}), [ring]({{< ref "ring" >}}), [rand(times, value)]({{< ref "rand_times_value" >}}), and [fill(row, col, value)]({{< ref "fill_row_col_value" >}}).
{{< /callout >}}

## Example using arrow functions

The following example uses an [arrow function](https://www.w3schools.com/js/js_arrow_function.asp), also known as an anonymous function, instead of a named function. Arrow functions provide a concise way to write functions in JavaScript, especially for short, one-off uses.

{{< p5-global-iframe quadrille="true" width="625" height="425" >}}
'use strict';
let source, target;
Quadrille.cellLength = 20;
const w = 600 / Quadrille.cellLength;
const h = 400 / Quadrille.cellLength;
let yellow, blue, red;

function setup() {
  createCanvas(600, 400);
  yellow = color('yellow');
  blue = color('blue');
  red = color('red');
  source = createQuadrille(w - 2, (h / 2) - 2, 50, yellow);
  source.rand(50, blue);
  target = createQuadrille(w - 2, (h / 2) - 2);
  source.visit(({row, col}) => {
    if (source.isEmpty(row, col) && source.ring(row, col).order === 3) {
      target.fill(row, col, red);
    }
  });
}

function draw() {
  background('coral');
  drawQuadrille(source, { outline: 'white', row: 1, col: 1 });
  drawQuadrille(target, { outline: 'cyan', row: (h / 2) + 1, col: 1 });
}
{{< /p5-global-iframe >}}

{{% details title="code" open=true %}}
```js
let source, target;
Quadrille.cellLength = 20;
const w = 600 / Quadrille.cellLength;
const h = 400 / Quadrille.cellLength;
let yellow, blue, red;

function setup() {
  createCanvas(600, 400);
  yellow = color('yellow');
  blue = color('blue');
  red = color('red');
  source = createQuadrille(w - 2, (h / 2) - 2, 50, yellow);
  source.rand(50, blue);
  target = createQuadrille(w - 2, (h / 2) - 2);
  source.visit(({row, col}) => {
    if (source.isEmpty(row, col) && source.ring(row, col).order === 3) {
      target.fill(row, col, red);
    }
  });
}

function draw() {
  background('coral');
  drawQuadrille(source, { outline: 'white', row: 1, col: 1 });
  drawQuadrille(target, { outline: 'cyan', row: (h / 2) + 1, col: 1 });
}
```
{{% /details %}}

### Step-by-Step Transformation to Arrow Function

To convert the named function `fx` into an [arrow function](https://www.w3schools.com/js/js_arrow_function.asp), also known as an anonymous function, follow these steps:

```js
// Original named function
function fx(row, col) {
  if (source.isEmpty(row, col) && source.ring(row, col).order === 3) {
    target.fill(row, col, red);
  }
}
```

**Step 1:** Convert the named function to an anonymous function expression.

```js
const fx = function(row, col) {
  if (source.isEmpty(row, col) && source.ring(row, col).order === 3) {
    target.fill(row, col, red);
  }
};
```

**Step 2:** Replace `function` with arrow syntax.

```js
const fx = (row, col) => {
  if (source.isEmpty(row, col) && source.ring(row, col).order === 3) {
    target.fill(row, col, red);
  }
};
```

**Final Step:** Inline the arrow function directly in `visit` as shown in the second example above:

```js
source.visit(({row, col}) => {
  if (source.isEmpty(row, col) && source.ring(row, col).order === 3) {
    target.fill(row, col, red);
  }
});
```

Using arrow functions provides a more concise syntax, making them suitable for inline functions in cases like this one.

{{< callout type="info" >}}
From now on, [arrow functions](https://www.w3schools.com/js/js_arrow_function.asp), also known as anonymous functions, will be used instead of named functions to simplify examples. Keep in mind that arrow functions do not have their own `this` context. For more details on how `this` behaves in arrow functions, refer to [this guide on arrow functions](https://www.w3schools.com/js/js_arrow_function.asp).
{{< /callout >}}

## Syntax

> `visite(fx)`

## Parameters

| Param | Description                                                                         |
|-------|-------------------------------------------------------------------------------------|
| `fx`  | Function: A function of the form `fx(row, col)` to be executed on all visited cells |