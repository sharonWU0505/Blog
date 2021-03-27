---
title: "Install Golang and Set Go Workplace"
date: "2020-04-16T15:00:00+08:00"
draft: false
slug: "install-and-set-go"
---

__[Go](https://golang.org/)__ or called __GoLang__ is an open source programming language that makes it easy to build simple, reliable, and efficient software.

In this post, we'll talk about how to install Golang and set its workplace. Also, sub directories under the workplace are introduced.

<!--more-->
## Install Go
- For MacOS, using *Homebrew*
```
brew install go
```
## Set Go Workplace

> A workspace is Go’s way to facilitate project management.

> A workspace in nutshell, **is a directory on your system where Go looks for source code files, manages dependency packages and build distribution binary files.** 
You can have as many workspaces as you want, as long as you keep GOPATH environment variable pointed to current working workspace directory.

> Reference: [Getting started with Go](https://medium.com/rungo/working-in-go-workspace-3b0576e0534a)

### Default Workplace
```
$HOME/go
```

### Customize Your Go Workplace
#### _Permanently_
- bash
    - open file: `open ~/.bash_profile`
    - edit file: add `export GOPATH=$HOME/<customized-path>/go` to the file
    - reload: `source ~/.bash_profile`
- zsh
    - open file: `open ~/.zshrc`
    - edit file: add `export GOPATH=$HOME/<customized-path>/go` to the file
    - reload: `source ~/.zshrc`
- others
    - see [SettingGOPATH](https://github.com/golang/go/wiki/SettingGOPATH)

#### _Temporarily_
- For MacOS
    - see [Setting up Environment Variables in MacOS Sierra](https://medium.com/@himanshuagarwal1395/setting-up-environment-variables-in-macos-sierra-f5978369b255)

---
## Sub Directories under a Go Workplace

### src
- Containing packages (also packages installed by `go get` command)
- Whenever working with a new Go project, a new directory for the project should be created under `$GOPATH/src`.

### pkg
- Containing Go package objects, which are compiled version of original package source code
- When a Go program hits `import` statement, Go looks for the followings in order.
  - pre-compiled package object
  - corresponded packages in `$GOPATH/src`
  - corresponded packages in `$GOROOT/src`

### bin
- Containing binary executable files for executing Go programs, which are created by `go install` commands
- `$GOBIN` is an environment variable that Go uses to put binary files. By default, `$GOBIN` is `$GOPATH/bin` but you can change it, too.
