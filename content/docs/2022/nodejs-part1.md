---
title: "Learn Node.js - Part 1"
date: "2022-05-06T10:00:00+08:00"
draft: false
slug: "nodejs-part1"
tags: ["Node.js"]
post_keywords: "Node.js,JavaScript"
---

This series is my notes when learning Node.js.

We will focus on fundenmental concepts of Node.js in this post.

<!--more-->

## Node.js

As what it says on the [official website](https://nodejs.org/en/), Node.js is a JavaScript runtime built on Chrome's V8 JavaScript engine.
Let's first look at several keywords here.
- **JavaScript runtime**: It refers to the environment where your JavaScript codes are executed. Runtime provides some objects/utilities, e.g. built-in APIs and filesystem access functionality, to JavaScript so that it can interact with the outside world.
- Chrome V8: It is a free and open-source **JavaScript engine** developed by Chromium Project. The engine translates JavaScript to runnable machine codes so that is can be executed by the CPU. The V8 engine is used in Node.js runtime.

## Characteristics

- Single-threaded, which means it won't create a new thread for every request. However, with this feature, Node.js is not suitable for handling CPU intensive operations.
- Non-blocking and asynchronous I/O. This allows Node.js to handle concurrent connections with a single server.
- Suits both client-side and server-side development
- A rich ecosytem having a large number of libraries

## Installation

We can directly install Node.js package, install Node.js through a package manager, or use `nvm` to install Node.js.
No matte which way, after it is succesfully installed, we should have access to the `node` executable program in the command line.

Ref: https://nodejs.dev/learn/how-to-install-nodejs

## Components

- Node CLI
- NPM
- package.json
- Node Modules
