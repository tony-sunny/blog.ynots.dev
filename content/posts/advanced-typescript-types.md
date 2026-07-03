+++
title = 'Advanced TypeScript Types'
date = 2024-09-22T10:13:51+05:30
lastmod = 2024-09-22T10:13:51+05:30
draft = false
tags = ['typescript']
+++

## Variance annotations

When TypeScript checks whether one generic type is assignable to another, it normally has to inspect how the type parameter is actually used inside the type, which can get slow (or ambiguous) for large or recursive generics. Variance annotations let you tell the compiler upfront how a type parameter is used, so it doesn't have to work that out on its own.
<!--more-->

There are two annotations you can add to a type parameter:
- `out T` — declares that `T` only shows up in "output" positions, like a return value. This makes the type **covariant** in `T`.
- `in T` — declares that `T` only shows up in "input" positions, like a function parameter. This makes the type **contravariant** in `T`.

```ts
interface Producer<out T> {
  make(): T;
}

interface Consumer<in T> {
  consume(value: T): void;
}
```

With `Producer`, a `Producer<Dog>` can be used wherever a `Producer<Animal>` is expected, since anything that produces a `Dog` also produces an `Animal`. With `Consumer`, it's the other way around: a `Consumer<Animal>` can be used wherever a `Consumer<Dog>` is expected, since something that can handle any `Animal` can certainly handle a `Dog`.

These annotations are just a hint, not a restriction. If your usage happens to disagree with the annotation, TypeScript will report an error rather than silently trusting it.

Refer [the TypeScript handbook](https://www.typescriptlang.org/docs/handbook/2/generics.html#variance-annotations) for more details.

## Inferring types with `infer`

Inside a conditional type (`T extends U ? X : Y`), you sometimes want to pull out a piece of `T` without already knowing its shape. The `infer` keyword lets you declare a placeholder type variable right there in the `extends` clause, and TypeScript fills it in for you if the check matches.

A common use is pulling the return type out of a function type:

```ts
type ReturnTypeOf<T> = T extends (...args: any[]) => infer R ? R : never;

type A = ReturnTypeOf<() => string>; // string
type B = ReturnTypeOf<(x: number) => void>; // void
```

Here `R` isn't a type you provide — TypeScript matches `T` against the function shape and binds whatever occupies the return position to `R`, then substitutes it into the `true` branch.

The same idea works for unwrapping a type from any generic shape, not just functions:

```ts
type ElementType<T> = T extends (infer U)[] ? U : T;

type C = ElementType<string[]>; // string
type D = ElementType<number>; // number
```

`infer` only makes sense inside the `extends` clause of a conditional type — it's how TypeScript lets you destructure a type the same way you'd destructure a value.

Refer [the TypeScript handbook](https://www.typescriptlang.org/docs/handbook/2/conditional-types.html#inferring-within-conditional-types) for more details.
