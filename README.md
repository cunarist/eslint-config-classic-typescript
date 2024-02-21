# `eslint-config-classic-typescript`

**Enforce classic JavaScript practices in TypeScript codebase by banning excessive keywords**

Though TypeScript has brought us type-safety for web development, many developers agree on that it has too many duplicated keywords and ways of writing code to achieve the same thing.

Not only that, TypeScript is not a standard web language. It might be hard to refactor the code once alternatives arise, such as [the type annotation proposal for JavaScript](https://github.com/tc39/proposal-type-annotations), if the codebase contains too much TypeScript-specific syntax.

Based on [`eslint`](https://github.com/eslint/eslint) and [`typescript-eslint`](https://github.com/typescript-eslint/typescript-eslint), this configuration enforces classic JavaScript syntax in TypeScript code. JavaScript is the standard language that has its own way of doing things.

The basic principle is not to declare things that don't exist in JavaScript. By doing so, TypeScript can be more coherent with JavaScript, while assisting only in areas that are related to type safety.

## Rules

### Class Based Types

Whether to use `type`, `interface`, or `class` to represent a typed structure is a long controversy in the TypeScript community.

This is related to the historical path of TypeScript, where the `interface` keyword was introduced in 2012, when JavaScript `class` keyword was not a thing yet. The JavaScript `class` keyword was introduced later in 2015.

This configuration makes your codebase use only `class` for types, to adhere to classic JavaScript way of doing object-oriented programming.

```typescript
// Produces warning.
enum MyEnum {
  A,
  B,
}

// Produces warning.
interface MyInterface {
  a: number;
  b: string;
}
```

```typescript
// OK.
class MyEnum {
  static A = 0;
  static B = 1;
}

// OK.
class MyInterface {
  a: number;
  b: string;
}
```

### No Type Aliases

```typescript
// Produces warning.
type ShapeType = { a: boolean; b: boolean };

// Produces warning.
type AliasType = Array<string>;

function myFunction(array: AliasType) {
  console.log(array);
}
```

```typescript
function myFunction(array: Array<string>) {
  console.log(array);
}
```

### No Namespace

ES6 modules should be used instead of namespaces. TypeScript was influced quite a lot from C++ and C#, and this keyword doesn't align well with JavaScript practices. This rule is also included in `typescript-eslint`'s recommended configuration.

```typescript
// Produces warning.
namespace MyNamespace {}
```

### Array Type Annotation

When annotating array types in TypeScript, developers encounter two syntaxes: `Array<T>` and `T[]`. While both syntaxes are valid and functionally equivalent, this configuration enforces the use of `Array<T>` over `T[]` for array type annotations.

The decision to prefer `Array<T>` over `T[]` is for ensuring alignment with high-level language paradigm, particularly in the context of JavaScript development. While `T[]` may be more familiar to developers coming from lower-level languages such as C, C++, and Rust, where arrays represent contiguous memory spaces, it does not really make sense for high-level languages like JavaScript.

In contrast, `Array<T>` provides a more explicit and descriptive representation of the `Array` type of JavaScript. This clarity is particularly beneficial for developers collaborating on projects, as it reduces ambiguity and aids in understanding the constructor and prototype of an array.

```typescript
// Produces warning.
const array: number[] = [1, 2, 3];
```

```typescript
// OK.
const array: Array<number> = [1, 2, 3];
```