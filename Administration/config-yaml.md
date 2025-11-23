---
icon: cpu
route: /administration/config-yaml/
---

# 配置文件

!!!warning 免责声明

此文档可能已过时、不完整或不正确。请参考您安装中的 [默认 config.yaml](https://github.com/SillyTavern/SillyTavern/blob/release/default/config.yaml) 以获取最新的设置列表。

**警告：不要直接编辑默认配置。这不会产生任何积极效果。请编辑存储库根目录中的副本。**
!!!

`config.yaml` 是 SillyTavern 服务器的主配置文件，您可以在[完成安装](/Installation/index.md)后在存储库根目录中找到它。它是一个 YAML 文件，包含各种设置，例如网络、安全和后端特定选项。**对此文件所做的更改将在重新启动服务器后生效。**

在[更新存储库](/Installation/Updating/index.md)后运行 `npm install`（具体来说是 `post-install.js` 脚本）时，上游添加的新设置会自动填充默认值。然后您可以根据需要修改这些设置。

对于嵌套设置，使用点表示法来指示层次结构。例如，`protocol.ipv6: false` 指的是 `protocol` 部分下的 `ipv6` 设置，值为 `false`。

```yaml
protocol:
  ipv6: false
```

## 命令行参数

启动 SillyTavern 服务器时，您可以传递命令行参数来覆盖 [config.yaml](../Administration/config-yaml.md) 中的某些设置。

### 示例

```shell
node server.js --port 8000 --listen false
# 或
npm run start -- --port 8000 --listen false
# 或（仅限 Windows）
Start.bat --port 8000 --listen false
```

### 支持的参数

!!!tip
所有参数都不是必需的。如果您不提供它们，SillyTavern 将使用 `config.yaml` 中的设置。
!!!

| 选项                          | 描述                                                          | 类型     |
|---------------------------------|----------------------------------------------------------------------|----------|
| `--version`                     | 显示版本号                                             | boolean  |
| `--global`                      | 强制使用系统范围的应用程序数据路径             | boolean  |
| `--configPath`                  | 覆盖 config.yaml 文件的路径（仅限独立模式）    | string   |
| `--dataRoot`                    | 设置数据存储的根目录（仅限独立模式）      | string   |
| `--port`                        | 设置 SillyTavern 运行的端口                       | number   |
| `--listen`                      | 使 SillyTavern 在所有网络接口上侦听                   | boolean  |
| `--whitelist`                   | 启用白名单模式                                               | boolean  |
| `--basicAuthMode`               | 启用基本身份验证                                         | boolean  |
| `--enableIPv4`                  | 启用 IPv4 协议                                            | boolean  |
| `--enableIPv6`                  | 启用 IPv6 协议                                            | boolean  |
| `--listenAddressIPv4`           | 指定要侦听的 IPv4 地址                              | string   |
| `--listenAddressIPv6`           | 指定要侦听的 IPv6 地址                              | string   |
| `--dnsPreferIPv6`               | DNS 首选 IPv6                                                 | boolean  |
| `--ssl`                         | 启用 SSL                                                          | boolean  |
| `--certPath`                    | 设置证书文件的路径                               | string   |
| `--keyPath`                     | 设置私钥文件的路径                               | string   |
| `--browserLaunchEnabled`        | 在浏览器中自动启动 SillyTavern                    | boolean  |
| `--browserLaunchHostname`       | 设置浏览器启动主机名                                     | string   |
| `--browserLaunchPort`           | 覆盖浏览器启动的端口                                | string   |
| `--browserLaunchAvoidLocalhost` | 在自动模式下避免使用 'localhost' 进行浏览器启动             | boolean  |
| `--corsProxy`                   | 启用 CORS 代理                                               | boolean  |
| `--requestProxyEnabled`         | 为传出请求启用代理                     | boolean  |
| `--requestProxyUrl`             | 设置请求代理 URL（HTTP 或 SOCKS 协议）                 | string   |
| `--requestProxyBypass`          | 设置请求代理绕过列表（空格分隔的主机列表）   | array    |
| `--disableCsrf`                 | 禁用 CSRF 保护（不推荐）                           | boolean  |

## 环境变量

配置也可以通过环境变量设置，这将覆盖 `config.yaml` 文件中的值。

环境变量应以 `SILLYTAVERN_` 为前缀，并对设置名称使用大写字母。例如，`dataRoot` 设置可以使用 `SILLYTAVERN_DATAROOT` 环境变量覆盖。

嵌套设置应使用下划线分隔。例如，`protocol.ipv6` 可以使用 `SILLYTAVERN_PROTOCOL_IPV6` 环境变量覆盖。

!!!warning
期望数组或对象的配置应进行 JSON 字符串化。例如，要使用 `SILLYTAVERN_WHITELIST` 环境变量覆盖 `whitelist` 设置，您应将其设置为 JSON 字符串：`SILLYTAVERN_WHITELIST='["127.0.0.1", "::1"]'`。
!!!

如果您使用的是 Node.js v20 或更高版本，您还可以在 `.env` 文件中存储环境变量，并使用 `--env-file` 标志将其传递给服务器。例如，要使用位于存储库根目录中的 `.env` 文件，您可以使用以下命令启动服务器：

```bash
node --env-file=.env server.js
```

或者，直接通过命令行传递环境变量：

```bash
SILLYTAVERN_LISTEN=true SILLYTAVERN_PORT=8000 node server.js
```

有关使用环境变量的更多信息，请参见 [Node.js 文档](https://nodejs.org/en/learn/command-line/how-to-read-environment-variables-from-nodejs)。

## 数据配置

| 设置 | 描述 | 默认值 | 允许的值 |
|---------|-------------|---------|-----------------|
| `dataRoot` | 用户数据存储的根目录（仅限独立模式） | `./data` | 任何有效的目录路径 |
| `skipContentCheck` | 跳过新默认内容的检查 | `false` | `true`, `false` |
| `enableDownloadableTokenizers` | 启用按需下载 tokenizer | `true` | `true`, `false` |

## 日志配置

| 设置 | 描述 | 默认值 | 允许的值 |
|---------|-------------|---------|------------------|
| `logging.minLogLevel` | 在终端中显示的最小日志级别 | `0` (DEBUG) | (DEBUG = 0, INFO = 1, WARN = 2, ERROR = 3) |
| `logging.enableAccessLog` | 将服务器访问日志写入文件和控制台 | `true` | `true`, `false` |

## [网络配置](/Administration/remote-connections.md)

| 设置 | 描述 | 默认值 | 允许的值 |
|---------|-------------|---------|-----------------|
| `listen` | 启用侦听传入连接 | `false` | `true`, `false` |
| `port` | 服务器侦听端口 | `8000` | 任何有效的端口号 (1-65535) |
| `protocol.ipv4` | 在 IPv4 协议上启用侦听 | `true` | `true`, `false`, `auto` |
| `protocol.ipv6` | 在 IPv6 协议上启用侦听 | `false` | `true`, `false`, `auto` |
| `listenAddress.ipv4` | 侦听特定的 IPv4 地址 | `0.0.0.0` | 有效的 IPv4 地址 |
| `listenAddress.ipv6` | 侦听特定的 IPv6 地址 | `'[::]'` | 有效的 IPv6 地址 |
| `dnsPreferIPv6` | DNS 解析首选 IPv6 | `false` | `true`, `false` |

## SSL 配置

| 设置 | 描述 | 默认值 | 允许的值 |
|---------|-------------|---------|------------------|
| `ssl.enabled` | 启用 SSL/TLS 加密和 HTTPS 协议 | `false` | `true`, `false` |
| `ssl.keyPath` | SSL 私钥的路径（相对于服务器目录） | `"./certs/privkey.pem"` | 有效的文件路径 |
| `ssl.certPath` | SSL 证书的路径（相对于服务器目录） | `"./certs/cert.pem"` | 有效的文件路径 |
| `ssl.keyPassphrase` | SSL 私钥的密码短语。如果不需要则留空 | `""` | 任何字符串 |

## 安全配置

### IP 白名单

| 设置 | 描述 | 默认值 | 允许的值 |
|---------|-------------|---------|-----------------|
| `whitelistMode` | 启用 IP 白名单过滤 | `true` | `true`, `false` |
| `enableForwardedWhitelist` | 检查转发的头以获取白名单 IP | `true` | `true`, `false` |
| `whitelist` | 允许的 IP 地址列表 | `["::1", "127.0.0.1"]` | 有效 IP 地址的数组 |
| `whitelistDockerHosts` | 自动将 Docker 主机 IP 加入白名单 | `true` | `true`, `false` |

### 主机白名单

| 设置 | 描述 | 默认值 | 允许的值 |
|---------|-------------|---------|-----------------|
| `hostWhitelist.enabled` | 启用主机白名单 | `false` | `true`, `false` |
| `hostWhitelist.scan` | 记录来自不受信任主机的传入请求 | `true` | `true`, `false` |
| `hostWhitelist.hosts` | 受信任主机名列表 | `[]` | 有效主机名的数组 |

### 安全覆盖

!!!danger
**强烈不建议禁用安全措施。在进行更改之前，请确保您了解自己在做什么。**
!!!

| 设置 | 描述 | 默认值 | 允许的值 |
|---------|-------------|---------|------------------|
| `allowKeysExposure` | 允许在 UI 中暴露未遮蔽的 API 密钥 | `false` | `true`, `false` |
| `disableCsrfProtection` | 禁用 CSRF 保护（不推荐） | `false` | `true`, `false` |
| `securityOverride` | 禁用启动安全检查（不推荐） | `false` | `true`, `false` |

## [用户身份验证](/Administration/multi-user.md)

| 设置 | 描述 | 默认值 | 允许的值 |
|---------|-------------|---------|-----------------|
| `basicAuthMode` | 启用基本身份验证 | `false` | `true`, `false` |
| `basicAuthUser.username` | 基本身份验证用户名 | `"user"` | 任何字符串 |
| `basicAuthUser.password` | 基本身份验证密码 | `"password"` | 任何字符串 |
| `enableUserAccounts` | 启用多用户模式 | `false` | `true`, `false` |
| `enableDiscreetLogin` | 在登录屏幕上隐藏用户列表 | `false` | `true`, `false` |
| `sessionTimeout` | 用户会话超时（秒） | `-1` (禁用) | 任何数字（-1 禁用，0 为浏览器关闭，>0 为超时） |
| `perUserBasicAuth` | 使用账户凭据进行基本身份验证 | `false` | `true`, `false` |

### SSO 自动登录

| 设置 | 描述 | 默认值 | 允许的值 |
|---------|-------------|---------|------------------|
| `sso.autheliaAuth` | 启用基于 Authelia 的自动登录。参见：[SSO](/Administration/sso.md) | `false` | `true`, `false` |
| `sso.authentikAuth` | 启用基于 Authentik 的自动登录。参见：[SSO](/Administration/sso.md) | `false` | `true`, `false` |

## 速率限制配置

| 设置 | 描述 | 默认值 | 允许的值 |
|---------|-------------|---------|------------------|
| `rateLimiting.preferRealIpHeader` | 使用 X-Real-IP 头而不是套接字 IP 进行速率限制 | `false` | `true`, `false` |

## 请求代理配置

| 设置 | 描述 | 默认值 | 允许的值 |
|---------|-------------|---------|-----------------|
| `requestProxy.enabled` | 为传出请求启用代理 | `false` | `true`, `false` |
| `requestProxy.url` | 代理服务器 URL | `null` | 有效的代理 URL（例如 `"socks5://username:password@example.com:1080"`） |
| `requestProxy.bypass` | 绕过代理的主机 | `["localhost", "127.0.0.1"]` | 主机名/IP 数组 |

## CORS 代理配置

!!!
某些扩展可能需要启用的 CORS 代理。任何内置功能都不需要它。
!!!

| 设置 | 描述 | 默认值 | 允许的值 |
|---------|-------------|---------|-----------------|
| `enableCorsProxy` | 启用 CORS 代理中间件 | `false` | `true`, `false` |

## 浏览器启动配置

> 以前称为 "Autorun" 设置。

| 设置 | 描述 | 默认值 | 允许的值 |
|---------|-------------|---------|-----------------|
| `browserLaunch.enabled` | 在服务器启动时自动打开浏览器 | `true` | `true`, `false` |
| `browserLaunch.browser` | 用于打开 URL 的浏览器 | `"default"` | `"default"`, `"chrome"`, `"firefox"`, `"edge"`, `"brave"` |
| `browserLaunch.hostname` | 覆盖浏览器启动的主机名 | `"auto"` | `"auto"`，任何有效的主机名（例如 `"localhost"`, `"st.example.com"`） |
| `browserLaunch.port` | 覆盖浏览器启动的端口 | `-1` | `-1`（使用服务器端口），任何有效的端口号 |
| `browserLaunch.avoidLocalhost` | 在启动 URL 中避免使用 'localhost' | `false` | `true`, `false` |

## 性能配置

| 设置 | 描述 | 默认值 | 允许的值 |
|---------|-------------|---------|------------------|
| `performance.lazyLoadCharacters` | 延迟加载角色数据 | `true` | `true`, `false` |
| `performance.useDiskCache` | 为角色卡启用磁盘缓存 | `true` | `true`, `false` |
| `performance.memoryCacheCapacity` | 最大内存缓存容量 | `100mb` | 人类可读的大小（例如 `100mb`, `1gb`） |

## Cache Buster 配置

!!!warning
需要 localhost 或带有 HTTPS 的域，否则将无法工作！
!!!

| 设置 | 描述 | 默认值 | 允许的值 |
|---------|-------------|---------|------------------|
| `cacheBuster.enabled` | 在首次加载或上传图像文件后清除浏览器缓存 | `false` | `true`, `false` |
| `cacheBuster.userAgentPattern` | 仅为与指定正则表达式模式匹配的用户代理清除缓存。例如：`'firefox'`（不区分大小写）。 | `''` | 任何有效的正则表达式字符串 |

## 缩略图配置

| 设置 | 描述 | 默认值 | 允许的值 |
|---------|-------------|---------|-----------------|
| `thumbnails.enabled` | 启用缩略图生成 | `true` | `true`, `false` |
| `thumbnails.quality` | JPEG 缩略图质量 | `95` | 0-100 |
| `thumbnails.format` | 缩略图的图像格式 | `jpg` | `jpg`, `png` |
| `thumbnails.dimensions.bg` | 背景缩略图大小 | `[160, 90]` | 两个数字的数组（宽度，高度） |
| `thumbnails.dimensions.avatar` | 头像缩略图大小 | `[96, 144]` |  两个数字的数组（宽度，高度） |
| `thumbnails.dimensions.persona` | 人格缩略图大小 | `[96, 144]` |  两个数字的数组（宽度，高度） |

## 备份配置

| 设置 | 描述 | 默认值 | 允许的值 |
|---------|-------------|---------|-----------------|
| `backups.chat.enabled` | 启用自动聊天备份 | `true` | `true`, `false` |
| `backups.chat.checkIntegrity` | 在保存前验证聊天文件的完整性 | `true` | `true`, `false` |
| `backups.common.numberOfBackups` | 要保留的备份数量 | `50` | 任何正整数 |
| `backups.chat.throttleInterval` | 备份限制间隔（毫秒） | `10000` | 任何正整数 |
| `backups.chat.maxTotalBackups` | 要保留的最大聊天备份总数 | `-1` | 任何正整数或 -1 |

## [扩展配置](/extensions/index.md)

| 设置 | 描述 | 默认值 | 允许的值 |
|---------|-------------|---------|-----------------|
| `extensions.enabled` | 启用 UI 扩展 | `true` | `true`, `false` |
| `extensions.autoUpdate` | 自动更新扩展（如果扩展清单启用） | `true` | `true`, `false` |
| `extensions.models.autoDownload` | 启用自动下载模型 | `true` | `true`, `false` |
| `extensions.models.classification` | 用于分类的 HuggingFace 模型 ID | `"Cohee/distilbert-base-uncased-go-emotions-onnx"` | 有效的模型 ID |
| `extensions.models.captioning` | 用于图像字幕的 HuggingFace 模型 ID | `"Xenova/vit-gpt2-image-captioning"` | 有效的模型 ID |
| `extensions.models.embedding` | 用于嵌入的 HuggingFace 模型 ID | `"Cohee/jina-embeddings-v2-base-en"` | 有效的模型 ID |
| `extensions.models.speechToText` | 用于语音转文本的 HuggingFace 模型 ID | `"Xenova/whisper-small"` | 有效的模型 ID |
| `extensions.models.textToSpeech` | 用于文本转语音的 HuggingFace 模型 ID | `"Xenova/speecht5_tts"` | 有效的模型 ID |

## [服务器插件](/For_Contributors/Server-Plugins.md)

| 设置 | 描述 | 默认值 | 允许的值 |
|---------|-------------|---------|-----------------|
| `enableServerPlugins` | 启用服务器端插件 | `false` | `true`, `false` |
| `enableServerPluginsAutoUpdate` | 在启动时尝试自动更新服务器插件 | `true` | `true`, `false` |

## [API 集成设置](/Usage/API_Connections/index.md)

### OpenAI 配置

| 设置 | 描述 | 默认值 | 允许的值 |
|---------|-------------|---------|-----------------|
| `promptPlaceholder` | 空提示的默认消息 | `"[Start a new chat]"` | 任何字符串 |
| `openai.randomizeUserId` | 为 API 调用随机化用户 ID | `false` | `true`, `false` |
| `openai.captionSystemPrompt` | 字幕完成的系统消息 | `""` | 任何字符串 |

### MistralAI 配置

| 设置 | 描述 | 默认值 | 允许的值 |
|---------|-------------|---------|-----------------|
| `mistral.enablePrefix` | 启用回复预填充。**前缀将在响应中回显** | `false` | `true`, `false` |

### Ollama 配置

| 设置 | 描述 | 默认值 | 允许的值 |
|---------|-------------|---------|-----------------|
| `ollama.keepAlive` | 模型保持活动持续时间（秒） | `-1` | `-1`（无限期），`0`（立即卸载），正整数 |
| `ollama.batchSize` | 控制生成请求的 "num_batch"（批量大小）参数 | `-1` | `-1`（模型默认值），正整数 |

### Claude 配置

!!!warning **重要！**

请谨慎使用，仅在提示前缀是静态的且在请求之间不会更改时使用。\{\{random\}\} 宏、lorebooks、向量、摘要等可能会使缓存无效，您只会在缓存未命中上浪费金钱。行为可能不可预测，不会也不能提供任何保证。

参见：[Prompt Caching](https://docs.anthropic.com/en/docs/build-with-claude/prompt-caching)
!!!

| 设置 | 描述 | 默认值 | 允许的值 |
|---------|-------------|---------|-----------------|
| `claude.enableSystemPromptCache` | 启用系统提示缓存 | `false` | `true`, `false` |
| `claude.cachingAtDepth` | 启用消息历史缓存 | `-1` | `-1`（禁用），`0` 或正整数 |
| `claude.extendedTTL` | 使用 1h TTL 而不是默认的 5m。请注意，这也会增加请求的成本。 | `false` | `true`, `false` |

### Google AI Studio 配置

| 设置 | 描述 | 默认值 | 允许的值 |
|---------|-------------|---------|-----------------|
| `gemini.apiVersion` | API 端点版本 | `v1beta` | `v1beta`, `v1alpha` |

### DeepL 配置

| 设置 | 描述 | 默认值 | 允许的值 |
|---------|-------------|---------|-----------------|
| `deepl.formality` | 翻译正式程度 | `"default"` | `"default"`, `"more"`, `"less"`, `"prefer_more"`, `"prefer_less"` |
