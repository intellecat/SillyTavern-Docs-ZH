---
icon: cpu
---

# 配置文件

!!! warning 免责声明

本文档可能已过时、不完整或不准确。请参考您安装中的 [默认 config.yaml](https://github.com/SillyTavern/SillyTavern/blob/release/default/config.yaml) 以获取最新的设置列表。

!!!

`config.yaml` 是 SillyTavern 服务器的主要配置文件，在[完成安装](/Installation/index.md)后可以在仓库根目录中找到。它是一个 YAML 文件，包含各种设置，如网络设置、安全设置和后端特定选项。**对此文件的更改将在重启服务器后生效。**

在[更新仓库](/Installation/Updating/index.md)后运行 `npm install`（或具体来说，`post-install.js` 脚本）时，上游版本中添加的新设置将自动填充默认值。然后您可以根据需要修改这些设置。

对于嵌套设置，使用点表示法来表示层次结构。例如，`protocol.ipv6: false` 指的是 `protocol` 部分下的 `ipv6` 设置，其值为 `false`。

```yaml
protocol:
  ipv6: false
```

## 数据配置

| 设置 | 描述 | 默认值 | 允许的值 |
|---------|-------------|---------|-----------------|
| `dataRoot` | 用户数据存储的根目录 | `./data` | 任何有效的目录路径 |
| `cardsCacheCapacity` | 已解析角色卡可以使用的最大内存量（MB） | 100 | 任何正整数 |
| `skipContentCheck` | 跳过新的默认内容检查 | `false` | `true`, `false` |

## [网络配置](/Administration/remote-connections.md)

| 设置 | 描述 | 默认值 | 允许的值 |
|---------|-------------|---------|-----------------|
| `listen` | 启用监听传入连接 | `false` | `true`, `false` |
| `port` | 服务器监听端口 | `8000` | 任何有效的端口号（1-65535） |
| `protocol.ipv4` | 启用在 IPv4 协议上监听 | `true` | `true`, `false` |
| `protocol.ipv6` | 启用在 IPv6 协议上监听 | `false` | `true`, `false` |
| `dnsPreferIPv6` | DNS 解析优先使用 IPv6 | `false` | `true`, `false` |

## 安全配置

| 设置 | 描述 | 默认值 | 允许的值 |
|---------|-------------|---------|-----------------|
| `whitelistMode` | 启用 IP 白名单过滤 | `true` | `true`, `false` |
| `enableForwardedWhitelist` | 检查转发头中的白名单 IP | `true` | `true`, `false` |
| `whitelist` | 允许的 IP 地址列表 | `["::1", "127.0.0.1"]` | 有效 IP 地址数组 |
| `enableCorsProxy` | 启用 CORS 代理中间件 | `false` | `true`, `false` |
| `allowKeysExposure` | 允许在 UI 中暴露 API 密钥 | `false` | `true`, `false` |
| `disableCsrfProtection` | 禁用 CSRF 保护（不推荐） | `false` | `true`, `false` |
| `securityOverride` | 禁用启动安全检查（不推荐） | `false` | `true`, `false` |

## [用户认证](/Administration/multi-user.md)

| 设置 | 描述 | 默认值 | 允许的值 |
|---------|-------------|---------|-----------------|
| `basicAuthMode` | 启用基本认证 | `false` | `true`, `false` |
| `basicAuthUser.username` | 基本认证用户名 | `"user"` | 任何字符串 |
| `basicAuthUser.password` | 基本认证密码 | `"password"` | 任何字符串 |
| `enableUserAccounts` | 启用多用户模式 | `false` | `true`, `false` |
| `enableDiscreetLogin` | 在登录屏幕隐藏用户列表 | `false` | `true`, `false` |
| `sessionTimeout` | 用户会话超时时间（秒） | `86400`（24小时） | 任何数字（-1 禁用，0 浏览器关闭时，>0 超时） |
| `cookieSecret` | 用于签名会话 cookie 的密钥 | `''`（自动生成） | 任何字符串 |
| `autheliaAuth` | 启用基于 Authelia 的自动登录。参见：[SSO](/Administration/sso.md) | `false` | `true`, `false` |
| `perUserBasicAuth` | 使用账户凭据进行基本认证 | `false` | `true`, `false` |

## 请求代理配置

| 设置 | 描述 | 默认值 | 允许的值 |
|---------|-------------|---------|-----------------|
| `requestProxy.enabled` | 启用出站请求代理 | `false` | `true`, `false` |
| `requestProxy.url` | 代理服务器 URL | `null` | 有效的代理 URL（如 `"socks5://username:password@example.com:1080"`） |
| `requestProxy.bypass` | 绕过代理的主机 | `["localhost", "127.0.0.1"]` | 主机名/IP 数组 |

## 自动运行配置

| 设置 | 描述 | 默认值 | 允许的值 |
|---------|-------------|---------|-----------------|
| `autorun` | 启动时自动打开浏览器 | `true` | `true`, `false` |
| `autorunHostname` | 自动运行打开浏览器时使用的主机名 | `"auto"` | `"auto"`，任何有效的主机名（如 `"localhost"`，`"st.example.com"`） |
| `autorunPortOverride` | 覆盖浏览器自动运行的端口 | `-1` | `-1`（使用服务器端口），任何有效的端口号 |
| `avoidLocalhost` | 避免自动运行使用 'localhost' | `false` | `true`, `false` |

## 缩略图配置

| 设置 | 描述 | 默认值 | 允许的值 |
|---------|-------------|---------|-----------------|
| `thumbnails.enabled` | 启用缩略图生成 | `true` | `true`, `false` |
| `thumbnails.quality` | JPEG 缩略图质量 | `95` | 0-100 |
| `thumbnails.format` | 缩略图的图像格式 | `jpg` | `jpg`, `png` |
| `thumbnails.dimensions.bg` | 背景缩略图尺寸 | `[160, 90]` | 两个数字的数组（宽度，高度） |
| `thumbnails.dimensions.avatar` | 头像缩略图尺寸 | `[96, 144]` | 两个数字的数组（宽度，高度） |

## 备份配置

| 设置 | 描述 | 默认值 | 允许的值 |
|---------|-------------|---------|-----------------|
| `backups.chat.enabled` | 启用自动聊天备份 | `true` | `true`, `false` |
| `backups.common.numberOfBackups` | 要保留的备份数量 | `50` | 任何正整数 |
| `backups.chat.throttleInterval` | 备份节流间隔（毫秒） | `10000` | 任何正整数 |
| `backups.chat.maxTotalBackups` | 要保留的最大总聊天备份数 | `-1` | 任何正整数或 -1 |

## [扩展配置](/extensions/index.md)

| 设置 | 描述 | 默认值 | 允许的值 |
|---------|-------------|---------|-----------------|
| `enableExtensions` | 启用 UI 扩展 | `true` | `true`, `false` |
| `enableExtensionsAutoUpdate` | 自动更新扩展 | `true` | `true`, `false` |
| `enableDownloadableTokenizers` | 启用按需分词器下载 | `true` | `true`, `false` |
| `extras.disableAutoDownload` | 禁用自动模型下载 | `false` | `true`, `false` |
| `extras.classificationModel` | 用于分类的 HuggingFace 模型 ID | `"Cohee/distilbert-base-uncased-go-emotions-onnx"` | 有效的模型 ID |
| `extras.captioningModel` | 用于图像说明的 HuggingFace 模型 ID | `"Xenova/vit-gpt2-image-captioning"` | 有效的模型 ID |
| `extras.embeddingModel` | 用于嵌入的 HuggingFace 模型 ID | `"Cohee/jina-embeddings-v2-base-en"` | 有效的模型 ID |
| `extras.speechToTextModel` | 用于语音转文本的 HuggingFace 模型 ID | `"Xenova/whisper-small"` | 有效的模型 ID |
| `extras.textToSpeechModel` | 用于文本转语音的 HuggingFace 模型 ID | `"Xenova/speecht5_tts"` | 有效的模型 ID |

## [服务器插件](/For_Contributors/Server-Plugins.md)

| 设置 | 描述 | 默认值 | 允许的值 |
|---------|-------------|---------|-----------------|
| `enableServerPlugins` | 启用服务器端插件 | `false` | `true`, `false` |

## [API 集成设置](/Usage/API_Connections/index.md)

### OpenAI 配置

| 设置 | 描述 | 默认值 | 允许的值 |
|---------|-------------|---------|-----------------|
| `promptPlaceholder` | 空提示的默认消息 | `"[Start a new chat]"` | 任何字符串 |
| `openai.randomizeUserId` | API 调用随机化用户 ID | `false` | `true`, `false` |
| `openai.captionSystemPrompt` | 说明完成的系统消息 | `""` | 任何字符串 |

### MistralAI 配置

| 设置 | 描述 | 默认值 | 允许的值 |
|---------|-------------|---------|-----------------|
| `mistral.enablePrefix` | 启用回复预填充。**前缀将在响应中回显** | `false` | `true`, `false` |

### Ollama 配置

| 设置 | 描述 | 默认值 | 允许的值 |
|---------|-------------|---------|-----------------|
| `ollama.keepAlive` | 模型保持活动时间（秒） | `-1` | `-1`（无限期），`0`（立即卸载），正整数 |

### Claude 配置

!!! warning
**重要！**

请谨慎使用，仅在提示前缀是静态的且在请求之间不变时使用。\{\{random\}\} 宏、知识库、向量、摘要等可能会使缓存失效，您只会在缓存未命中时浪费金钱。行为可能不可预测，且不能也不会做出任何保证。

参见：[提示缓存](https://docs.anthropic.com/en/docs/build-with-claude/prompt-caching)
!!!

| 设置 | 描述 | 默认值 | 允许的值 |
|---------|-------------|---------|-----------------|
| `claude.enableSystemPromptCache` | 启用系统提示缓存 | `false` | `true`, `false` |
| `claude.cachingAtDepth` | 启用消息历史缓存 | `-1` | `-1`（禁用），`0` 或正整数 |

### DeepL 配置

| 设置 | 描述 | 默认值 | 允许的值 |
|---------|-------------|---------|-----------------|
| `deepl.formality` | 翻译正式程度 | `"default"` | `"default"`, `"more"`, `"less"`, `"prefer_more"`, `"prefer_less"` |
