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