# What is an interface used for?

## Theory

## Interfaces and Type Aliases

An `interface` (or a `type` alias) describes the shape of an object:

```typescript
interface LoginDto {
  username: string;
  password: string;
}

type Role = 'ROLE_ADMIN' | 'ROLE_CLIENT';

const body: LoginDto = { username: 'admin', password: 'secret' };
// const bad: LoginDto = { username: 'admin' }; // Error: property 'password' is missing
```

In API tests, interfaces describe request bodies (DTOs) and expected responses, so the editor autocompletes fields and catches typos.

## Question

What is a TypeScript `interface` mainly used for in test code?

## Options

- **A.** Describing the shape of objects, such as request and response bodies
- **B.** Creating objects at runtime
- **C.** Defining CSS selectors
- **D.** Running code in parallel

## Correct Answer

**A**

## Explanation

**Option A** is correct. An interface lists the properties and their types that an object must have. The compiler then checks every object declared with that type.

- **B** is wrong: interfaces disappear at runtime and cannot create objects (classes and object literals do that).
- **C** and **D** are unrelated to interfaces.

`type` aliases can do the same for object shapes and can also express unions such as `'ROLE_ADMIN' | 'ROLE_CLIENT'`.
