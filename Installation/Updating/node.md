---
order: -50
route: /installation/updating/node/
---

# 如何更新 Node.js

出于安全和性能原因，保持 Node.js 运行时的更新非常重要。以下是根据您的操作系统更新 Node.js 的步骤。

我们推荐使用最新的长期支持（LTS）版本，您可以在 [Node.js 官方网站](https://nodejs.org/en/about/previous-releases) 上找到。

## 如何检查当前的 Node.js 版本

1. 打开您的终端或命令提示符。
2. 输入以下命令并按回车：

```bash
node -v
```

## nvm（Node 版本管理器）- 跨平台

如果您正在使用 `nvm`：

1. 打开您的终端。
2. 输入以下命令：

[**Unix/Linux/macOS:**](https://github.com/nvm-sh/nvm)

```bash
nvm install --lts
nvm use --lts
```

[**Windows:**](https://github.com/coreybutler/nvm-windows)

```bash
nvm install lts
nvm use lts
```

## Windows - 常规安装

1. 前往 Node.js [下载页面](https://nodejs.org/en/download/)。
2. 下载 LTS 版本的 Windows 安装程序。
3. 运行安装程序并按照提示完成安装。

## Windows - SillyTavern 启动器

如果您使用 SillyTavern 启动器安装：

1. 打开 SillyTavern 启动器。
2. 导航到 `工具箱 / 应用安装器 / 核心工具 / 安装 Node.js`。

**或者：**

在 PowerShell 中使用 winget 手动操作：

```powershell
winget install --id=OpenJS.NodeJS.LTS  -e
```

## Android - Termux

1. 打开 Termux 应用。
2. 输入以下命令：

```bash
pkg update
pkg upgrade nodejs-lts
```

不要忘记在更新过程中通过在虚拟键盘上按 `Y` 来接受可能出现的任何提示。

## macOS - 常规安装

1. 前往 Node.js [下载页面](https://nodejs.org/en/download/)。
2. 下载 LTS 版本的 macOS 安装程序。
3. 运行 `.pkg` 文件并按照提示完成安装。

## macOS - Homebrew

如果您已安装 Homebrew，可以使用以下命令更新 Node.js：

```bash
brew update
brew upgrade node
```

## Linux - 包管理器

在 Linux 上更新 Node.js 的方法取决于您的发行版。

但由于官方软件源中的 Node.js 版本可能不是最新的，我们推荐使用 [Node 版本管理器（nvm）](https://github.com/nvm-sh/nvm) 或 [NodeSource 软件源](https://github.com/nodesource/distributions)。

## Docker

无需操作。我们提供的预构建 Docker 镜像已使用最新版本的 Node.js 编译。
