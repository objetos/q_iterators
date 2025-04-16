---
title: cells(filter?)
weight: 3
---

Lazily iterates over all matching cells in the quadrille.

The `cells(filter?)` method is the foundation of all quadrille iteration.

It yields each cell in **row-major order** (top to bottom, left to right), returning an object of the form:

```js
{ row: number, col: number, value: any }
```

This generator is **lazy**, meaning it produces values on demand, and is the **basis** of both `for...of` loops and the `visit` method.

{{< callout type="info" >}}  
`cells()` is the foundation of `for...of` and `visit`.  
You can use it directly to power advanced filtering, mapping, and functional patterns.
{{< /callout >}}  

{{< callout type="warning" >}}  
`cells()` is best suited for intermediate to advanced users comfortable with generators and iterables.  
Beginners should start with [`for...of`](/docs/api/Quadrille/for_of) or [`visit()`](/docs/api/Quadrille/visit) for easier syntax.
{{< /callout >}}  

## Usage

### Yield all cells

```js
[...q.cells()].forEach(({ row, col, value }) => {
  console.log(row, col, value);
});
```

### Yield only non-empty (filled) cells

```js
[...q.cells(Quadrille.isFilled)];
```

### Use with Array methods

```js
const colors = [...q.cells()].map(({ value }) => value.toString());
const numericCells = [...q.cells(v => typeof v === 'number')];
```

## Filters

You can optionally pass a `filter` to control which cells are yielded.  
This makes `cells()` highly versatile and expressive.

### 1. **No filter**: yield all cells

```js
for (const cell of q.cells()) console.log(cell);
```

### 2. **Function filter**: test value only

```js
q.cells(value => value != null);
```

Use `Quadrille.isFilled`, `Quadrille.isEmpty`, or your own custom test.

### 3. **Array / Set filter**: whitelist of values

```js
q.cells(['a', 'b', 'c']);
q.cells(new Set([1, 2, 3]));
```

Only yield cells whose value is included.

### 4. **Object filter**: target specific rows, columns, or values

```js
q.cells({
  value: v => typeof v === 'string',
  row: r => r % 2 === 0,
  col: c => c < 4
});
```

Each predicate (value, row, col) is optional.  
They are combined with `AND` logic — all must match.

## Examples

### Filter and count matching cells

```js
const filledCells = [...q.cells(Quadrille.isFilled)];
console.log(filledCells.length);
```

### Replace all strings with numbers

```js
for (const { row, col, value } of q.cells(v => typeof v === 'string')) {
  q.fill(row, col, value.length);
}
```