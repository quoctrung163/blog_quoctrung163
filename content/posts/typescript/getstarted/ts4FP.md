+++
title = "TypeScript for Functional Programmers"
date = "2020-09-24"
author = "quoctrung163"
authorTwitter = "quoctrung163" #do not include @
cover = ""
tags = ["ts", "getstated"]
keywords = ["", ""]
description = ""
showFullContent = false
+++

I. Types
- Primitive types:
  - number
  - string
  - bigint
  - boolean
  - symbol
  - null
  - underfined
  - object

- Other important Typescript types

  |  Type   |  Explanation   |
  | --- | --- |
  |  unknown   |  the top type   |
  |  never   |   the bottom type   |
  |  object literal   |  eg { property: Type }   |
  |  void   |  a subtype of undefined intended for use as return type   |
  |  T[]   |  mutable array, also written Array<T>   |
  |  [T, T]   | tuples, which are fixed-length but mutable |
  |   (t: T) => U | functions |

- Notes
  - Function syntax includes parameter names. This is pretter hard to get used to!

  ```ts
  let fst: (a: any, d: any) => any = (a, d) => a;
  //        ^ (t: t) => U | anonymous function
  // or more precisely :(
  let snd: <T, U>(a: T, d: U) => U = (a, d) => d;
  ```

  - Object literal type syntax closely mirrors object literal value syntax:
  
  ```ts
  let o: { n: number; xs: object[] } = { n: 1, xs: [] };
  ```

  - [T, T] is a subtype of T[]

- Boxed types

```ts
(1).toExponential();
// equivalent to
Number.prototype.toExponential.call(1);
```

