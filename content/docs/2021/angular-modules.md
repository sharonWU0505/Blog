---
title: "Angular - Introduction to Modules"
date: "2021-09-01T13:00:00+08:00"
draft: false
slug: "angular-intro-to-modules"
tags: ["Angular", "Angular Basic Concepts"]
post_keywords: "Angular,Angular Modules"
---

最近由於工作需要開始學習 Angular，過程中發現雖然 Angular 也是 component-based 的 SPA (Single Page Application) 框架，卻多了一層 Module (模組) 的概念。若是從 React 過來的開發者，可能會對這個概念有點陌生，所以我們就來看看 Angular Modules 是什麼吧～

<!--more-->

## Basics

Angular 應用是模組化的，它的模組系統叫做 ***NgModules***。其實 NgModules 的定義和作用與大家經常在程式設計中聽到的模組/模組化相去不遠，它是由數個具備基礎功能的元件所組成的特定功能組件，經過組合 (composition) 形成具有完整功能的系統或程式。

NgModules 系統與 JavaScript（ES2015/ES6）用來處理 JavaScript 物件的模組系統（import & export）不同，也沒有直接的關聯。但這兩種系統互補，撰寫 Angular 的過程中一定會共同使用兩者。

以 Angular 的設計來說，一個應用至少會有一個 `NgModule` class，也就是 ***Root Module（根模組）***。基於開發習慣，root module 通常會被命名為 `AppModule`，並被定義在 `app.module.ts` 中。如果要啟用 Angular 應用，那麼就要 bootstrap root module。

一個 Angular module 包含 *components*, *service providers* 等其他要在該 module 下作用的程式檔案。Module 可以引用其他 module 匯出的功能，也可以匯出指定功能讓其他 module 使用。

## NgModule Metadata

我們使用被 `@NgModule()` *decorator* 裝飾的 class 來定義 NgModule。 `@NgModule()` 是一個**接收單一 metadata JavaScript object 的函式**，object 描述了 module 的屬性，以下介紹各屬性的用途：

- `declarations`: scope 屬於此 module 的 components、directives 和 pipes
- `exports`: 能在其他模組之 component templates 中被使用的 `declarations` 的子集
- `imports`: 那些匯出此 module 的 component templates 所需 classes 的 module
- `providers`: 此 module 向 global services 貢獻的 service creators，能被應用中的任何部分使用。也可以在 component-level 再指名 `providers`。
- `bootstrap`: 應用中的 main application view，又稱為 *Root Component*，是應用中其他 views 的 *host (宿主)*。**只有 root NgModule 需要有此 `bootstrap` 屬性。**

這是一個簡單的 root NgModule 範例：

```jsx
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';

@NgModule({
  imports:      [ BrowserModule ], // BrowserModule is for connecting to DOM.
  providers:    [ Logger ],
  declarations: [ AppComponent ],
  exports:      [ AppComponent ],  // In reality, we don't need to export here, because no any other modules will need to import root module.
  bootstrap:    [ AppComponent ]
})
export class AppModule { }
```

## NgModules and Components

那麼 module 和 component 之間的關係是什麼呢？

NgModule 為它的 components 提供一個 *compilation context（*編譯上下文環境*）*。Root module 總會有一個 root component，也能再包含任意數量的其他 components，他們會共享一個 compilation context*。*

而 component 和 component template 組成 view，view 能具有階層結構（hierarchy），此結構可以混合使用由不同 NgModule 中的 component 定義的 view。

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5fc123a9-d87c-48a4-8e83-3fe746d84a39/Untitled.png)

如上圖所示，此 view 階層由來自兩個 NgModule（由藍色和橘色區分）的 views 組合而成。

## References

- [Introduction to Angular concepts](https://angular.io/guide/architecture)
- [Introduction to modules](https://angular.io/guide/architecture-modules)
- [NgModule 簡介]([https://angular.tw/guide/architecture-modules](https://angular.tw/guide/architecture-modules))
