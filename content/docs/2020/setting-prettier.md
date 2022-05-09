---
title: "Setting Prettier to Format Codes"
date: "2020-11-01T11:00:00+08:00"
draft: false
slug: "setting-prettier"
tags: ["Tools"]
post_keywords: "eslint,prettier"
---

[Prettier](https://prettier.io/) is an opinionated **code formatter** supporting several languages. The biggest reason for adopting Prettier is to stop all the on-going debates over styles. Therefore, applying Prettier to frontend development flow can save much time on formatting codes and align coding styles.

In my opinion, ESLint and Prettier are must tools for every frontend projects (not only personal, but also group ones).

<!--more-->

## Prettier vs. Linters
Linters have two categories of rules:

- Formatting rules: eg: `max-len`, `no-mixed-spaces-and-tabs`, `keyword-spacing`, `comma-style`, …
- Code-quality rules: eg: `no-unused-vars`, `no-extra-bind`, `no-implicit-globals`, `prefer-promise-reject-errors`, …

Prettier covers all needs of formatting codes, but it does nothing with checking code qualities.
In other words, **use Prettier for formatting and linters for catching bugs!**

## Usage

### Global Settings
- Install: `npm install -g prettier@2.2.1`
  - should lock the version, because default rules differ by releases
  - check the version: `npm list -g prettier`
- Create a `.prettierrc` file
  - `cd` (to root path)
  - `touch .prettierrc`
- Add rules to `~/.prettierrc`
  - open the file: `open ~/.prettierrc`
  - add following lines and save it

```
{
  "printWidth": 100,
  "tabWidth": 2,
  "semi": true,
  "trailingComma": "es5",
  "jsxBracketSameLine": true
}
```

- (optional) Create a `.prettierignore` file
- Format certain files: `prettier --write ${file-path-or-pattern}`

### Integrate with VSCode
- Install [Prettier - Code formatter](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode) plugin (is using `prettier@2.2.1`)
- Open `settings.json`, add following lines and save it
- Format current file: `cmd+shift+p` > `Format Document`

#### For Prettier
```
"editor.defaultFormatter": "esbenp.prettier-vscode",
"editor.formatOnSave": true,  // default
```

#### Other Settings
```
"editor.rulers": [
  100
],
"editor.tabSize": 2,                                     // optional
"editor.renderWhitespace": "boundary",                   // recommended
"javascript.updateImportsOnFileMove.enabled": "always",  // recommended
```

### Local Settings by Projects

If a project has its own Prettier config file, the configuration will be applied.
Prettier searches recursively up the file path, it will fallback to global settings if local configuration is not found.

## Configuration
- See [options](https://prettier.io/docs/en/options.html)

## References
- https://prettier.io/
- https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode
- https://hackmd.io/@chupai/HkNT0IMhr
