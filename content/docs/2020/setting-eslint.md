---
title: "Setting ESLint to Enhance Frontend Development"
date: "2020-10-29T11:00:00+08:00"
draft: false
slug: "setting-eslint"
tags: ["ESLint", "Workflow"]
post_keywords: "eslint,prettier"
---

Linting tools like [ESLint](https://eslint.org/) allow developers to discover problems with their JavaScript code without executing it.

讓我們來談談如何以 ESLint 改進前端開發流程，且如何挑選 ESLint。

<!--more-->

ESLint can
- To find code problems
- To find patterns that doesn’t adhere to style guides
- Integrate with editors and CI pipeline to auto-check

More ESlint can do
- Customize linting rules
- Fix many problems automatically

What is the difference between linters and prettier?
- See [Setting Prettier to Format Codes](../setting-prettier)

## Select Guides

#### Must have
- [`eslint:recommended`](https://eslint.org/docs/rules/)
- [`plugin:react/recommended`](https://github.com/yannickcr/eslint-plugin-react)

#### More rules selected
- Best practices
  - [error] default-case
  - [error] default-case-last (for version >= 7.0)
  - [error] default-param-last (conflicts with Redux reducer)
  - [warn] eqeqeq
  - [warn] no-empty-function
  - [warn] no-extra-bind
- Stylistic issues
  - [warn] max-depth: 4
  - [warn] max-len: 100
  - [warn] no-multiple-empty-lines: max 2 lines
  - [warn] no-trailing-spaces: ignore comments
- React
  - [error] react/no-access-state-in-setstate
- React Hook
  - [error] react-hooks/rules-of-hooks
  - [warn] react-hooks/exhaustive-deps

---

## Integrate with React Projects

Integrating ESLint with React project helps us to find problems and stick to coding guidelines.

#### Installation

- If you used [create-react-app](https://github.com/facebook/create-react-app) to start a project, ESLint is already provided. It's highly recommended to [extend the default configuration](https://create-react-app.dev/docs/setting-up-your-editor/#extending-or-replacing-the-default-eslint-config).
  - Install `eslint-plugin-react` and `eslint-plugin-react-hooks`
  - Add `.eslintrc` and customize [settings](https://gitlab.rayark.com/backend/phoenix/-/blob/master/admin-frontend/.eslintrc)

#### Customize ESLint Configuration for Project Bootstrapped by Create-React-App

- Step 1: extend ESlint
    - run `touch .env` at project root
    - add `EXTEND_ESLINT=true` to .env
- Step 2: install React related ESlint config
    - run `npm install eslint-plugin-react --save-dev`
    - run `npm install eslint-plugin-react-hooks --save-dev`
- Step 3: set `.eslintrc`
    - run `touch .eslintrc` at project root
    - remove `"eslintConfig"` settings in `package.json`
    - add the following to the file

```
{
  "env": {
    "browser": true,
    "commonjs": true,
    "es2020": true,
    "node": true
  },
  "extends": [
    // "react-app",  // will cause cannot find eslint error
    "eslint:recommended",
    "plugin:react/recommended"
  ],
  "parserOptions": {
    "ecmaFeatures": {
      "jsx": true
    },
    "ecmaVersion": 11,
    "sourceType": "module"
  },
  "plugins": [
    "react",
    "react-hooks"
  ],
  "rules": {
    "default-case": "error",
    "eqeqeq": "warn",
    "no-unused-vars": ["warn" , {
      "vars": "all",
      "args": "after-used",
      "argsIgnorePattern": "^_"
    }],
    "no-empty-function": "warn",
    "no-extra-bind": "warn",
    "max-depth": "warn",
    "max-len": [
      "warn",
      {
        "code": 100
      }
    ],
    "no-multiple-empty-lines": "warn",
    "no-trailing-spaces": [
      "warn",
      {
        "ignoreComments": true,
        "skipBlankLines": true
      }
    ],
    "react/no-access-state-in-setstate": "error",
    "react-hooks/rules-of-hooks": "error",
    "react-hooks/exhaustive-deps": "warn"
  }
}
```

#### 備註
- We hide `react-app` in extends and set `eslint:recommended` by ourselves.
- If the settings do not work
    - delete `node_modules`
    - run `npm install` again
- 若沒有使用 React Hook，不必加上 Hook 相關的 lint

---

## Integrate with VSCode

- Install [extension ESLint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)
- Add following lines to VSCode `settings.json`

```
"eslint.lintTask.enable": true,
"eslint.workingDirectories": "./",  // based on your project structure
```

---

## References
- Popular style guides and ESlint config
  - [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript)（較嚴謹）
    - [eslint-config-airbnb](https://github.com/airbnb/javascript/tree/master/packages/eslint-config-airbnb)
  - [Google JavaScript Style Guide](https://google.github.io/styleguide/jsguide.html)
  - [Javascript Standard Style](https://standardjs.com/readme-zhtw.html)（較寬鬆）
