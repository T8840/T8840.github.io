---
title: Nodejs环境配置
cover: 'https://images.unsplash.com/photo-1691841921811-c03b83c92938?crop=entropy&cs=tinysrgb&fit=max&fm=jpg&ixid=MnwxfDB8MXxyYW5kb218MHx8fHx8fHx8MTY5NDY3NTUxNg&ixlib=rb-4.0.3&q=80&w=1080'
date: 2023-09-14 14:59:05
updated: 2023-09-14 14:59:05
tags: JS
categories: JS
---

## 安装NVM
要安装 nvm（Node Version Manager），可以按照以下步骤进行：

#### Linux安装nvm
打开终端。
在终端中运行以下命令来安装 nvm 的安装脚本：

```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
```
这将下载并运行 nvm 的安装脚本。

安装完成后，终端会显示一些说明信息。按照说明，在终端中运行以下命令来加载 nvm：
```
source ~/.nvm/nvm.sh
```
或者，你可以重新打开一个新的终端窗口，nvm 将会自动加载。

现在，你可以使用 nvm 来安装和管理 Node.js 版本。例如，要安装最新版本的 Node.js，可以运行以下命令：
```
nvm install node
```
这将安装最新的稳定版本的 Node.js。

验证安装是否成功，可以运行以下命令检查 Node.js 和 npm（Node 包管理器）的版本：
```
node --version
npm --version
```
这将显示已安装的 Node.js 和 npm 的版本号。

以上是在 Linux 或 macOS 系统上安装 nvm 的步骤。如果你使用的是 Windows 系统，可以尝试使用 nvm 的 Windows 版本（nvm-windows）。你可以在 nvm 的 GitHub 存储库上找到更多关于 nvm 的详细信息和使用说明。

### 安装yarn

```
npm install -g yarn
```

