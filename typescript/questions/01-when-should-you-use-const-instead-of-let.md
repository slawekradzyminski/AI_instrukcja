# When should you use const instead of let?

## Theory

## `const`, `let` and `var`

- `const` declares a variable that cannot be **reassigned**.
- `let` declares a variable that can be reassigned later.
- `var` is the old way; it has function scope and hoisting surprises, so modern code avoids it.

```typescript
const baseUrl = 'http://localhost:8081';
let retries = 0;
retries++;            // OK
// baseUrl = '/x';    // Error: Cannot assign to 'baseUrl' because it is a constant
```

`const` protects the variable binding, not the contents of an object:

```typescript
const user = { name: 'Ann' };
user.name = 'Bob';    // allowed: the object is changed, the variable is not reassigned
```

## Question

When should you use const instead of let?

## Options

- **A.** Only for numbers and strings
- **B.** By default, whenever the variable is not reassigned later
- **C.** Only inside classes
- **D.** When the value must never change, including object properties

## Correct Answer

**B**

## Explanation

**Option B** is the common convention: start with `const` and switch to `let` only when you really reassign the variable. It makes code easier to read, because a reader knows the value will not be replaced.

- **A** is wrong: `const` works with any value, including objects, arrays and functions.
- **C** is wrong: `const` is a normal variable declaration and is used everywhere.
- **D** is wrong: `const` does not freeze objects. Properties of a `const` object can still change; only reassignment of the variable is blocked.
