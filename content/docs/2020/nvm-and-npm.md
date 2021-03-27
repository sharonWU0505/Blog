---
title: "Introduction to NVM and NPM"
date: "2020-02-07T11:00:00+08:00"
draft: false
slug: "nvm-and-npm"
---

### What is npm and nvm?
- __npm (Node Package Manager)__
  - JavaScript 套件庫管理工具，可以藉由它下載各式各樣的套件
- __nvm (Node Version Manager)__
  - 各套件和專案用的 Node.js 版本不同，而版本間有不相容的問題，導致套件和專案無法順利運行，因此需要版本管理工具
  - 從 nvm 下載/更換 Node.js 和 npm 版本

<!--more-->

### Install nvm (for MacOS)
- Install nvm using *Homebrew*
```
brew install nvm
```

- Add `nvm` command to shell
```
echo "source $(brew --prefix nvm)/nvm.sh" >> .bash_profile
```

- Reload the *bashprofile* file
```
. ~/.bash_profile
```
or
```
source ~/.bash_profile
```

- If using *zsh*, need to add command to *zshrc*
  - `open ~/.zshrc`
  - Add `source ~/.bash_profile` to file, which means load all commands under *bashprofile*
- Check if installation is successful

```
nvm --version
> Example output: 0.35.1
```
or
```
command -v nvm
> output: nvm
```

### Install Node.js and npm by nvm

- Check available remote version
```
nvm ls-remote
```

- Install preferred version
```
nvm install <version>
```

- Use specific version
```
nvm use <version>
> Example output: Now using node v10.17.0 (npm v6.11.3)
```

- See available local versions
```
nvm list
```
