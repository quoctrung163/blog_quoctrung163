+++
title = "TypeScript Tooling in 5 minutes"
date = "2020-09-27"
author = "quoctrung163"
authorTwitter = "" #do not include @
cover = ""
tags = ["ts", "getstarted"]
keywords = ["", ""]
description = ""
showFullContent = false
+++

### Installing Typescript
- Có 2 cách chính để cài TS
  - với npm 
    `npm install -g typescript`
  - or TS Visual Studio plugins

{{< linebreak >}}
### Building your first TS file
- In your editor, type the following JS code in greeter.ts

```ts
function greeter(person) {
  return `Hello, ${person}`;
}

let user = "Quoc Trung";
document.body.textContent = greeter(user);
```
{{< linebreak >}}
### Compiling your code
- At the command line, run the TS compiler:

```ts
tsc greeter.ts
```

- The result will be a file `greeter.js` and type `node greeter.js` to display result.

{{< linebreak >}}
### Interface 
```ts
interface Person {
  firstName: string;
  firstName: string;
}

function greeter(person: Person) {
  return `Hello, ${person.firstName} ${person.lastName}`;
}

let user = { firstName: "Trung", lastName: "Phan" };
document.body.textContent = greeter(user);
```

{{< linebreak >}}
### Classes

```ts
class Student {
  fullName: string;
  constructor(
    public firstName: string,
    public middleInitial: string,
    public lastName: string
  ) {
    this.fullName = firstName + " " + middleInitial + " " + lastName;
  }
}
```
