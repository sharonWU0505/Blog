---
title: "Validating React Component Props with props-types"
date: "2021-03-20T19:00:00+08:00"
draft: false
slug: "react-props-types"
---
### Using propTypes
* Reference: [Validating React Component Props with prop-types](https://blog.logrocket.com/validating-react-component-props-with-prop-types-ef14b29963fc/)
* Note that, propTypes type-checking only happens in **development mode**, enabling you to catch bugs in your React application while developing. For performance reasons, it is not triggered in production environment.

<!--more--> 

##### Installation
```
npm install prop-types --save
```
* In older version of React, `PropTypes` can be accessed by `React.PropTypes`

##### Available Validators
* Basic data types
    * `PropTypes.any`
    * `PropTypes.bool `
    * `PropTypes.number`
    * `PropTypes.string `
    * `PropTypes.func`
    * `PropTypes.array`
    * `PropTypes.object`
    * `PropTypes.symbol`
* Renderable types
    * `PropTypes.node` :  the prop should be anything that can be rendered by React 
    * `PropTypes.element`: the prop should be a React element

##### Example

```
import PropTypes from "prop-types";

# method 1
class ReactComponent extends React.Component {
  // ...component class body here
}

ReactComponent.propTypes = {
  // ...prop type definitions here
}

# method 2 (using `static` class properties syntax)
class ReactComponent extends React.Component {
  // ...component class body here
  
  static propTypes = {
    // ...prop type definitions here
  }
}
```
