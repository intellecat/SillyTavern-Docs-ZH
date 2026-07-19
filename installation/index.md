# 安装指南

请根据您的平台选择相应的安装指南：

* [Windows](/Installation/Windows.md)
* [Linux and Mac](/Installation/LinuxMacOS.md)
* [Android](/Installation/Android.md)
* [Docker](/Installation/Docker.md)

## 分支说明 {#branches}

SillyTavern 采用双分支开发系统，以确保所有用户获得流畅的使用体验。

* `release` - 🌟 **推荐大多数用户使用。** 这是最稳定且推荐的分支，仅在发布重要版本时更新。适合绝大多数用户使用。通常每月更新一次。
* `staging` - ⚠️ **不建议普通用户使用。** 此分支包含最新功能，但请注意它可能随时出现问题。仅适合专业用户和发烧友使用。每天更新多次。

## 全局/独立模式

SillyTavern 有两种运行模式，在处理配置和数据路径的方式上有所不同。

* **独立模式**（默认）——使用服务器目录中的 `config.yaml` 文件和 `data` 目录。所有数据将限制在安装路径内。这是大多数用户推荐的模式。
* **全局模式**——使用系统范围内的路径来存储配置和数据。当您以软件包形式安装 SillyTavern，或希望在多个安装之间共享相同的配置和数据时，此模式非常有用。

!!!info
使用[官方 npm 包](https://www.npmjs.com/package/sillytavern)安装的 SillyTavern（例如 `npx sillytavern@latest`）默认以全局模式运行。
!!!

### 数据路径 {#data-paths}

**独立模式**路径相对于 SillyTavern 安装目录：

* **配置路径**：`./config.yaml`
* **数据根目录**：`./data/`

**全局模式**路径取决于操作系统：

* **Linux**：`~/.local/share/SillyTavern/config.yaml`（或 `$XDG_DATA_HOME/SillyTavern/config.yaml`）和 `~/.local/share/SillyTavern/data/`（或 `$XDG_DATA_HOME/SillyTavern/data/`）
* **Windows**：`%APPDATA%\SillyTavern\config.yaml` 和 `%APPDATA%\SillyTavern\data\`
* **MacOS**：`~/Library/Application Support/SillyTavern/config.yaml` 和 `~/Library/Application Support/SillyTavern/data/`

### 如何在全局模式下运行

!!!warning
在全局模式下运行时，`dataRoot` 和 `configPath` 不能通过 [CLI 参数](../Administration/config-yaml.md#command-line-arguments) 或 [config.yaml](../Administration/config-yaml.md) 覆盖。
!!!

1. 向服务器启动命令传递 `--global` 参数（例如 `node server.js --global`）。
2. 向 shell 启动脚本传递 `--global` 参数（例如 `Start.bat --global` 或 `./start.sh --global`）。
3. 使用 `package.json` 中的 `start:global` 脚本（例如 `npm run start:global`）。
