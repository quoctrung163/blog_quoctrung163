+++
title = "TypeScript for JavaScript Programmers"
date = "2020-09-22"
author = "quoctrung163"
authorTwitter = "quoctrung163" #do not include @
cover = ""
tags = ["ts", "getstarted", "4jsprogrammer"]
keywords = ["", ""]
description = ""
showFullContent = false
+++
I. Types by Inference
- Typescript biết Javascript sẽ tạo ra các type trong cho nhiều trường hợp.

- Ví dụ trong tạo biến và gán nó vào giá trị cụ thể của JS, Typescript sẽ sử dụng giá trị đó như là type.

```ts
let helloWorld = "Hello World";
//  ^ = let helloWorld: string
```

- Bằng cách hiểu cách JS hoạt động, TS có thể xây dựng type-system chấp nhận JS code nhưng có types. Đó là cách TS biết rằng `helloWorld` là chuỗi trong ví dụ trên.

{{< linebreak >}}

II. Defining Types
- Bạn có thể sử dụng nhiều design pattern trong JS. Tuy nhiên, có vài design pattern làm nó trở nên khó bởi vì JS tự động sinh ra type (ví dụ patterns dùng cho dynamic programming).

- Để giải quyết trường hợp này, Typescript có hỗ trợ một số mở rộng cho Javascript, nó cung cấp cho bạn nói với Typescript types nên là gì.

- Ví dụ, để tạo object với infered type có các thuộc tính `name: string` and `id: number`, bạn có thể viết

```ts
const user = {
  name: "Trung",
  id: 0
};
```

- Bạn có thể mô tả rõ ràng object's shape bằng cách khai báo `interface` 

```ts
interface User {
  name: string;
  id: number;
}
```

- Sau đó chỉ cần khai báo đối tượng phù hợp với `interface User` bằng cách sử dụng syntax như `: TypeName` sau một khai báo biến.

```ts
const user: User = {
  name: "Trung",
  id: 0
};
```

- Nếu bạn cung cấp object không phù hợp với interface thì TS sẽ báo lỗi :D

```ts
interface User {
  name: string;
  id: number;
};

const user: User = {
  username: "Trung",
  `Type '{ username: string; id: number; }' is not assignable to type 'User'.
  Object literal may only specify known properties, and 'username' does not exist in type 'User'.`
  id: 0
}
```

- Vì JS hỗ trợ OOP nên TS cũng vậy. Bạn có thể khai báo interface với class :v

```ts
interface User {
  name: string;
  id: number;
}

class UserAccount {
  name: string;
  id: number;

  constructor(name: string, id: number) {
    this.name = name;
    this.id = id;
  }
}

const user: User = new UserAccount("Trung", 3);
```

- Bạn có thể sử dụng interface để chú thích các parameters (tham số) và return values to functions:

```ts
function getAdminUser() : User {
  // return User
}

function deleteUser(user: User) {
  //
}
```

- Có 2 cú pháp để building types: `Interface và Types`. Bạn nên sử dụng `interface`. Sử dụng `type` khi cần các tính năng cụ thể.

III. Composing Types
- Với Typescript, bạn có thể tạo các type phức tạp bằng cách kết hợp những type đơn giản.

- Có 2 cách phổ biến để tạo Composing Types là `Unions` và `Generics`.

- `Unions`:
  - Với Unions bạn có thể khai báo type có thể là nhiều type @@. 
  
  ```ts
  type MyBool = true | false;
  type WindowStates = "open" | "closed" | "minimized";
  type LockStates = "locked" | "unlocked";
  type OddNumbersUnderTen = 1 | 3 | 5 | 7 | 9;
  ```

  - Unions cung cấp cách xử lí nhiều loại types khác nhau. Ví dụ bạn có function nhận một `array` or `string`.

  ```ts
  function getLength(obj: string | string[]) {
    return obj.length;
  }
  ```

- `Generics`:
  - Generics cung cấp varibles cho types. ví dụ phổ biến dùng cho `array`. 

  - An array không có generics có thể chứa bất cứ thứ gì. An array với generics có thể mô tả giá trị mà array đó chứa.

  ```ts
  type StringArray = Array<string>;
  type NumberArray = Array<number>;
  type ObjectWithNameArray = Array<{ name: string }>;
  ```

  - Bạn có thể khai báo types của riêng bạn bằng cách sử dụng generics.

  ```ts
  interface Backpack<Type> {
    add: (obj: Type) => void;
    get: () => Type;
  }

  declare const backpack: Backpack<string>;

  const object = backpack.get();

  backpack.add("Red");

  backpack.add(23);
  // => Argument of type 'number' is not assignable to parameter of type 'string'.
  ```

IV. Structural Type System
- One of TS's core principles (nguyên tắc cốt lõi) là tập trung kiểm tra `shape that value have`. Điều này được gọi là `duck typing` or `structural typing`.

- In a structural type, if 2 objects have the same shape, they are considered to be of the same type.

```ts
interface Point {
  x: number;
  y: number;
}

function printPoint(p: Point) {
  console.log(`${p.x}, ${p.y}`);
}

// prints "12, 26"
const point = { x: 12, y: 26 };
printPoint(point);
```

- The point variable is never declared to be a Point type. However, TypeScript compares the shape of point to the shape of Point in the type-check. They have the same shape, so the code passes.

```ts
const point3 = { x: 12, y: 26, z: 89 };
printPoint(point3); // prints "12, 26"

const rect = { x: 33, y: 3, width: 30, height: 80 };
printPoint(rect); // prints "33, 3"

const color = { hex: "#187ABF" };
printPoint(color);

`Argument of type '{ hex: string; }' is not assignable to parameter of type 'Point'.
  Type '{ hex: string; }' is missing the following properties from type 'Point': x, y
`
```

- Không có sự khác biệt giữa các classes và objects tuân theo `shapes`

```ts
class VirtualPoint {
  x: number;
  y: number;

  constructor(x: number, y: number) {
    this.x = x;
    this.y = y;
  }
}

const newVPoint = new VirtualPoint(13, 56);
pointPoint(newVPoint); // prints "13, 56"
```