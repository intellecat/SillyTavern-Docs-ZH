---
order: 25
icon: gear
expanded: true
route: /administration/
---

# 管理

!!!warning
尽管遵循了许多安全最佳实践，SillyTavern 服务器仍不足以安全地暴露在公开的互联网上。

**在未确保实施适当安全措施的情况下，切勿将任何实例托管在公开的互联网上。**

**由于安全措施不当或不足导致未经授权的访问而造成的任何损害或损失，我们概不负责。**
!!!

:::callout
**[config.yaml](./config-yaml.md)**

SillyTavern 的主配置文件，包含网络、安全及后端特定选项等各种设置。
:::

:::callout
**[多用户](multi-user)**

要与他人共享您的 SillyTavern 实例，可以创建多个用户账户。每位用户拥有各自独立的设置、扩展和数据。用户账户还可以设置密码保护。
:::

:::callout
**[远程访问](remote-connections)**

您可以从手机、平板电脑或其他计算机访问您的 SillyTavern 实例。
:::

:::callout
**[VPN 与隧道](tunneling.md)**

要从互联网访问您的 SillyTavern 实例，可以使用 VPN 或隧道服务，例如 Cloudflare Zero Trust、ngrok 或 Tailscale。
:::

:::callout
**[反向代理](reverse-proxying)**

进阶用户可以设置反向代理，以便从互联网访问其 SillyTavern 实例。
:::

## 数据目录结构

本节概述了 SillyTavern 中用户数据的存储结构。此处仅描述默认数据布局（数据根目录为 SillyTavern 安装目录的子目录）。如果您已自定义数据布局，请参阅您的自定义配置以获取详细信息。

### `data/[user-handle]`（例如 `data/default-user`）

为每个用户账户创建，此文件夹包含用户特定数据，例如角色文件、对话历史记录和设置。

### `data/_cache`

用于存储服务器下载的文件，例如分词器文件和 transformers.js 模型。

#### `data/_cache/characters`

当启用 `performance.useDiskCache` 设置时，包含已解析的角色数据，在启动时及角色更新时同步。

这以磁盘空间为代价，实现更快速的角色数据加载。

### `data/_css`

用于存储自定义 CSS 文件。

目前仅支持 `user.css` 文件，该文件允许您向前端添加自定义样式。

### `data/_errors`

存储包含各种 HTTP 状态码错误页面的 HTML 文件。当服务器遇到错误时，这些文件用于显示自定义错误页面。

- `forbidden-by-whitelist.html`：当请求被 IP 地址白名单阻止时显示。
- `host-not-allowed.html`：当请求被主机白名单阻止时显示。
- `unauthorized.html`：当基本身份验证失败时显示。
- `url-not-found.html`：当请求的资源未找到时显示。

### `data/_storage`

用于存储[用户账户数据](./multi-user.md)。

不建议手动编辑这些文件，否则可能导致数据损坏。

### `data/_uploads`

用于临时存储用户上传的文件，在服务器处理期间使用。

每次启动时这些文件会被自动清除。

### `data/_webpack`

用于存储已编译的 webpack 资产和缓存文件。

如果更新后前端出现问题，请尝试清除此文件夹，以强制完整重新构建前端资产。

### `data/access.log`

记录服务器收到的传入 HTTP 请求的日志文件，从首次成功连接后开始记录。

请定期检查此文件，以监控是否有任何可疑活动。

### `data/cookie-secret.txt`

包含服务器用于签名 Cookie 的密钥。

如果该文件不存在，首次启动时会自动生成。

## 安全检查清单

**以下内容仅为建议。在将您的 ST 实例上线之前，请咨询 Web 应用安全专家。**

1. 保持操作系统和运行时软件（例如 Node.js）为最新版本。这可确保您的系统拥有最新的安全补丁和修复，有助于防范潜在漏洞。
2. 使用[白名单](/Administration/config-yaml.md#ip-whitelisting)和网络防火墙，仅允许受信任的 IP 范围访问服务器。
3. 启用[基本身份验证](/Administration/config-yaml.md#user-authentication)，它在访问前端应用之前充当"主密码"。
4. 或者，配置外部身份验证。一些常见的服务包括 [Authelia](https://www.authelia.com/) 和 [authentik](https://goauthentik.io/)。详情请参阅 [SSO 指南](sso.md)。
5. 切勿让管理账户没有密码。如果存在未受保护的管理账户，服务器将在启动时发出警告。
6. 在本地网络以外的地方使用隐蔽登录设置，以对潜在的外部人员隐藏用户列表。
7. 经常检查访问日志。日志会写入服务器控制台和 `access.log` 文件，提供有关传入连接的信息，例如 IP 地址和用户代理。
8. 配置 HTTPS。对于本地服务器，可以生成并使用自签名证书。否则，您可能需要部署反向代理 Web 服务器，例如 [Traefik](https://traefik.io/) 或 [Caddy](https://caddyserver.com/docs/getting-started)。
9. 配置并启用[主机白名单](/Administration/config-yaml.md#host-whitelisting)，尤其是在本地网络上不使用 HTTPS 加密时。
10. 配置并启用[私有地址白名单](/Administration/config-yaml.md#private-address-whitelisting)以防止 SSRF 攻击。

有关安全代理的更多信息，请参阅以下指南：[SillyTavern 反向代理](reverse-proxying)。
