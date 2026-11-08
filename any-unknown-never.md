# any, unknown and never in TypeScript

## Introduction

When I started TypeScript, I was confused about `any`, `unknown` and `never`. They look the same, but they are not. All three sit at special places in the type hierarchy. This is what I understood.

## What they mean

`any` turns off type checking. `unknown` can hold any value, but you must check it before use. It is the safe top type, because every type can be assigned to it. `never` means a value that can never happen. It is the bottom type, so it can be assigned to every type.

## any is risky

```ts
let a: any = "hello";
a.toFixed(); // no error here, but it crashes when running
```

TypeScript does not warn me here. The bug shows up only when the code runs.

## unknown is safe

```ts
let u: unknown = "hello";

if (typeof u === "string") {
  u.toUpperCase(); // safe now
}
```

I must check the type first. Then TypeScript lets me use it. This is why `unknown` is better than `any` for API responses and `catch` errors.

## never is for checking all cases

```ts
type Shape = "circle" | "square";

function area(s: Shape): number {
  switch (s) {
    case "circle":
      return 3.14;
    case "square":
      return 4;
    default:
      const check: never = s;
      return check;
  }
}
```

If I add a new shape later and forget it here, TypeScript shows an error.

## Conclusion

I use `unknown` when I am not sure about a type. I use `never` for impossible cases. I try to avoid `any`, because it hides bugs instead of preventing them.