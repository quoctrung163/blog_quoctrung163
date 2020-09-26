+++
title = "TypeScript for Java/C# Programmers"
date = "2020-09-24"
author = "quoctrung163"
authorTwitter = "quoctrung163" #do not include @
cover = ""
tags = ["ts", "getstated"]
keywords = ["", ""]
description = ""
showFullContent = false
+++

### I. Rethinking Types
- Typescript understanding of the `type` có đôi chút khác biệt với C# và Java qua các điểm sau.

- Types as Sets:
  - Trong C# or Java, thật có ý nghĩa khi nghĩ về sự tương ứng 1-1 giữa runtime-type và compile-time declarations.

  - Trong TS, sẽ tốt hơn khi nghĩ type như tập hợp có các giá trị chung. Bởi vì `types are just sets`, một giá trị cụ thể có thể thuộc nhiều `set` cùng lúc.

  - Làm thế nào để bạn mô tả giá trị thuộc `string set` hay `number set`. => Thật đơn giản khi chỉ cần dùng `unions` => `string | number`

- Erased Structural Types
  - Trong TS, object không thuộc chính xác type nào đó.

  - For example, nếu ta khởi tạo object như một interface, ta có thể sử dụng nó mặc dù ko có quan hệ khai báo :v

  ```ts
  interface Pointlike {
    x: number;
    y: number;
  }

  interface Named {
    name: string;
  }

  function printPoint(point: Pointlike) {
    console.log("x = " + point.x + ", y = " + point.y);
  }

  function printName(x: Named) {
    console.log("Hello, " + x.name);
  }

  const obj = {
    x: 0,
    y: 0,
    name: "Origin"
  }
  ```

- Consequences of Structural Typing
  - Empty Types: 
  ```ts
  class Empty {}
  
  function fn(arg: Empty) {
    // dosomething?
  }

  // No error, but this isn't an 'Empty'?
  fn({k: 10});
  ```

  - TS xác định nếu gọi `fn` có hợp lệ hay ko? bằng cách xem argument (đối số) có phải là `empty valid` (giá trị rỗng hợp lệ). 

  - Bằng cách kiểm tra cấu trúc của `k: 10` và `class Empty { }`.Ta có thể thấy rằng `{ k: 10 }` có tất cả `properties` mà `class Empty { }` có.
  Vì `Empty` không có `properties`. 

  - Do vậy nên nó hợp lệ :D

### II. Identical Types
- Another surprise :v

```ts
class Car {
  drive() {
    // hit the gas
  }
}
class Golfer {
  drive() {
    // hit the ball far
  }
}

// No error?
let w: Car = new Golfer();
```
- Again, this isn't an error because the structures of these classes are they same. :D
