# Which array method returns only the elements that match a condition?

## Theory

## Common Array Methods

```typescript
const prices = [10, 25, 99];

prices.map(p => p * 2);       // [20, 50, 198]  transforms every element
prices.filter(p => p > 20);   // [25, 99]       keeps matching elements
prices.find(p => p > 20);     // 25             first match or undefined
prices.some(p => p > 90);     // true           does any element match?
prices.every(p => p > 0);     // true           do all elements match?
prices.includes(25);          // true
```

`map`, `filter` and `find` do not change the original array.

## Question

Which array method returns a new array with only the elements that match a condition?

## Options

- **A.** `map`
- **B.** `find`
- **C.** `filter`
- **D.** `forEach`

## Correct Answer

**C**

## Explanation

**Option C** is correct. `filter` runs the condition for every element and returns a new array containing only those for which it returned `true`.

- **A** `map` returns a new array of the same length with transformed elements.
- **B** `find` returns only the **first** matching element (or `undefined`), not an array.
- **D** `forEach` runs a function for each element and returns `undefined`; it is for side effects, not for building results.
