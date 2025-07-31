---
title: cells(filter?)
weight: 3
---

[Generator function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Iterators_and_generators#generator_functions) that lazily yields matching cells in the quadrille, each as an object of the form `{ row, col, value }`.

Traversal is row-major (top to bottom, left to right). Returns an [`IterableIterator`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols#the_iterable_protocol)—not an array—so values are produced on demand.

{{< callout type="info" >}}  
This method is typically used in conjunction with [`for...of`]({{< relref for_of >}}) or [`visit`]({{< relref visit >}}). Direct use is rare and generally reserved for cases involving array transformations or custom iteration logic.  
{{< /callout >}}

## Examples

### **No filter**: yield all cells

```js
q.cells();
````

### **Function filter**: value-based predicate

```js
q.cells(({ value }) => value != null);
```

Use also `Quadrille.isFilled(value)`, `Quadrille.isEmpty(value)`, or a custom predicate.

### **Function filter**: row and column predicates

```js
// even rows and columns less than 4
q.cells(({ row, col }) => row % 2 === 0 && col < 4);
```

### **Function filter**: combined value, row, and col predicates

```js
q.cells(({ value, row, col }) =>
  typeof value === 'string' &&
  row % 2 === 0 &&
  col < 4
);
```

### **Array / Set filter**: whitelist of values

```js
q.cells(['a', 'b', 'c']);
q.cells(new Set([1, 2, 3]));
```

Yields cells whose value is included.

### **Advanced**: transform or analyze matching cells

```js
// Extract values from filled cells in the top-left 4×4 block
const values = Array.from(q.cells(({ value, row, col }) =>
  Quadrille.isFilled(value) && row < 4 && col < 4
)).map(({ value }) => value);
```

Use this when:

* you need an array of matching cells,
* you're chaining with array methods like [`map`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map) or [`reduce`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce),
* or you're doing offline processing or analysis.

## Syntax

> `cells()`
> `cells(filter)`

## Parameters

| Param    | Description                                                                                                                                                                                                                |
| -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `filter` | Optional filter to select which cells are yielded. It can be: <ul><li>a value collection (`Array` or `Set`)</li><li>a predicate function (`({ row, col, value }) => boolean`)</li></ul> If omitted, all cells are returned |