---
title: Vue CLI
date: 2018-07-03 20:27:51
author: One-Lemon
top: true
cover: true
summary: 
tags:
	- Vue
	- JS
	- demo
---

# Vue CLI 简单的小案例

> Vue CLI 也叫做脚手架工具，前端中一定会用到打包工具 webpakck 但是繁琐的配置问题让人十分头疼，但是 Vue CLI 提供了很好的快速打包配置功能。有着丰富的官方插件，并且还有图形化的创建和管理 Vue 项目的用户界面，可以说真的是 webpack 手残党的福星。

## 安装

> **Node 版本要求**
>
> Vue CLI 需要 [Node.js](https://nodejs.org/) 8.9 或更高版本 (推荐 8.11.0+)。你可以使用 [nvm](https://github.com/creationix/nvm) 或 [nvm-windows](https://github.com/coreybutler/nvm-windows) 在同一台电脑中管理多个 Node 版本。

使用以下命令行进行安装：

```sh
npm install -g @vue/cli
# 如果你是用的是 yarn
yarn global add @add/cli
```

如何测试自己是否安装成功：

```sh
vue --version
```

## 创建项目

### 命令方式

1. 通过以下命令创建一个 Vue 项目：

   ```SH
   vue create vue-demo
   ```

2. 选择一个配置的预设方案（回车确键认下一步）：

   ```sh
   Vue CLI v3.8.2
   ? Please pick a preset:    # 选择预设方案
   > hello-config (less, babel, eslint)	# 我之前就配置好并且保存的，后面会遇到的。
     default (babel, eslint)	# 默认设置 
     Manually select features	# 手动选择，这里我们选择这个(你也可以使用默认设置)
   ```

3. 手动选择预设功能（空格键选择，`a`键全选）：

   ```sh
   Vue CLI v3.8.2
   ? Please pick a preset: Manually select features	# 选择预设
   ? Check the features needed for your project: (Press <space> to select, <a> to toggle all, <i> to invert selection)
   >(*) Babel	# ES6 转 ES5 
    ( ) TypeScript	# 支持 TypeScript
    ( ) Progressive Web App (PWA) Support	# 支持 PWA 应用程序
    ( ) Router	# 路由
    ( ) Vuex	# Vue 程序开发的状态管理模式
    (*) CSS Pre-processors	# CSS 预处理器 
    (*) Linter / Formatter	# 规范化语法(魔鬼) 
    ( ) Unit Testing	# 单元测试
    ( ) E2E Testing	# E2E 测试
   ```

4. 之后就是对相应的预设功能做一些选择，`eslint` 选择 `ESLint + Standard config` 标准设置，config 选择 `In dedicated config files`，也可以按照你自己的喜好来，这里只是我的设置。之后会提示你是否保存，保存为 xxx 预设名。

### 图形化界面

> 通过 `vue ui` 的命令进入图形化页面创建和管理项目，具体步骤和上面差不多，而且是图形化很容易就不说了。