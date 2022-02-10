---
title: "Angular Forms - Reactive Forms and Template-Driven Forms"
date: "2022-02-08T16:00:00+08:00"
draft: false
slug: "angular-forms"
tags: ["Angular"]
post_keywords: "Angular,Angular Forms,React Forms,Template-Driven Forms"
---

寫網頁的過程中免不了一定會出現「表單」，Angular 提供兩種不同的表單建立方式來處理使用者輸入，分別是 reactive forms 和 template-driven forms。

兩種方式都能夠
- 抓取使用者在 view 上的 input events
- 驗證使用者輸數
- 建立 form model 和 data model 給後續表單資料更新
- 建議一套追蹤表單資料變化的機制

而本文將介紹兩種方式的異同和使用方式。

<!--more-->

## Choosing an approach

兩種方式的關鍵差異可由下張表來描述

| | REACTIVE | TEMPLATE-DRIVEN |
| - | -------- | --------------- |
| Setup of form model	| Explicit, created in component class | Implicit, created by directives |
| Data model | Structured and immutable | Unstructured and mutable |
| Data flow	| Synchronous | Asynchronous |
| Form validation	| Functions | Directives |

因為 reactive form 提供直接對接底層的 form API，且 view 和 data model 的更新為同步的，測試時較不須要處理 change detection 問題，因此當我們要建立 large-scale 或複雜的表單時，使用上會比 templare-driven form 來得方便。

## Setting up the form model

Reactive and template-driven forms 共用 building blocks 來追蹤使用者與 form input 元素的互動以及 component 裡的 form data 變化。兩種方式只差別在開發者如何建立和管理 form-control instances。

#### Common form foundation classes

- `FormControl` tracks the value and validation status of an individual form control.
- `FormGroup` tracks the same values and status for a collection of form controls.
- `FormArray` tracks the same values and status for an array of form controls.
- `ControlValueAccessor` creates a bridge between Angular FormControl instances and built-in DOM elements.

##### Setup in reactive forms

(1) Register the reactive forms module

```typescript
import { Component } from '@angular/core';
import { FormControl } from '@angular/forms';

@Component({
  selector: 'app-reactive-favorite-color',
  template: `
    <!-- (3) Register the control in the template -->
    Favorite Color: <input type="text" [formControl]="favoriteColorControl">
  `
})
export class FavoriteColorComponent {
  favoriteColorControl = new FormControl(''); // (2) Generate a new FormControl
}
```

更多常見的使用：
- 使用 `FormGroup` 建立巢狀表單欄位
- 使用 `setValue()` 方法更改單一 `FormControl` 的值
- 使用 `patchValue()` 方法一次更新多個 `FormControl` 的值
- 使用 `FormBuilder` service 使建立表單更容易，不必自行重複處理 form group 和 form control
- 使用 `FormArray` 動態建立長度不固定的 unamed `FormControl`

**在 reactive form 中，form model（`FormControl` instance）是真相來源。它透過 `[formControl]` directive 與 template input element 綁定，提供表單元素的值和狀態。**

#### Setup in template-driven forms

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-template-favorite-color',
  template: `
    Favorite Color: <input type="text" [(ngModel)]="favoriteColor">
  `
})
export class FavoriteColorComponent {
  favoriteColor = '';
}
```

**在 template-driven form 中，form model 是隱含的，是 direcive `NgModel` 為 input element 建立並管理了 `FormControl` instance。**

在此方式中，template 是真相來源，開發者沒有辦法直接修改 `FormControl` instance。

## Data flow in forms

Reactive 和 template-driven forms 處理 view 和 component model 資料更新的流程不同。

#### Data flow in reactive forms

在 reactive forms 中，view 中的表單元素都直接與 form model 相連（a `FormControl` instance）。
從 view 到 model 以及從 model 到 view 的更新是同步的，而且不依賴於 UI 是如何渲染的。

##### How the data flows when an input field's is changed from the view:
1. The user types a value into the input element, in this case the favorite color Blue.
2. The form input element emits an "input" event with the latest value.
3. The control value accessor listening for events on the form input element immediately relays the new value to the `FormControl` instance.
4. The `FormControl` instance emits the new value through the `valueChanges` observable.
5. Any subscribers to the `valueChanges` observable receive the new value.

##### How the data flows when a programmatic change to the model is propagated to the view:
1. The user calls the `favoriteColorControl.setValue()` method, which updates the `FormControl` value.
2. The `FormControl` instance emits the new value through the `valueChanges` observable.
3. Any subscribers to the `valueChanges` observable receive the new value.
4. The control value accessor on the form input element updates the element with the new value.

#### Data flow in template-driven forms

##### How data flows when an input field's value is changed from the view:
1. The user types Blue into the input element.
2. The input element emits an "input" event with the value Blue.
3. The control value accessor attached to the input triggers the `setValue()` method on the `FormControl` instance.
4. The `FormControl` instance emits the new value through the `valueChanges` observable.
5. Any subscribers to the `valueChanges` observable receive the new value.
6. The control value accessor also calls the `NgModel.viewToModelUpdate()` method which emits an `ngModelChange` event.
7. Because the component template uses two-way data binding for the `favoriteColor` property, the `favoriteColor` property in the component is updated to the value emitted by the `ngModelChange` event (Blue).

##### How data flows from model to view when the `favoriteColor` changes from Blue to Red:
1. The favoriteColor value is updated in the component.
2. Change detection begins.
3. During change detection, the `ngOnChanges` lifecycle hook is called on the `NgModel` directive instance because the value of one of its inputs has changed.
4. The `ngOnChanges()` method queues an async task to set the value for the internal `FormControl` instance.
5. Change detection completes.
6. On the next tick, the task to set the `FormControl` instance value is executed.
7. The `FormControl` instance emits the latest value through the `valueChanges` observable.
8. Any subscribers to the `valueChanges` observable receive the new value.
9. The control value accessor updates the form input element in the view with the latest `favoriteColor` value.

#### Mutability of the data model

- Reactive forms 中的資料是 immutable 的，每次 data model 有所改變，`FormControl` instance 都會回傳一個新的 data model 而不是更新現有的。這也讓 change detection 較有效率。
- Template-driven forms 則基於 two-way data binding 在 template 有任何改變時去更新 component data model。

## References

- [Angular - Introduction to Forms](https://angular.io/guide/forms-overview)
