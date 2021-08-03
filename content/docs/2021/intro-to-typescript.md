---
title: "Introduction to TypeScript and Primitive Types"
date: "2021-07-30T17:00:00+08:00"
draft: false
slug: "intro-to-typescript"
tags: ["JavaScript, TypeScript"]
post_keywords: "JavaScript,TypeScript"
---

眾所皆知，JavaScript 是一個「弱型別」的語言，意思是：

- 開發者在宣告變數時，不需要事先指定變數的型別 e.g. string, number, boolean, etc.
- JavaScript 會根據變數被賦予的值去判斷它的型別
- 某些情況下，JavaScript 會在背地裡執行「強制轉型」(coercion)，將變數轉換成可以處理的型別

弱型別的設計讓 JavaScript 撰寫起來更方便，但也因此經常衍生出許多開發者預料之外的行為。在較大型、多人協作的專案裡，開發者們逐漸轉向 TypeScript，以期能借用「強型別」的概念，預先有靜態的型別檢查，避免預期外的錯誤。

TypeScript 是 JavaScript 型別的超集，它會被編譯成 JavaScript，並可被瀏覽器執行。目前是一個由微軟維護的[開源專案](https://github.com/Microsoft/TypeScript)。

除了強型別在編譯階段發現錯誤的特性，撰寫 TypeScript 還有一些優點，包括：

- 型別系統作為變數和函式的定義，具有文件的功用，能增加程式碼的可讀性
- 增強了編譯器和 IDE 的功能，例如除錯、提示、自動完成等

而 TypeScript 當然也有缺點，例如：

- 有一定的學習成本，整合專案也需要一些時間，短期內會拉長開發時程
- 非所有第三方函式庫都已支援

不過整體而言，TypeScript 的使用比例都逐漸增加當中，實務上也已顯現出價值，所以還是值得一學。接下來將整理 TypeScript 的基礎。

## 安裝 TypeScript

如果你使用 npm 作為 package 管理工具，那麼可以以下指令在全域安裝 `tsc` 指令：

```
npm install -g typescript
```

執行後，可透過 `tsc --version` 確認是否安裝成功。

## 編譯 TypeScript 檔案

大部分以 TypeScript 編寫的檔案以 `.ts` 為檔名結尾（用 TypeScript 編寫 React 時，以 `.tsx` 為檔名結尾）。

透過執行 TypeScript 檔案，例如：

```
tsc myFile.ts
```

將會得到一編譯完成的 JavaScript (`.js`) 檔案，例如：`myFile.js`。

如果想啟用 watch mode，可以透過 `tsc myFile.ts -w` 進行。

## Primitive Types

由於 JavaScript 的資料型別分成原始型別 (primitive types) 和物件型別 (object types)，所以我們也以此分別介紹它們在 TypeScript 中的應用。

最基本的 TypeScript 定義為：

```
let firstVar: type;
```

若在賦值時型別錯誤，預期會在 terminal 中看到錯誤。

#### String, Number, and Boolean

Primitive types 包括 `string`, `number`, `boolean`, `null`, `undefined`。

我們可以透過以下定義一個型別為 `string` 的變數。定義其他型別的作法基本上相同。

```
let myString: string = "hello world"
```

#### Null and Undefined

`null` 和 `undefined` 就比較特別了，它們是所有型別的子型別。意思是型別為 `null` 或 `undefined` 的變數，可以被賦值給其他變數，例如：

```
let myUndefined: undefined = undefined;
let myBoolean: boolean = myUndefined;
```

#### Void

不過 JavaScript 中就沒有空值 (`void`) 的概念了。在 TypeScript 中，可以用 `void` 表示沒有任何返回值的函式，也可以用來宣告變數，但型別為 `void` 的變數只能被賦值為 `null` 或 `undefined`，所以沒什麼意義。
