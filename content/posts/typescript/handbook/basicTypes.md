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

  - 1: bạn sử dụng các type sau `[]` để chứng tỏ đó là array of element type

  ```ts
  let list: number[] = [1, 2, 3];
  ```

  - 2: là sử dụng generic array type, `Array<elemType>`:

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

- you can change value: :D

```ts
enum Color {
  Red = 1,
  Green = 2,
  Blue = 4
}

let c: Color = Color.Green;
console.log(c); // => 2
```

- Bạn cũng có thể lấy propertyName

```ts
enum Color {
  Red,
  Green,
  Blue
};

let colorName: string = Color[2];

console.log(colorName); // => Green
```

{{< linebreak >}}

### Unknown
- Chúng ta có thể cần theo dõi type variables mà ta không biết khi viết app. These values come from `dynamic content` - from the `user` - or we may want to intentionally(cố ý) accept all values in your API.

- In the case, we want to provide a type that tells the compiler and future readers that this varible could be (có thể là) anything, so we give it the `unknown` type.

```ts
let notSure: unknown = 4;
notSure = "maybe a string instead";

// OK, definitely a boolean
notSure = false;
```

- Nếu bạn có variable với type là `unknown`, bạn có thể làm cho nó hẹp lại thành một cái gì đó cụ thể hơn bằng cách `typeof` check, comparison checks, or more advanced type guards that will be discussed in a later chapter:

```ts
declare const maybe: unknown;
// 'maybe' could be a string, object, boolean, undefined, or other types
const aNumber: number = maybe;

`Type 'unkown' is not assignable to type 'number'`

if (maybe === true) {
  // Typescript knows that maybe is a boolean now
  const aBoolean: boolean = maybe;
  // so it cannot be a string
  const aString: string = maybe;
  `Type 'boolean' is not assignale to type 'string'`.
}

if (typeof maybe === "string") {
  // Typescript knows that maybe is a string
  const aString: string = maybe;
  // So, it cannot be a boolean
  const aBoolean: boolean = maybe;
  `Type 'string' is not assignable to type 'boolean'.`
}
```

{{< linebreak >}}

### Any
- `any` type :v

```ts
declare function getValue(key: string): any;
// OK return value of getValue is not checked
const str: string = getValue("mystring");
```
- the `any` type is powerful way to work with existing JS, allowing you to gradually opt-in and opt-out of type checking during compilation.

- Không như `unknown`, `any` cho phép bạn truy cập các thuộc tính tuỳ ý, ngay cả những thứ không tồn tại.

```ts
let looselyTyped: any = 4;
// OK, ifItExists might exist at runtime
looselyTyped.ifItExists();
looselyTyped.toFixed();

let strictlyTyped: unknown = 4;
strictlyTyped.toFixed();
`Object is of type 'unknown'.`
```

- The `any` will continue to prepagate throught your object:

```ts
let looselyTyped: any = {};
let d = looselyTyped.a.b.c.d;
```

{{< linebreak >}}

### Void
- like c++ or other programming, it return null or not return and just display some variable

```ts
function warnUser(): void {
  console.log("this is my warning message");
}
```

{{< linebreak >}}

### Null and Undefined
- like any programming like c#, java, c++, etc.. :v

```ts
let u: undefined = undefined;
let n: null = null;
```

{{< linebreak >}}

### Never
- `never` type đại diện giá trị type value không bao giờ xảy ra.

- for example `never` is the return type for a function expression or an arrow function expression that always throws an exception (ném 1 biểu thức) or never return.

- even `any` isn't assign to never

```ts
// function returning must not have a reachable end p
function error(message: string): never {
  throw new Error(message);
}

// Inferred return type is never
function fail() {
  return error("Something failed");
}

// function returning never must not have a reachable end p
function infiniteLoop(): never {
  while(true) {}
}
```

### Object
- Object là type đại diện cho `none-primitive type` (không nguyên thuỷ).

- Với `object` type, APIs như `Object.create` có thể đại diện tốt hơn (better represented)

```ts
declare function create(o: object | null): void;

// ok
create({prop: 0}); 
create(null);

// error
create(42); 
`Argument of type '42' is not assignable to parameter of type 'object | null'`

create(string);
`Argument of type '"string"' is not assignable to parameter of type 'object | null'.`

create(false);
`Argument of type 'false' is not assignable to parameter of type 'object | null'.`

create(undefined);
`Argument of type 'undefined' is not assignable to parameter of type 'object | null'.`
```

{{< linebreak >}}
### Type assertions
- Đây là dạng ép kiểu trong TS

- Type assertions có 2 dạng:
  - One is the as-syntax:
  
  ```ts
  let someValue: unknown = "this is a string";
  
  let strLength: number = (someValue as string).length;
  ```

  - and the other version is the `angle-bracket` syntax:
  
  ```ts
  let someValue: unknown = "this is a string";
  
  let strLength: number = (<string>someValue).length;
  ```