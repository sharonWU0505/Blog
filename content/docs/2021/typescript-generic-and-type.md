---
title: "TypeScript: Generic Types and Type Alias"
date: "2021-08-05T14:00:00+08:00"
draft: false
slug: "typescript-generic-and-type"
tags: ["JavaScript", "TypeScript"]
post_keywords: "JavaScript,TypeScript"
---

此篇要來談談 TypeScript 通用型別 (Generic Types) 的用途，以及 interface 和型別別名 (Type Alias) 的使用差異。

<!--more-->

## 通用型別 (Generic Types)

有時我們想在具有多種型別可能的情況下重用 (reuse) 程式碼，例如以下這個例子：

```typescript
function identity(arg: number): number {
  return arg;
}
```

`identify` 函式收到參數 `arg`，並回傳該參數。如果想更有彈性地使用函式，比方說不限定參數型別，則我們可以使用 `any`：

```typescript
function identity(arg: any): any {
  return arg;
}
```

但如此一來我們便無法限制回傳值的型別要等同輸入值的型別，這時正是使用通用型別的時機。

我們使用 type variable `T` 來代表型別，限制了函式輸入和輸出值的型別必須相同：

```typescript
function identify<T>(arg: T): T {
  return arg;
}
```

使用 **type variable** 時，因為無從事先得知變數的型別，所以無法操作屬性或方法，例如以下範例將會收到錯誤：

```typescript
function identify<T>(arg: T): T {
  console.log(arg.length); // error: Property 'length' does not exist on type 
  return arg;
}
```

不過如果是：

```typescript
function loggingIdentity<T>(args: T[]): T[] {
  console.log(args.length);
  return args;
}
```

將不會有問題，因為 arrays of type `T` 確定有 `length` 屬性。

通用型別當然也可以跟 interface, class, tuple 等搭配運用，詳細使用方式請見官方[文件](https://www.typescriptlang.org/docs/handbook/2/generics.html)。

## 介面 (Interface) 與型別別名 (Type Alias) 的異同

型別別名在使用上和 interface 非常相似。

#### 相似之處

`type` 和 `interface` 一樣可以用來定義物件格式：

```typescript
type Animal = {
  name: string
}
```

`type` 可使用 intersections 延伸屬性（`interface` 則通常會使用 `extends`)

```typescript
type Animal = {
  name: string
}

type Bear = Animal & { 
  honey: boolean 
}

const bear = getBear();
bear.name;
bear.honey;
```

#### 不同之處

> Aliasing doesn’t actually create a new type - it creates a new name to refer to that type. Aliasing a primitive is not terribly useful, though it can be used as a form of documentation.

根據[官方文件](https://www.typescriptlang.org/docs/handbook/declaration-files/by-example.html#reusable-types-type-aliases)，`type` 看起來更推薦用在單純要表示偏靜態的資料格式之時：

```typescript
// declaration
type GreetingLike = string | (() => string) | MyGreeter;

declare function greet(g: GreetingLike): void;

// usage
function getGreeting() {
  return "howdy";
}

class MyGreeter extends Greeter {
  ...
}

greet("hello");
greet(getGreeting);
greet(new MyGreeter());
```

## References

- https://www.typescriptlang.org/
- https://willh.gitbook.io/typescript-tutorial/
