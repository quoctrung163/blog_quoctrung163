+++
title = "Functions"
date = "2020-10-02"
author = "quocturng163"
authorTwitter = "" #do not include @
cover = ""
tags = ["ts", "handbook"]
keywords = ["", ""]
description = ""
showFullContent = false
+++

### Functions
- Just as in JavaScript, TypeScript functions can be created both as a named function or as an anonymous function

```ts
// Named function
function add(x, y) {
  return x + y;
}

// Anonymouse function
let myAdd = function (x, y) {
  return x + y;
};
```

- function can refer to variables outside of the function body.

```ts
let z = 100;

function addToZ(x, y) {
  return x + y + z;
}
```

{{< linebreak >}}

### Funciton types

#### Typing the function
- We can add types to each of the parameter and then to the function itself to add a return type.

- Typescript can figure the return type out by looking at the return statements, so we can also optionally leave this off in many cases.

```ts
function add(x: number, y: number): number {
  return x + y;
}

let myAdd = function (x: number, y: number): number {
  return x + y;
}
```

#### Writing the function type

```ts
let myAdd: (x: number, y: number) => number = function (
  x: number,
  y: number
): number {
  return x + y;
}
```

- a funtion's type has the same two parts: the type of arguments and the return type.

{{< linebreak >}}

### Interring the types
- In playing with example, you may notice that the Typescript compiler can figure out the type even if you only have types on one side of the equation:

```ts
// the parameter 'x' and 'y' have the type number
let myAdd = function (x: number, y: number): number {
  return x + y;
}

// myAdd has the full function type
let myAdd2: (baseValue: number, increment: number) => number = function (x, y) {
  return x + y;
}
```