---
order: 50
icon: package
expanded: true
route: /installation/
---

# 安装

请根据您的平台选择安装指南：

* [Windows](/Installation/Windows.md)
* [Linux 和 Mac](/Installation/LinuxMacOS.md)
* [Android](/Installation/Android.md)
* [Docker](/Installation/Docker.md)

## 分支

SillyTavern 采用双分支系统进行开发，以确保所有用户都能获得流畅的体验。

* `release` -🌟 **推荐大多数用户使用。** 这是最稳定且推荐的分支，仅在推送主要版本时更新。适合大多数用户。通常每月更新一次。
* `staging` - ⚠️ **不建议日常使用。** 此分支具有最新功能，但请谨慎使用，因为它可能随时出现问题。仅适用于高级用户和爱好者。每天更新数次。

## Global / Standalone 模式

SillyTavern 有两种运行模式，它们在处理配置和数据路径方面有所不同。

* **Standalone 模式**（默认） - 使用服务器目录中的 `config.yaml` 文件和 `data` 目录。所有数据都将限制在安装路径中。这是大多数用户推荐的模式。
* **Global 模式** - 使用系统范围的配置和数据路径。这适用于将 SillyTavern 作为软件包安装，或者当您想在多个安装之间共享相同的配置和数据时。

!!!info
使用[官方 npm 包](https://www.npmjs.com/package/sillytavern)进行的安装（例如 `npx sillytavern@latest`）将默认以 global 模式运行。
!!!

### 数据路径

**Standalone 模式**路径相对于 SillyTavern 安装目录：

* **配置路径**: `./config.yaml`
* **数据根目录**: `./data/`

**Global 模式**路径取决于操作系统：

* **Linux**: `~/.local/share/SillyTavern/config.yaml`（或 `$XDG_DATA_HOME/SillyTavern/config.yaml`）和 `~/.local/share/SillyTavern/data/`（或 `$XDG_DATA_HOME/SillyTavern/data/`）
* **Windows**: `%APPDATA%\SillyTavern\config.yaml` 和 `%APPDATA%\SillyTavern\data\`
* **MacOS**: `~/Library/Application Support/SillyTavern/config.yaml` 和 `~/Library/Application Support/SillyTavern/data/`

### 如何以 global 模式运行

!!!warning
在 global 模式下运行时，不能使用 [CLI 参数](../Administration/config-yaml.md#command-line-arguments)或 [config.yaml](../Administration/config-yaml.md) 覆盖 `dataRoot` 和 `configPath`。
!!!

1. 向服务器启动命令传递 `--global` 参数（例如 `node server.js --global`）。
2. 向 shell 启动脚本传递 `--global` 参数（例如 `Start.bat --global` 或 `./start.sh --global`）。
3. 使用 `package.json` 文件中的 `start:global` 脚本（例如 `npm run start:global`）。
