# Type Narrowing in TypeScript

## Introduction

Type narrowing means making a general type more specific inside a block of code. It is very helpful when you work with union types. Without it, TypeScript would not let you call methods that exist on only one member of the union. The three simple ways to do it are `typeof`, `instanceof` and `in`.

## Narrowing with typeof

`typeof` is used for primitive types like string and number.

```ts
function format(value: string | number): string {
  if (typeof value === "string") {
    return value.toUpperCase();
  }
  return value.toFixed(2);
}
```

Inside the `if` block, TypeScript knows that `value` is a string. After that block, it knows `value` must be a number.

## Narrowing with instanceof

`instanceof` is used for classes.

```ts
function handle(err: Error | string): string {
  if (err instanceof Error) {
    return err.message;
  }
  return err;
}
```

TypeScript checks the prototype chain, so the `Error` branch gets full access to `message` and `stack`. I find this useful inside `catch` blocks because the error can be anything.

## Narrowing with in

The `in` operator checks if a property exists in an object.

```ts
type Dog = { bark(): void };
type Cat = { meow(): void };

function speak(pet: Dog | Cat): void {
  if ("bark" in pet) {
    pet.bark();
  } else {
    pet.meow();
  }
}
```

Here TypeScript understands that if `bark` exists, the `pet` is a `Dog`. This is especially useful for object types that have no shared property to compare.

## Conclusion

Narrowing helps us avoid unsafe type assertions. I use `typeof` for primitives, `instanceof` for classes and `in` for object shapes. Together, these checks make code safer and easier to read.