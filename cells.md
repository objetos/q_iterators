---
title: cells(filter?)
weight: 3
---

[Generator function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Iterators_and_generators#generator_functions) that lazily yields matching cells in the quadrille, each as an object of the form `{ row, col, value }`.

Traversal is row-major (top to bottom, left to right). Returns an [`IterableIterator`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols#the_iterable_protocol)—not an array—so values are produced on demand.

{{< callout type="info" >}}  
This method underlies both [`for...of`]({{< relref for_of >}}) and [`visit`]({{< relref visit >}}), which are preferred in most use cases. Direct use is rare and only recommended when chaining with array methods.
{{< /callout >}}

## Examples

### **No filter**: yield all cells

```js
q.cells();
```

### **Function filter**: test value only

```js
q.cells(value => value != null);
```

Use `Quadrille.isFilled`, `Quadrille.isEmpty`, or a custom predicate.

### **Array / Set filter**: whitelist of values

```js
q.cells(['a', 'b', 'c']);
q.cells(new Set([1, 2, 3]));
```

Yields cells whose value is included.

### **Object filter**: combine value, row, and col predicates

```js
q.cells({
  value: v => typeof v === 'string',
  row: r => r % 2 === 0,
  col: c => c < 4
});
```

Each predicate is optional. All must match if provided.

### **Advanced**: transform or analyze matching cells

```js
// Extract values from filled cells in the top-left 4×4 block
const values = Array.from(q.cells({
  value: Quadrille.isFilled,
  row: r => r < 4,
  col: c => c < 4
})).map(({ value }) => value);
```

Use this when:
- you need an array of matching cells,
- you're chaining with array methods like [`map`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map) or [`reduce`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce),
- or you're doing offline processing or analysis.

## Syntax

> `cells()`  
> `cells(filter)`

## Parameters

| Param    | Type                          | Description |
|----------|-------------------------------|-------------|
| `filter` | `Array`, `Set`, `Function`, or `Object` | Optional filter to select which cells are yielded. Accepts: <ul><li>a value collection (`Array` or `Set`)</li><li>a predicate function (`value => boolean`)</li><li>an object with optional `value`, `row`, and/or `col` predicate functions</li></ul> If omitted, all cells are returned. |