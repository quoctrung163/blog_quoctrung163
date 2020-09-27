+++
title = "TypeScript for Functional Programmers"
date = "2020-09-27"
author = "quoctrung163"
authorTwitter = "quoctrung163" #do not include @
cover = ""
tags = ["ts", "getstarted"]
keywords = ["", ""]
description = ""
showFullContent = false
+++

### I. Types
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

  - Object literal type syntax phản ánh chặt chẽ object literal value syntax:
  
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

{{<linebreak>}}

### II. Gradual typing
- Typescript sử dụng type `any` khi mà bạn không nói `type` của biểu thức là gì?

- Compared to Dynamic, gọi `any` type là overstatement. Nó chỉ tắt type checker bất cứ khi nào nó xuất hiện.

- For example, you can push any value into an any[] without marking the value in any way:

```ts
// with "noImplicitAny": false in tsconfig.json, anys: any[]
const anys = [];
anys.push(1);
anys.push("oh no");
anys.push({ anything: "goes" });
```

- and you use an expression of type `any` anywhere
```ts
anys.map(anys[1]); // "oh no" is not a function
```

- nếu bạn init variable với type `any`, variable sẽ được gán type `any`.

```ts
let sepsis = anys[0] + anys[1]; 
// this could mean anything
```

- to get an error when TS sản xuất (produces) an any, use `noImplicitAny": true` or `strict: true` in `tsconfig.json`.

{{<linebreak>}}
### III. Structural typing
- Structural typing là khái niệm quen thuộc với hầu hết functional programmers, mặc dù Haskell và MLs không phải type `Structural`.

```ts
// @strict: false
let o = { x: "hi", extra: 1 }; // ok
let o2 : { x: string } = o; // ok
```

- Ở ví dụ trên `{ x: "hi", extra: 1}` matching với `{ x: string, extra: number }`. 
- Nhưng nó lại chấp nhận gán với ` x: string` bởi vì nó có tất cả yêu cầu về `properties` và các `properties` đó có `type` để gán :(

{{<linebreak>}}

### IV. Unions
- Trong ts, union types are untagged (không được gắn thẻ). Nói cách khác, ko có phân biệt gì với `unions` như `data` trong Haskell.

- Tuy nhiên bạn có thể phân loại type trong `unions` bằng cách sử dụng built-in tags or other property.

```ts
function start(
  arg: string | string[] | (() => string) | { s: string }
) : string {
  // this is super common in Javascript
  if (typeof arg === "string") {
    return commonCase(arg);
  } else if (Array.isArray(arg)) {
    return arg.map(commonCase).join(",");
  } else if (typeof arg === "function") {
    return commonCase(arg());
  } else {
    return commonCase(arg.s);
  }

  function commonCase(s: string): string {
    // finally, just convert a string to another string
    return s;
  }
}
```

- `string`, `Array` và `Function` có built-in type được cài sẵn, thuận tiện để lại `object type` cho các `else branch`.

- Điều này khả thi tuy nhiên để tạo `unions` khó phân biệt trong runtime.

- For new code, it's best to build only discriminated unions.

- the following types have built-in predicates:

  | Type  | Predicate  |
  |---|---|
  |  string  | typeof s === "string"  |
  | number | typeof n === "number" |
  | bigint  | typeof m === "bigint"  |
  | boolean | typeof b === "boolean" |
  | symbol | typeof g === "symbol" |
  | undefined | typeof undefined === "undefined" |
  | function | typeof f === "function" |
  | array | Array.isArray(a)|
  | object | 	typeof o === "object" |

{{<linebreak>}}
### V. Intersections
- In addition to unions, TS also has intersections (giao lộ)

```ts
type Combined = { a: number } & { b: string };
type Conflicting = { a: number } & { a: string };
```

- `Combined` has two properties, a and b, just as if they had been written as one `object literal type`.

- Intersection and union đệ quy trong từng trường hợp xung đột, so `Conflicting.a: number & string`

{{<linebreak>}}
### VI. Unit types
- Unit types are subtypes (kiểu phụ) of primitive types that contain exactly one primitive value.

- For example, the string "foo" has the type "foo". Since JavaScript has no built-in enums, it is common to use a set of well-known strings instead. Unions of string literal types allow TypeScript to type this pattern:

```ts
declare function pad(s: string, n: number, direction: "left" | "right"): string;
pad("hi", 10, "left");
```

- When needed, the compiler widens — converts to a supertype — the unit type to the primitive type, such as "foo" to string. This happens when using mutability, which can hamper some uses of mutable variables:

```ts
let s = "right";
pad("hi", 10, s); // error: 'string' is not assignable to '"left" | "right"'
Argument of type 'string' is not assignable to parameter of type '"left" | "right"'.
```

- Here’s how the error happens:
  - "right": "right"
  - s: string because "right" widens to string on assignment to a mutable variable.
  - string is not assignable to "left" | "right"

{{<linebreak>}}
### VII. Contextual typing
- Ts có một số chỗ rõ ràng mà nó có thể suy ra các types :v, như khai báo biến:

```ts
let s = "i'm a string";
```

- But it also infers types in a few other places that you may not expect if you've worked with other C-syntax language.

```ts
declare function map<T, U>(f: (t: T) => U, ts: T[]): U[];
let sns = map((n) => n.toString(), [1, 2, 3]);
```

- Here, `n: number` in this example cũng vậy, mặc dù T và U chưa được suy luận trước khi gọi (inferred before the call).

- In face, after [1, 2, 3] has been used to infer `T = number`, then return type of `n => n.toString()` is used to infer `U = string`, causing `sns` to have the type `string[]`.

- Note that inference(suy luận) will work in any order, but intellisense will only work left-to-right, so TS prefers to declare `map` with the array first:

```ts
declare function map<T, U>(ts: T[], f: (t: T) => U): U[];
```

- Contextual typing cũng hoạt động đệ quy thông qua object literals, và trên unit types sẽ được suy ra là `string` or `number`. And it can infer return types from context:

```ts
declare function run<T> (thunk: (t: T) => void): T;
let i: { interface: string } = run ((o) => {
  o.interface = "INSERT STATE HERE";
});
```

{{< linebreak >}}
### VIII. Type aliases
- Type aliases là một cách gọi khác, như `type` trong Haskel. The compiler sẽ cố gắng sử dụng `alias name` bất cứ khi nào nó muốn sử dụng trong source code, nhưng không phải lúc nào cũng thành công.

```ts
type Size = [number, number];
let x: Size = [101.1, 999.9];
```

{{< linebreak >}}
### IX. Discriminated Unions
- The closest equivalent to data is a union of types with discriminant properties, normally called discriminated unions in TypeScript:

```ts
type Shape =
  | { kind: "circle"; radius: number }
  | { kind: "square"; x: number }
  | { kind: "triangle"; x: number; y: number };
```

- Unlike Haskell, the tag, or discriminant, is just a property in each object type. Each variant has an identical property with a different unit type. This is still a normal type;

- The leading `|` is an optional part of the union type syntax. You can discriminate (phân biệt) members of the union using normal Javascript code:

```ts
type Shape = 
  | { kind: "circle"; radius: number }
  | { kind: "square"; x: number}
  | { kind: "triangle"; x: number; y: number };

function area(s: Shape) {
  if (s.kind === "circle") {
    return Math.PI * s.radius * s.radius;
  } else if (s.kind === "square") {
    return s.x * s.x;
  } else {
    return (s.x * s.y) / 2;
  }
}
```

{{< linebreak >}}
### X. Type Parameters
- Like most C-desended(C-hạ xuống) languages, TS requires declaration of type parameters:

```ts
function liftArray<T>(t: T): Array<T> {
  return [t];
}
```

- Không có yêu cầu về TH, nhưng các `type parameters` được quy ước là các chữ cái viết hoa đơn.

```ts
function firstish <T extends { length: number }>(t1: T, t2: T
  return t1.length > t2.length ? t1 : t2;
)
```