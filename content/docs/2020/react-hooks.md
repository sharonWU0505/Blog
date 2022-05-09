---
title: "Notes for React Hooks"
date: "2020-01-23T09:00:00+08:00"
draft: false
slug: "react-hooks"
tags: ["React"]
post_keywords: "react,react hook,react hooks"
---

React proposes Hook in version 16.8. It allows developers to use state and methods in component life cycle without writing class components. Additionally, there is no breaking changes for Hook. It is completely opt-in and 100% backwards-compatible.

The following are my notes when learning React Hook.

<!--more-->

## Motivation

#### It’s hard to reuse **stateful logic** between components.

Traditional approaches are: [render props](https://zh-hant.reactjs.org/docs/render-props.html), [higher-order component (HOC)](https://zh-hant.reactjs.org/docs/higher-order-components.html), and [context](https://zh-hant.reactjs.org/docs/context.html)

We have to restructure components when applying each approach.

#### Complex components become hard to understand.

Unrelated actions such as fetching data and setting event listeners are written in the same life cycle. Thus, it's hard to split code and logic.

#### Classes confuse both people and machines.

Writing functions are easier than writing classes.

## Introduction

Hooks are functions that let you “hook into” React state and lifecycle features from **function components**.

## Using the State Hook

#### Syntax

```javascript
import React, { useState } from 'react';  // import the useState Hook from React

function Example() {
  const [count, setCount] = useState(0);  // declare a state variable and a function for updating state

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

#### Tips

- Use square brackets: `const [count, setCount] = useState(0)`
- Use multiple state variables: State variables can hold objects and arrays just fine, so you can still group related data together. However, unlike this.setState in a class, **updating a state variable always replaces it instead of merging it**.

## Using the Effect Hook

> [A Complete Guide to useEffect](https://overreacted.io/a-complete-guide-to-useeffect/#speaking-of-race-conditions)

The Effect Hook lets you **perform side effects** in function components. `useEffect` can be thought of as `componentDidMount`, `componentDidUpdate`, and `componentWillUnmount` combined of React class lifecycle.

#### Effects Without Cleanup
- `useEffect` by default runs both after the first render and after every update.

```javascript
function Example() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    document.title = `You clicked ${count} times`;
  });
}
```

#### Effects With Cleanup

- class component: We typically set up a subscription in `componentDidMount`, and clean it up in `componentWillUnmount`. Lifecycle methods force us to split this logic.

```javascript
useEffect(() => {
  function handleStatusChange(status) {
    setIsOnline(status.isOnline);
  }

  ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);

  // Every effect may return a function that cleans up after it. This lets us keep the logic for adding and removing subscriptions close to each other.
  return () => {
    ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
  };
});
```

#### Tips

- Use multiple effects to separate concerns
- Effects by default will run on each re-render. We should think of the use case.
- Optimize performance by skipping effects.
   ``` javascript
   useEffect(() => {
     document.title = `You clicked ${count} times`;
   }, [count]); // Only re-run the effect if count changes
   ```
- If you want to run an effect and clean it up only once (on mount and unmount), you can pass an empty array (`[]`) as a second argument. This tells React that your effect doesn’t depend on any values from props or state, so it never needs to re-run.

## Rules of Hooks

- Only call Hooks at the top level (React relies on the order in which Hooks are called.)
- Only call Hooks from React functions
- [More rules](https://reactjs.org/docs/hooks-rules.html)

## Extended Topics

- [Building Your Own Hooks](https://reactjs.org/docs/hooks-custom.html) (should start with `use`)
- [Hooks API Reference](https://reactjs.org/docs/hooks-reference.html)
- [Hooks FAQ](https://reactjs.org/docs/hooks-faq.html)

## Discussion

#### 與 Redux 的搭配方式

- 跨多個 container/component 的 state 仍可用 Redux 管理
- 涉及 state 且需要重用的邏輯可以包成 Hook，例如權限判斷，不再需要使用 HOC, Context（較難寫，也會讓程式碼變複雜）
- component 可進一步拆分，寫成 function component 搭配 Hook 處理 state，再組裝，以避免過肥的 component

#### 開始使用 Hook

- 不需要將現存的 class component 轉變為 function component + Hook（by React 官方）
- 從新功能開始，試用 Hook，並了解原生 Hook, ex: `useState`, `useEffect`, `useCallback`, etc. 的使用方式。

## 延伸閱讀

**Ephemeral State and App State**
- Ephemeral state (sometimes called UI state or local state) is the state you can neatly contain in a single widget.
- State that is not ephemeral, that you want to **share across many parts of your app**, and that you want to keep between user sessions, is what we call application state (sometimes also called shared state).

[source](https://flutter.dev/docs/development/data-and-backend/state-mgmt/ephemeral-vs-app)
