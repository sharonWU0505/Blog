---
title: "Validating React Component Props with props-types"
date: "2020-06-22T19:00:00+08:00"
draft: false
slug: "react-props-types"
tags: ["React"]
---

以下內容大多翻譯自 React 文件：[Typechecking With PropTypes](https://zh-hant.reactjs.org/docs/typechecking-with-proptypes.html)

在寫 React component 時經常遇到需要確認 `props` 是否符合型別和資料結構的情況，以確保 component 能如預期運作。除了用 [Flow](https://flow.org/) 或 [TypeScript](https://www.typescriptlang.org/) 去檢查型別之外，React 本身也提供 `propTypes` 屬性用以確認 component 拿到的 `props`。在 function 和 class component 上都可以使用。

不只上述好處，個人認為定義 `propTypes` 也能讓 component 的 interface 更為清楚，使其他開發者一眼就知道該傳入的 `props`，有類似文件的作用。

<!--more-->

> 須注意為了避免效能問題，propTypes 檢查只發生在開發模式。
> 如果發生錯誤，會在 console 中看到警告

### Install props-types

自 React v15.5 後，React.PropTypes 被移出到獨立的 library，因此若要使用須額外安裝。

```
npm install prop-types --save
```

### Supported PropTypes

- Basic data types
  - `PropTypes.any`
  - `PropTypes.bool `
  - `PropTypes.number`
  - `PropTypes.string `
  - `PropTypes.func`
  - `PropTypes.array`
  - `PropTypes.object`
  - `PropTypes.symbol`
- Renderable types
  - `PropTypes.node` :  the prop should be anything that can be rendered by React 
  - `PropTypes.element`: the prop should be a React element
- 也有其他更精確的 operator 可以用，亦能傳入 expression
  - `instanceOf`
  - `oneOf`
  - `oneOfType`
  - `arrayOf`
  - `shape`
- 有能定義是否為 required
  - `isRequired`

### Example (同理 function component)

```
import PropTypes from "prop-types";

class ReactComponent extends React.Component {
  // ...component class body here
}

ReactComponent.propTypes = {
  // ...props-type definitions here
}
```

### Default Prop Values

也可以給予 `props` `defaultProps`。型別檢查會在 `defaultProps` 賦予 `props` 值之後，所以型別檢查也會作用在 `defaultProps` 上。

```
class ReactComponent extends React.Component {
  render() {
    return (
      <div>Hello, {this.props.name}</div>
    )
  }
}

ReactComponent.defaultProps = {
  name: "stranger",
}
```

### Reference

- [PropTypes in React: A complete guide](https://blog.logrocket.com/validating-react-component-props-with-prop-types-ef14b29963fc/)
