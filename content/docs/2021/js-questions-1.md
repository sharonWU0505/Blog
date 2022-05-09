---
title: "WIP: JavaScript Questions (1)"
date: "2021-09-14T10:00:00+08:00"
draft: true
slug: "js-questions-1"
tags: ["JavaScript"]
post_keywords: "JavaScript"
---

受到 [前端工程師一定要會的 JS 觀念題](https://medium.com/starbugs/%E9%9D%A2%E8%A9%A6-%E5%89%8D%E7%AB%AF%E5%B7%A5%E7%A8%8B%E5%B8%AB%E4%B8%80%E5%AE%9A%E8%A6%81%E6%9C%83%E7%9A%84-js-%E8%A7%80%E5%BF%B5%E9%A1%8C-%E4%B8%AD%E8%8B%B1%E5%B0%8D%E7%85%A7%E4%B9%8B%E4%B8%8A%E7%AF%87-3b0a3feda14f) 的啟發，也想來整理 JavaScript 的常見問題，期望自己未來能用更口語但又不失精確，且搭配範例的方式解釋出這些概念。

第一部分一樣以 **What is ...?** 為主，會先列出幾個常見問題，之後再慢慢補上自己的理解。

<!--more-->

### Questions

> What is scope? What is the difference between `var`, `const`, and `let` on scopes?

> What is hoisting? Do `const` and `let` have hoisting?

> What are primitive types and object types for variables?

> What is IIFE (Immediately Invoked Function Expression)?

IIFE is a JavaScript function which is executed right after it is defined.

An IIFE is composed by two parts:
- (1) an anonymous function wrapped by the grouping operator `()`, by which variables in the function will not pollute the global environment
- (2) an expression `()` for executing the function

```
(function () {
  var aName = "Barry";
})();

// Variable name is not accessible from the outside scope
aName; // throws "Uncaught ReferenceError: aName is not defined"
```

IIFE is also a common design pattern. If you check codes bundled by Webpack, we'll see a lot of IIFE. IIFE prevents creating unnecessary global variables and having naming conflicts, thus it makes benefits on maintaining codes.

> What is closure?

> What is prototype?
