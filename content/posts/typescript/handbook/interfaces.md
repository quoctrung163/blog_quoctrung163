+++
title = "Interfaces in TS"
date = "2020-09-27"
author = "quoctrung163"
authorTwitter = "quoctrung163" #do not include @
cover = ""
tags = ["ts", "handbook"]
keywords = ["", ""]
description = ""
showFullContent = false
+++

- One of Typescript's core principles is that type checking focuses on the shape that values have. This is sometimes called "duck typing" or "structural subtyping".

- In TS, interfaces fill the role of naming these types, and are a powerful way of defining contracts within your code as well as contracts with code outside of you project.

{{< linebreak >}}
### Our First Interface
- simple example:

```ts
function printLabel(labeledObj: { label: string }) {
  console.log(labeledObj.label);
}

let myObj = { size: 10, label: "Size 10 Object" };
printLabel(myObj);
```

- we can write the same example again, this time using an interface to describe the requirement of having the label property that is a string:

```ts
interface LabeledValue {
  label: string;
}

function printLabel(labeledObj: LabeledValue) {
  console.log(labeledObj.label);
}

let myObj = { size: 10, label: "Size 10 Object" };
printLabel(myObj);
```

{{< linebreak >}}
### Optional Properties
- Đôi khi chúng ta không nhất phải bắt buộc phải có tất cả property và để thể hiện `property không bắt buộc` ta có thể thêm `?` vào cuối biến đó.

```ts
interface SquareConfig {
  color?: string;
  width?: number;
}

function createSquare(config: SquareConfig): { color: string; area: number } {
  let newSquare = { color: "white", area: 100 };
  
  if (config.color) {
    newSquare.color = config.color;
  }

  if (config.width) {
    newSquare.area = config.width * config.width;
  }

  return newSquare;
}

let mySquare = createSquare({ color: "black: });
```

### Readonly properties
- Một số property chỉ nên sửa đổi khi object được tạo đầu tiên. Bạn có thể chỉ định bằng cách dùng `readonly` trước tên của property.

```ts
interface Point {
  readonly x: number;
  readonly y: number;
}
```

- sau khi đặt `readonly` thì các property không thể sửa đổi :v

```ts
let p1: Point = { x: 10, y: 20 };
p1.x = 4; // Error~
`Cannot assign to 'x' because it is a read-only property.`
```

- ts come with a `ReadonlyArray<T>` type that is the same as `Array<T>` with all mutating methods removed, so you can make sure you don't change your after creation:

```ts
let a: number[] = [1, 2, 3, 4, 5];
let ro: ReadonlyArray<number> = a;

ro[0] = 12; // error!
`Index signature in type 'readonly number[]' only permits reading.`
```

`readonly vs const`
- the easiest way to remember whether to use `readonly` or `const` is to ask whether you're using it on variable or property.

- Variables use `const` and Properties use `readonly` :D

{{< linebreak >}}

### Excess Property Checks
- Khi kết hợp giữa `interface` và `property không bắt buộc` có thể sẽ xảy ra một vài cạm bẫy sau:

```ts
interface SquareConfig {
  color?: string;
  width?: number;
}

function createSquare(config: SquareConfig): { color: string; area: number} {
  return { 
    color: config.color || "red",
    area: config.width ? config.width*config.width : 20  
  }
}

let mySquare = createSquare({ colour: "red", width: 100 });
`Argument of type '{ colour: string; width: number; }' is not assignable to parameter of type 'SquareConfig'.
  Object literal may only specify known properties, but 'colour' does not exist in type 'SquareConfig'. Did you mean to write 'color'?`
```

- Vì `color` không bắt buộc nên trường hợp này Object truyền vào sẽ chỉ có thuộc tính là `width`

- Để tránh những nhầm lẫn này xảy, TS sẽ check riêng object. Cụ thể khi gán cho biến khác hoặc khi đã truyền tham số vào hàm sẽ xảy ra lỗi không tồn tại thuộc tính có kiểu của đối tượng.

```ts
let mySquare = createSquare({ colour: "red", width: 100 });
`Argument of type '{ colour: string; width: number; }' is not assignable to parameter of type 'SquareConfig'.
  Object literal may only specify known properties, but 'colour' does not exist in type 'SquareConfig'. Did you mean to write 'color'?`
```

- Để bỏ qua check như trên chỉ cần ép kiểu là được

```ts
let mySquare = createSquare({
  width: 100,
  opacity: 0.5
} as SquareConfig)
```

- Còn một cách tốt hơn để nhận được property ngoài là thêm index là 1 biến kiểu chuỗi:

```ts
interface SquareConfig {
  color?: string;
  width?: number;
  [propName: string]: any;
}
```

- Mặc dù đang được chỉ định thêm `propName` nhưng không hẵn có thể truy cập đến những property khác nữa. Sẽ được nói rõ hơn ở phần `Indexable Types` phía dưới:

- Nếu chúng ta chỉ gán biến 1 lần sẽ chỉ mất 2 dòng code.

```ts
let squareOptions = {
  colour: "red",
  width: 100
};
let mySquare = createSquare(squareOptions);
```

- Có thể bạn nghĩ viết thế này đơn giản hơn rất nhiều nhưng khi code trở lên phức tạp hơn chúng ta cần phải sử dụng kỹ thật trên. Tức là trong nhiều trường hợp chúng ta phải ý thức trước được là sẽ có những thuộc tính dư thừa nữa.

{{< linebreak >}}

### Function Types
- Interface ngoài việc thể hiện các `Object` có những thuộc tính đặc trưng ra còn có thể thể hiện được cả `method`.

```ts
interface SearchFunc {
  (source: string, subString: string): boolean;
}
```

- one defined, we can use this function type interface like we would other interfaces.

```ts
let mySearch: SearchFunc;

mySearch = function (source: string, subString: string) {
  let result = source.search(subString);
  return result > -1;
}
```
{{< linebreak >}}

### Indexabel Types
- Tương tự như cách ta có thể sử dụng interface để mô tả `function-types`, ta cũng có thể `index into` như a[10], or ageMap["daniel"]. 

- Indexabel types have an index signture that describes the types we can use to index into the object, along with sinature the corresponding return types when indexing.

```ts
interface StringArray {
  [index: number]: string;
}

let myArray: StringArray;
myArray = ["Bob", "Fred"];

let myStr: string = myArray[0];
```

{{< linebreak >}}

### Class Types
- Implementing an interface

- one of the most common use of interface in languages like c# and java, that explicity enforcing that a class particular contract, it also possibel in Typescript.

```ts
interface ClockInterface {
  currentTime: Date;
}

class Clock implements ClockInterface {
  currentTime: Date = new Date();
  constructor(h: number, m: number) {}
}
```

- You can also describe methods in an interface that are implemented in the class, as we do with setTime in the below example:

```ts
interface ClockInterface {
  currentTime: Date;
  setTIme(d: Date): void;
}

class Clock implements ClockInterface {
  currentTime: Date = new Date();
  setTime(d: Date) {
    this.currentTime = d;
  }
  constructor(h: number, m: number) {}
}
```

{{< linebreak >}}

### Extending Interfaces
- Like classes, interface can extend each other. This alows you to copy the members of one interface into another, which gives you more flexibility in how you separate your interface into reusable components.

```ts
interface Shape {
  color: string;
}

interface Square extends Shape {
  sideLength: number;
}

let square = {} as Square;
square.color: "blue";
square.sideLength = 10;
```

- An interface can extend multiple interface, creating a combination of all of the interfaces.

```ts
interface Shape {
  color: string;
}

interface PenStoke {
  penWidth: number;
}

interface Square extends Shape, PenStroke {
  sideLength: number;
}

let square = {} as Square;
square.color = "blue";
square.sideLength = 10;
square.penWidth = 5.0;
```

{{< linebreak >}}

### Hybrid Types

- As we mentioned earlier, interfaces can describe the rich types present in real world JS. Because of JS dynamic and flexible nature, you may occasionlly encounter an object that works as a combination of some of the types described above.

```ts
interface Counter {
  (start: number): string;
  interval: number;
  reset(): void;
}

function getCounter(): Counter {
  let counter = function (start: number) {}
 as Counter;
  counter.interval = 123;
  counter.reset = function () {};
  return counter;
}

let c = getCounter();
c(10);
c.reset();
c.interval = 5.0;
```