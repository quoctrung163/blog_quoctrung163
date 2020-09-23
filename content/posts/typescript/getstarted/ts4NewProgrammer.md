+++
title = "TypeScript for the New Programmer"
date = "2020-09-22"
author = "quoctrung163"
authorTwitter = "" #do not include @
cover = ""
tags = ["ts", "getstarted", "4newprogrammer"]
keywords = ["", ""]
description = ""
showFullContent = false
+++

I. Những cái ngớ ngẫn của Javascript {{< emoji ":cat:" >}}
- Toán tử == ép kiểu các đối số của nó => hành vi ko mong muốn.
```js
if ("" == 0) {
  // why ?
}

if (1 < x < 3) {
  // true for *any* value of x!
}
```

- JS cho phép truy cập underfind property
```js
const obj = { width: 10, height: 15 };
const area = obj.width * obj.cao; 
// => NAN ?
```

- Hầu hết các ngôn ngữ sẽ gặp lỗi khi gắp những TH này. Nhưng JS {{< emoji ":pensive:" >}}.
- Đối với ứng dụng nhỏ thì có thể dễ quản lý nhưng khi ứng dụng to thì sẽ gặp những lỗi không mong muốn.

{{< linebreak >}}
II. TypeScript: A static type checker
- Typescript xác định lỗi mà không cần chạy chương trình

- ex: 
```js
const obj = { width: 10, height: 15 };
const area = obj.width * obj.cao; 
// => Property 'cao' does not exist on type '{ width: number; height: number; }'. Did you mean 'height'?
```

III. Là Typed Superset của Javascript
- TypeScript là ngôn ngữ tập siêu của Javascript nên cú pháp của JS sẽ hoàn toàn hợp lệ đối với TS