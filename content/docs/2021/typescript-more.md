---
title: "TypeScript: Any, Array, Union Types, Enum, Function, and Interface"
date: "2021-08-03T10:00:00+08:00"
draft: false
slug: "typescript-more"
tags: ["TypeScript"]
post_keywords: "JavaScript,TypeScript"
---

此篇繼續來看 TypeScript 的基本語法。

<!--more-->

## Any

在 TypeScript 中，可用 `any` 來表示允許賦值是任意型別。

如果使用普通型別，在賦值過程中改變型別是不被允許的，但如果是 `any` 型別，則允許。以下程式將能順利被編譯。

```typescript
let myNumber: any = 'nine';
myNumber = 9;
```

## 型別推論

如果沒有明確地指定型別，那麼 TypeScript 會依照型別推論（Type Inference）的規則推斷出型別。例如：

```typescript
let myNumber = 'nine';
myNumber = 9;
```

編譯以上程式碼會收到錯誤。原因是 TypeScript 遇到賦值時會推斷出變數的型別是 `string`，爾後重新賦值時不允許改變型別。

但如果宣吿變數時沒有指定型別也沒有賦值，則它會被推斷成任意型別 (`any`)，所以以下程式碼會通過編譯。

```typescript
let myNumber;
myNumber = 'nine';
myNumber = 9;
```

## Array

宣告 Array 型別其實很單純：

- an array of number: `number[]`
- an array of string: `string[]`

```typescript
const numArray: number[] = [1, 2, 3];
```

## Union Types

聯合型別（Union Types）表示取值可以為多種型別中的一種。

```typescript
let myNumber: string | number;
myNumber = 'nine';
myNumber = 9;
```

當 TypeScript 不確定一個聯合型別的變數到底是哪一個型別時，我們只能存取所有型別裡共有的屬性或方法。

```typescript
function getLength(something: string | number): number {
  return something.length; // error: 因為 number 沒有 `length` 屬性
}
```

```typescript
function getLength(something: string | number): number {
  return something.toString(); // works
}
```

## Enum (列舉)

Enum 型別經常被用在變數值有一定範圍的場景，例如一週 7 天、篩選選項等等。

我們用 `enum` 關鍵字來定義變數：

```typescript
enum Filters {
  ALL,
  ACTIVE,
  PAST_DUE,
}
```

列舉的項目會被賦值從 0 開始遞增的數字，同時也會有數字到列舉值的反向對映，所以如果我們印出 `Filters`，會看到：

```typescript
{
  '0': 'ALL',
  '1': 'ACTIVE',
  '2': 'PAST_DUE',
  ALL: 0,
  ACTIVE: 1,
  PAST_DUE: 2,
}
```

我們可用 `Filters.PAST_DUE` 來取得列舉項目的值。

Enum 還有更多應用，包括手動賦值、常數列舉等等，有興趣的話可再去閱讀[官方文件](https://www.typescriptlang.org/docs/handbook/enums.html#numeric-enums)。

## Function

定義函式時，要使用 TypeScript 約束函式的輸入和輸出，例如：

#### Normal Function

```typescript
function sum(num1: number, num2: number): number {
  return num1 + num2;
}
```

#### Arrow Function

```typescript
const sum = (num1: number, num2: number): number => {
  return num1 + num2;
}

sum(1, 2);     // works
sum(1, 2, 3);  // works: more arguments are ignored
```

#### Optional Parameters

如果某參數為 optional，可用 `?` 來處理：

```typescript
function f(x?: number) {
  // ...
}

f(); // OK
f(10); // OK
```

## Interface

在 TypeScript 中，我們使用 interface 來定義物件 (object) 的型別或形狀。以下是一個簡單的例子：

```typescript
interface Person { // interface naming 通常首字母是大寫
  name: string;
  gender: string;
}

let Amy: Person = {
  name: 'Amy',
  age: 25
};
```

可以使用 `readonly` property 來定義唯讀的物件屬性：

```typescript
interface Person {
  readonly name: string;
  gender: string;
}
```

interface 的更多應用請見[官方文件](https://www.typescriptlang.org/docs/handbook/2/objects.html)。

## References

- https://www.typescriptlang.org/
- https://willh.gitbook.io/typescript-tutorial/
