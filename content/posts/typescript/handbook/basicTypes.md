+++
title = "Basic Types"
date = "2020-09-27"
author = "quoctrung163"
authorTwitter = "quoctrung163" #do not include @
cover = ""
tags = ["handbook", "ts"]
keywords = ["", ""]
description = ""
showFullContent = false
+++

### Boolean
- The most basic datatype is simple `true/false` value

```ts
let isDone: boolean = false;
```
{{< linebreak >}}

### Number
- As in Javascript, all numbers in Typescript, are either floating point values or BigIntegers.

- These floating point number get the type `number`, while BigIntergers get the type `bigint`.

```ts
let decimal: number = 6;
let hex: number = 0xf00d;
let binary: number = 0b1010;
let octal: number = 0o744;
let big: bigint = 100n;
```
{{< linebreak >}}

### String
- like JS, TS also uses double quotes (`"`) or single quotes (`'`) to surround string data.

```ts
let color: string = "blue";
color = "red";
```

- bạn có thể sử dụng `template string` `` 
để nhúng các biểu thức typescript `${expr}`.

```ts
let fullName: string = `Quoc Trung`;
let age: number = 20;
let sentence: string = `Hello, my name is ${fullName}. 
I'll be ${age + 1} years old next month.`;
```
{{< linebreak >}}

### Array
- Giống với JS, TS cho phép làm việc với các array.

- Array types trong TS có thể viết bằng 2 cách.

- Cách 1: bạn sử dụng các type sau `[]` để chứng tỏ đó là array of element type

```ts
let list: number[] = [1, 2, 3];
```

- Cách 2: là sử dụng generic array type, `Array<elemType>`:

```ts
let list: Array<number> = [1, 2, 3];
```
{{< linebreak >}}

### Tuple
- Tuple types cho phép bạn thể hiện array với 1 số lượng elements cố định có kiểu đã biết.

- vd bạn muốn bắt buộc giá trị là `number` và `string`:
```ts
// declare a tuple type
let x: [string, number];

// initialize it
x = ["hello", 10]; // OK

// initialize it incorrectly
x = [10, "hello"]; // Error;

`Type 'number' is not assignable to type 'string'.
Type 'string' is not assignable to type 'number'.`
```

{{< linebreak >}}

### Enum
- Một bổ sung hữu ích để set datatype từ JS là `enum`. Như những ngôn ngữ như C#, an enum là cách thêm các tên các giá trị tuỳ chọn.

```ts
enum Color {
  Red,
  Green,
  Blue
}

let c: Color = Color.Green;
```

- Mặc định, enums bắt đầu giá trị cho các member là `0`. 

- Vd: Red :`0`, Green: `1`, Blue = `2`

- you also can change value: :D

```ts
enum Color {
  Red = 1,
  Green = 2,
  Blue = 4
}

let c: Color = Color.Green;
console.log(c); // => 2
```

- Bạn có thể lấy propertyName

```ts

```