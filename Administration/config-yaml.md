---
icon: cpu
route: /administration/config-yaml/
---

# 配置文件

!!!warning 免责声明

本文档可能已过时、不完整或存在错误。请参阅您安装目录中的 [默认 config.yaml](https://github.com/SillyTavern/SillyTavern/blob/release/default/config.yaml) 以获取最新的设置列表。

**警告：请勿直接编辑默认配置文件，这样做不会产生任何效果。请编辑仓库根目录中的副本。**
!!!

`config.yaml` 是 SillyTavern 服务器的主配置文件，您可以在[完成安装](/Installation/index.md)并首次运行服务器后，在仓库根目录中找到它。这是一个 YAML 文件，包含各种设置，例如网络、安全和后端专用选项。**对此文件所做的更改将在重启服务器后生效。**

上游新增的设置在[更新仓库](/Installation/Updating/index.md)后重启服务器时会自动填充默认值。您可以按需修改这些设置。若要在启动前添加缺失的设置，请运行 `npm run init` 脚本。

对于嵌套设置，使用点表示法来标识层级关系。例如，`protocol.ipv6: false` 表示 `protocol` 部分下的 `ipv6` 设置，值为 `false`。

```yaml
protocol:
  ipv6: false
```

## 命令行参数 {#command-line-arguments}

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
所有参数均为可选。如果不提供，SillyTavern 将使用 `config.yaml` 中的设置。
!!!

| 选项 | 描述 | 类型 |
|---------------------------------|----------------------------------------------------------------------|----------|
| `--version` | 显示版本号 | boolean |
| `--global` | 强制使用系统级路径存储应用数据 | boolean |
| `--configPath` | 覆盖 config.yaml 文件路径（仅独立模式） | string |
| `--dataRoot` | 设置数据存储根目录（仅独立模式） | string |
| `--port` | 设置 SillyTavern 运行的端口 | number |
| `--listen` | 使 SillyTavern 监听所有网络接口 | boolean |
| `--whitelist` | 启用白名单模式 | boolean |
| `--basicAuthMode` | 启用基本身份验证 | boolean |
| `--enableIPv4` | 启用 IPv4 协议 | boolean |
| `--enableIPv6` | 启用 IPv6 协议 | boolean |
| `--listenAddressIPv4` | 指定要监听的 IPv4 地址 | string |
| `--listenAddressIPv6` | 指定要监听的 IPv6 地址 | string |
| `--dnsPreferIPv6` | DNS 优先使用 IPv6 | boolean |
| `--ssl` | 启用 SSL | boolean |
| `--certPath` | 设置证书文件路径 | string |
| `--keyPath` | 设置私钥文件路径 | string |
| `--browserLaunchEnabled` | 自动在浏览器中启动 SillyTavern | boolean |
| `--browserLaunchHostname` | 设置浏览器启动的主机名 | string |
| `--browserLaunchPort` | 覆盖浏览器启动的端口 | string |
| `--browserLaunchAvoidLocalhost` | 自动模式下避免使用 'localhost' 进行浏览器启动 | boolean |
| `--corsProxy` | 启用 CORS 代理 | boolean |
| `--requestProxyEnabled` | 启用出站请求代理 | boolean |
| `--requestProxyUrl` | 设置请求代理 URL（HTTP 或 SOCKS 协议） | string |
| `--requestProxyBypass` | 设置请求代理绕过列表（以空格分隔的主机列表） | array |
| `--disableCsrf` | 禁用 CSRF 保护（不推荐） | boolean |

## 环境变量 {#environment-variables}

配置也可以通过环境变量设置，环境变量的值将覆盖 `config.yaml` 文件中的值。

环境变量应以 `SILLYTAVERN_` 为前缀，设置名称使用大写字母。例如，`dataRoot` 设置可以通过 `SILLYTAVERN_DATAROOT` 环境变量覆盖。

嵌套设置应使用下划线分隔。例如，`protocol.ipv6` 可以通过 `SILLYTAVERN_PROTOCOL_IPV6` 环境变量覆盖。

!!!warning
期望数组或对象的配置应使用 JSON 字符串格式。例如，要通过 `SILLYTAVERN_WHITELIST` 环境变量覆盖 `whitelist` 设置，应将其设置为 JSON 字符串：`SILLYTAVERN_WHITELIST='["127.0.0.1", "::1"]'`。
!!!

如果您使用 Node.js v20 或更高版本，也可以将环境变量存储在 `.env` 文件中，并通过 `--env-file` 标志传递给服务器。例如，要使用位于仓库根目录的 `.env` 文件，可以使用以下命令启动服务器：

```bash
node --env-file=.env server.js
```

或者，直接通过命令行传递环境变量：

```bash
SILLYTAVERN_LISTEN=true SILLYTAVERN_PORT=8000 node server.js
```

更多关于环境变量的使用方法，请参阅 [Node.js 文档](https://nodejs.org/en/learn/command-line/how-to-read-environment-variables-from-nodejs)。

## 数据配置 {#data-configuration}

| 设置 | 描述 | 默认值 | 允许的值 |
|---------|-------------|---------|-----------------|
| `dataRoot` | 用户数据存储的根目录（仅独立模式） | `./data` | 任意有效的目录路径 |
| `skipContentCheck` | 跳过新默认内容的检查 | `false` | `true`、`false` |
| `enableDownloadableTokenizers` | 启用按需分词器下载 | `true` | `true`、`false` |
| `whitelistImportDomains` | 用于导入网络托管角色卡和资源的可信域名列表 | [查看此处](https://github.com/SillyTavern/SillyTavern/blob/d118eee014330c37978cc0226f4ca74a1f321eea/default/config.yaml#L220) | 字符串数组 |

## 日志配置

| 设置 | 描述 | 默认值 | 允许的值 |
|---------|-------------|---------|------------------|
| `logging.minLogLevel` | 终端中显示的最低日志级别 | `0`（DEBUG） | （DEBUG = 0，INFO = 1，WARN = 2，ERROR = 3） |
| `logging.enableAccessLog` | 将服务器访问日志写入文件和控制台 | `true` | `true`、`false` |

## [网络配置](/Administration/remote-connections.md)

| 设置 | 描述 | 默认值 | 允许的值 |
|---------|-------------|---------|-----------------|
| `listen` | 启用监听传入连接 | `false` | `true`、`false` |
| `port` | 服务器监听端口 | `8000` | 任意有效端口号（1-65535） |
| `heartbeatInterval` | 为 Docker 健康检查写入心跳文件的间隔（秒）。设置为 0 可禁用 | `0` | `0`（禁用）、正整数 |
| `protocol.ipv4` | 启用 IPv4 协议监听 | `true` | `true`、`false`、`auto` |
| `protocol.ipv6` | 启用 IPv6 协议监听 | `false` | `true`、`false`、`auto` |
| `listenAddress.ipv4` | 监听特定的 IPv4 地址 | `0.0.0.0` | 有效的 IPv4 地址 |
| `listenAddress.ipv6` | 监听特定的 IPv6 地址 | `'[::]'` | 有效的 IPv6 地址 |
| `dnsPreferIPv6` | DNS 解析优先使用 IPv6 | `false` | `true`、`false` |
| `enableKeepAlive` | 全局启用 HTTP/HTTPS 长连接 | `false` | `true`、`false` |

## SSL 配置 {#ssl-configuration}

| 设置 | 描述 | 默认值 | 允许的值 |
|---------|-------------|---------|------------------|
| `ssl.enabled` | 启用 SSL/TLS 加密和 HTTPS 协议 | `false` | `true`、`false` |
| `ssl.keyPath` | SSL 私钥路径（相对于服务器目录） | `"./certs/privkey.pem"` | 有效的文件路径 |
| `ssl.certPath` | SSL 证书路径（相对于服务器目录） | `"./certs/cert.pem"` | 有效的文件路径 |
| `ssl.keyPassphrase` | SSL 私钥的密码短语。如不需要请留空 | `""` | 任意字符串 |

## 安全配置

### IP 白名单 {#ip-whitelisting}

| 设置 | 描述 | 默认值 | 允许的值 |
|---------|-------------|---------|-----------------|
| `whitelistMode` | 启用 IP 白名单过滤 | `true` | `true`、`false` |
| `enableForwardedWhitelist` | 检查转发头中的白名单 IP。头信息在[转发头配置](#转发头配置)部分定义 | `true` | `true`、`false` |
| `whitelist` | 允许的 IP 地址列表 | `["::1", "127.0.0.1"]` | 有效 IP 地址数组 |
| `whitelistDockerHosts` | 自动将 Docker 主机 IP 加入白名单 | `true` | `true`、`false` |

### 转发头配置

!!!
调整用于确定真实 IP 的头信息，以用于 IP 白名单、速率限制和访问日志等功能。

仅当您确定使用了正确配置的[反向代理](./reverse-proxying.md)时才进行修改，否则可能导致 IP 欺骗。
!!!

| 设置 | 描述 | 默认值 | 允许的值 |
|---------|-------------|---------|-----------------|
| `forwardedHeaders.xRealIp` | 使用 `X-Real-IP` 头检测客户端 IP | `true` | `true`、`false` |
| `forwardedHeaders.xForwardedFor` | 使用 `X-Forwarded-For` 头检测客户端 IP | `true` | `true`、`false` |
| `forwardedHeaders.cfConnectingIp` | 使用 `CF-Connecting-IP` 头检测客户端 IP | `false` | `true`、`false` |

### 私有地址白名单 {#private-address-whitelisting}

!!!
这是防止服务器端请求伪造（SSRF）攻击的额外安全层，对解析到私有 IP 地址的服务器端 HTTP 请求执行白名单检查。启用后，[私有网络范围](./remote-connections.md#what-is-considered-a-private-address)将被阻止，除非在 `privateAddressWhitelist.allowedRanges` 设置中明确允许。

**此功能默认禁用，但在启用监听模式或服务器可被不可信用户访问时，建议启用。**
!!!

| 设置 | 描述 | 默认值 | 允许的值 |
|---------|-------------|---------|-----------------|
| `privateAddressWhitelist.enabled` | 启用私有地址白名单 | `false` | `true`、`false` |
| `privateAddressWhitelist.allowUnresolvedHosts` | 允许对无法解析的主机发送请求 | `false` | `true`、`false` |
| `privateAddressWhitelist.allowedRanges` | 允许的私有 IP 地址和 CIDR 范围列表 | `['127.0.0.0/8', '::1/128']` | 有效 IP 地址和 CIDR 范围数组 |
| `privateAddressWhitelist.log.blockedRequests` | 记录解析到私有 IP 地址的被阻止请求 | `true` | `true`、`false` |
| `privateAddressWhitelist.log.allowedRequests` | 记录解析到私有 IP 地址的被允许请求 | `false` | `true`、`false` |

### 主机白名单 {#host-whitelisting}

| 设置 | 描述 | 默认值 | 允许的值 |
|---------|-------------|---------|-----------------|
| `hostWhitelist.enabled` | 启用主机白名单 | `false` | `true`、`false` |
| `hostWhitelist.scan` | 记录来自不可信主机的传入请求 | `true` | `true`、`false` |
| `hostWhitelist.hosts` | 可信主机名列表 | `[]` | 有效主机名数组 |

### 安全覆盖

!!!danger
**强烈不建议禁用安全措施。在进行任何更改之前，请确保您了解自己在做什么。**
!!!

| 设置 | 描述 | 默认值 | 允许的值 |
|---------|-------------|---------|------------------|
| `allowKeysExposure` | 允许在界面中显示未遮蔽的 API 密钥 | `false` | `true`、`false` |
| `disableCsrfProtection` | 禁用 CSRF 保护（不推荐） | `false` | `true`、`false` |
| `securityOverride` | 禁用启动时的安全检查（不推荐） | `false` | `true`、`false` |

## [用户身份验证](/Administration/multi-user.md) {#user-authentication}

!!!tip
默认情况下，速率限制同时适用于基本身份验证和用户账户登录尝试。您可以在[速率限制配置](#速率限制配置)部分调整限制。

如果通过隧道连接（例如 Cloudflare Tunnel）使用基本身份验证，请务必相应地调整[速率限制设置](#速率限制配置)和 [IP 地址检测](#转发头配置)，以防止被锁定。
!!!

| 设置 | 描述 | 默认值 | 允许的值 |
|---------|-------------|---------|-----------------|
| `basicAuthMode` | 启用基本身份验证 | `false` | `true`、`false` |
| `basicAuthUser.username` | 基本身份验证用户名 | `"user"` | 任意字符串 |
| `basicAuthUser.password` | 基本身份验证密码 | `"password"` | 任意字符串 |
| `enableUserAccounts` | 启用多用户模式 | `false` | `true`、`false` |
| `enableDiscreetLogin` | 在登录界面隐藏用户列表 | `false` | `true`、`false` |
| `sessionTimeout` | 用户会话超时时间（秒） | `-1`（禁用） | 任意数字（-1 表示禁用，0 表示关闭浏览器时过期，大于 0 表示超时时间） |
| `perUserBasicAuth` | 使用账户凭据进行基本身份验证 | `false` | `true`、`false` |

### SSO 自动登录 {#sso-auto-login}

!!!warning
不当保护 SSO 流程可能导致未授权访问。在启用 SSO 之前，请确保正确配置可信代理并测试您的设置。如果不确定安全影响，建议保持 SSO 自动登录禁用，并使用其他身份验证方法。
!!!

| 设置 | 描述 | 默认值 | 允许的值 |
|---------|-------------|---------|------------------|
| `sso.trustedProxies` | 用于 SSO 身份验证的可信代理 IP 列表 | `["::1", "127.0.0.1"]` | 有效 IP 地址、CIDR 范围或通配符模式数组 |
| `sso.autheliaAuth` | 启用基于 Authelia 的自动登录。参见：[SSO](/Administration/sso.md) | `false` | `true`、`false` |
| `sso.authentikAuth` | 启用基于 Authentik 的自动登录。参见：[SSO](/Administration/sso.md) | `false` | `true`、`false` |

## 速率限制配置

!!!tip
要禁用特定路由的速率限制，请将限制设置为 `0`。

例如，要禁用登录速率限制，请将 `rateLimiting.accountsLoginMaxAttempts` 设置为 `0`。
!!!

| 设置 | 描述 | 默认值 | 允许的值 |
|---------|-------------|---------|------------------|
| `rateLimiting.preferRealIpHeader` | 使用[转发头配置](#转发头配置)中配置的头信息中的 IP，而非套接字 IP 进行速率限制 | `false` | `true`、`false` |
| `rateLimiting.accountsLoginMaxAttempts` | 用户账户临时锁定前的最大登录尝试次数（锁定 1 分钟） | `5` | 任意正整数或 `0` |
| `rateLimiting.accountsRecoverMaxAttempts` | 临时锁定前的最大密码恢复尝试次数（锁定 5 分钟） | `5` | 任意正整数或 `0` |
| `rateLimiting.basicAuthMaxAttempts` | 临时锁定前的最大基本身份验证尝试次数（锁定 1 分钟） | `5` | 任意正整数或 `0` |

## 请求代理配置

!!!warning
请求代理与私有地址白名单功能存在冲突。如果同时启用两者，只有绕过代理的请求会被私有地址白名单检查，而经过代理的请求将不会被检查。如有疑虑，建议禁用请求代理。
!!!

| 设置 | 描述 | 默认值 | 允许的值 |
|---------|-------------|---------|-----------------|
| `requestProxy.enabled` | 启用出站请求代理 | `false` | `true`、`false` |
| `requestProxy.url` | 代理服务器 URL | `null` | 有效的代理 URL（例如 `"socks5://username:password@example.com:1080"`） |
| `requestProxy.bypass` | 绕过代理的主机 | `["localhost", "127.0.0.1"]` | 主机名/IP 数组 |

## CORS 代理配置

!!!warning
某些扩展可能需要启用 CORS 代理。内置功能不需要此功能。

在没有适当安全措施（例如 IP、主机和私有地址白名单）的情况下启用 CORS 代理可能导致 SSRF 漏洞。如果启用此功能，请确保相应地配置白名单，并且只在信任需要此功能的扩展时才启用它。
!!!

| 设置 | 描述 | 默认值 | 允许的值 |
|---------|-------------|---------|-----------------|
| `enableCorsProxy` | 启用 CORS 代理中间件 | `false` | `true`、`false` |

## CORS 配置

| 设置 | 描述 | 默认值 | 允许的值 |
|---------|-------------|---------|-----------------|
| `cors.enabled` | 启用或禁用 CORS 中间件 | `true` | `true`、`false` |
| `cors.origin` | 允许的来源。`"null"` 匹配浏览器默认文件来源 | `["null"]` | `"*"`（任意来源）、允许来源数组 |
| `cors.methods` | 允许的 HTTP 方法 | `["OPTIONS"]` | HTTP 方法数组 |
| `cors.allowedHeaders` | 允许的请求头 | `[]` | 头名称数组 |
| `cors.exposedHeaders` | 暴露的响应头 | `[]` | 头名称数组 |
| `cors.credentials` | 允许凭据（Cookie、授权头） | `false` | `true`、`false` |
| `cors.maxAge` | 预检缓存最大有效期（秒） | `null` | `null`、正整数 |

## 浏览器启动配置

> 以前称为"自动运行"设置。

| 设置 | 描述 | 默认值 | 允许的值 |
|---------|-------------|---------|-----------------|
| `browserLaunch.enabled` | 服务器启动时自动打开浏览器 | `true` | `true`、`false` |
| `browserLaunch.browser` | 用于打开 URL 的浏览器 | `"default"` | `"default"`、`"chrome"`、`"firefox"`、`"edge"`、`"brave"` |
| `browserLaunch.hostname` | 覆盖浏览器启动的主机名 | `"auto"` | `"auto"`、任意有效主机名（例如 `"localhost"`、`"st.example.com"`） |
| `browserLaunch.port` | 覆盖浏览器启动的端口 | `-1` | `-1`（使用服务器端口）、任意有效端口号 |
| `browserLaunch.avoidLocalhost` | 在启动 URL 中避免使用 'localhost' | `false` | `true`、`false` |

## 性能配置

| 设置 | 描述 | 默认值 | 允许的值 |
|---------|-------------|---------|------------------|
| `performance.lazyLoadCharacters` | 延迟加载角色数据 | `true` | `true`、`false` |
| `performance.useDiskCache` | 启用角色卡磁盘缓存 | `true` | `true`、`false` |
| `performance.memoryCacheCapacity` | 最大内存缓存容量 | `100mb` | 可读大小（例如 `100mb`、`1gb`） |
| `performance.requestCompression.enabled` | 为包含大量数据的客户端请求启用 gzip 压缩（例如设置或聊天保存） | `false` | `true`、`false` |
| `performance.requestCompression.minPayloadSize` | 触发压缩的最小载荷大小。设置为 0 可压缩所有请求 | `256kb` | 可读大小（例如 `256kb`、`1mb`） |
| `performance.requestCompression.maxPayloadSize` | 压缩的载荷大小硬性上限。设置为 0 可允许压缩任意大小 | `8mb` | 可读大小（例如 `8mb`、`16mb`） |
| `performance.requestCompression.timeout` | 请求压缩超时时间（毫秒） | `4000` | 正整数 |

## 缓存清除配置

!!!warning
需要 localhost 或带有 HTTPS 的域名，否则将无法工作！
!!!

| 设置 | 描述 | 默认值 | 允许的值 |
|---------|-------------|---------|------------------|
| `cacheBuster.enabled` | 首次加载或上传图片文件后清除浏览器缓存 | `false` | `true`、`false` |
| `cacheBuster.userAgentPattern` | 仅为匹配指定正则表达式的用户代理清除缓存。示例：`'firefox'`（不区分大小写）。 | `''` | 任意有效的正则表达式字符串 |

## 缩略图配置

| 设置 | 描述 | 默认值 | 允许的值 |
|---------|-------------|---------|-----------------|
| `thumbnails.enabled` | 启用缩略图生成 | `true` | `true`、`false` |
| `thumbnails.quality` | JPEG 缩略图质量 | `95` | 0-100 |
| `thumbnails.format` | 缩略图的图片格式 | `jpg` | `jpg`、`png` |
| `thumbnails.dimensions.bg` | 背景缩略图尺寸 | `[160, 90]` | 两个数字的数组（宽度、高度） |
| `thumbnails.dimensions.avatar` | 头像缩略图尺寸 | `[96, 144]` | 两个数字的数组（宽度、高度） |
| `thumbnails.dimensions.persona` | 人设缩略图尺寸 | `[96, 144]` | 两个数字的数组（宽度、高度） |

## 备份配置

| 设置 | 描述 | 默认值 | 允许的值 |
|---------|-------------|---------|-----------------|
| `backups.common.numberOfBackups` | 保留的备份数量 | `50` | 任意正整数 |
| `backups.chat.enabled` | 启用自动聊天备份 | `true` | `true`、`false` |
| `backups.chat.checkIntegrity` | 保存前验证聊天文件完整性 | `true` | `true`、`false` |
| `backups.chat.throttleInterval` | 备份节流间隔（毫秒） | `10000` | 任意正整数 |
| `backups.chat.maxTotalBackups` | 保留的最大聊天备份总数 | `-1` | 任意正整数或 -1 |
| `backups.allowFullDataBackup` | 允许用户创建完整数据备份存档 | `true` | `true`、`false` |

## [扩展配置](/extensions/index.md) {#extensions-configuration}

| 设置 | 描述 | 默认值 | 允许的值 |
|---------|-------------|---------|-----------------|
| `extensions.enabled` | 启用 UI 扩展 | `true` | `true`、`false` |
| `extensions.autoUpdate` | 自动更新扩展（如果扩展清单启用） | `true` | `true`、`false` |
| `extensions.models.autoDownload` | 启用自动模型下载 | `true` | `true`、`false` |
| `extensions.models.classification` | 用于分类的 HuggingFace 模型 ID | `"Cohee/distilbert-base-uncased-go-emotions-onnx"` | 有效的模型 ID |
| `extensions.models.captioning` | 用于图片说明的 HuggingFace 模型 ID | `"Xenova/vit-gpt2-image-captioning"` | 有效的模型 ID |
| `extensions.models.embedding` | 用于嵌入的 HuggingFace 模型 ID | `"Cohee/jina-embeddings-v2-base-en"` | 有效的模型 ID |
| `extensions.models.speechToText` | 用于语音转文字的 HuggingFace 模型 ID | `"Xenova/whisper-small"` | 有效的模型 ID |
| `extensions.models.textToSpeech` | 用于文字转语音的 HuggingFace 模型 ID | `"Xenova/speecht5_tts"` | 有效的模型 ID |

## [服务器插件](/For_Contributors/Server-Plugins.md)

| 设置 | 描述 | 默认值 | 允许的值 |
|---------|-------------|---------|-----------------|
| `enableServerPlugins` | 启用服务器端插件 | `false` | `true`、`false` |
| `enableServerPluginsAutoUpdate` | 启动时尝试自动更新服务器插件 | `true` | `true`、`false` |

## Git 配置

!!! Git 后端说明

1. `auto` - 优先使用系统二进制文件，回退到内置引擎
2. `system` - 使用 [simple-git](https://www.npmjs.com/package/simple-git) 的系统 git 二进制文件
3. `builtin` - 使用 [isomorphic-git](https://www.npmjs.com/package/isomorphic-git) 的内置引擎
!!!

| 设置 | 描述 | 默认值 | 允许的值 |
|---------|-------------|---------|-----------------|
| `git.backend` | 用于插件/扩展仓库操作的 Git 后端 | `auto` | `auto`、`system`、`builtin` |

## [API 集成设置](/Usage/API_Connections/index.md)

### OpenAI 配置

| 设置 | 描述 | 默认值 | 允许的值 |
|---------|-------------|---------|-----------------|
| `promptPlaceholder` | 空提示词的默认消息 | `"[Start a new chat]"` | 任意字符串 |
| `openai.randomizeUserId` | 随机化 API 调用的用户 ID | `false` | `true`、`false` |
| `openai.captionSystemPrompt` | 说明补全的系统消息 | `""` | 任意字符串 |

### MistralAI 配置

| 设置 | 描述 | 默认值 | 允许的值 |
|---------|-------------|---------|-----------------|
| `mistral.enablePrefix` | 启用回复预填充。**前缀将在响应中回显** | `false` | `true`、`false` |

### Ollama 配置

| 设置 | 描述 | 默认值 | 允许的值 |
|---------|-------------|---------|-----------------|
| `ollama.keepAlive` | 模型保活时长（秒） | `-1` | `-1`（无限期）、`0`（立即卸载）、正整数 |
| `ollama.batchSize` | 控制生成请求的 "num_batch"（批处理大小）参数 | `-1` | `-1`（模型默认值）、正整数 |

### Claude 配置

!!!warning **重要！**

请谨慎使用，仅在提示词前缀在请求之间保持不变时使用。`\{\{random\}\}` 宏、知识书、向量、摘要等可能会使缓存失效，导致您浪费缓存未命中的费用。提供商可能对缓存有最低提示词大小要求。行为可能不可预测，不提供任何保证，请查阅 API 文档。

参见：[提示词缓存](https://platform.claude.com/docs/en/build-with-claude/prompt-caching)
!!!

| 设置 | 描述 | 默认值 | 允许的值 |
|---------|-------------|---------|-----------------|
| `claude.enableSystemPromptCache` | 启用系统提示词缓存 | `false` | `true`、`false` |
| `claude.cachingAtDepth` | 启用消息历史缓存 | `-1` | `-1`（禁用）、`0` 或正整数 |
| `claude.extendedTTL` | 使用 1 小时 TTL 代替默认的 5 分钟。注意这也会增加请求费用。 | `false` | `true`、`false` |
| `claude.enableAdaptiveThinking` | 为支持的模型（Opus 4.6+）启用自适应思考。禁用可强制使用旧版思考模式（带有思考预算）。参见：[自适应思考](https://platform.claude.com/docs/en/build-with-claude/adaptive-thinking) | `false` | `true`、`false` |

### Google Gemini 配置

| 设置 | 描述 | 默认值 | 允许的值 |
|---------|-------------|---------|-----------------|
| `gemini.apiVersion` | API 端点版本（仅限 AI Studio） | `v1beta` | `v1beta`、`v1alpha` |
| `gemini.thoughtSignatures` | 为请求添加思考签名（如可用）。仅适用于 Gemini 3 及以上版本 | `true` | `true`、`false` |
| `gemini.enableSystemPromptCache` | 启用系统提示词缓存（仅限 OpenRouter） | `false` | `true`、`false` |
| `gemini.image.personGeneration` | 参见：<https://ai.google.dev/gemini-api/docs/imagen#imagen-configuration> | `allow_adult` | `dont_allow`、`allow_adult`、`allow_all` |

### DeepL 配置

| 设置 | 描述 | 默认值 | 允许的值 |
|---------|-------------|---------|-----------------|
| `deepl.formality` | 翻译正式程度 | `"default"` | `"default"`、`"more"`、`"less"`、`"prefer_more"`、`"prefer_less"` |
