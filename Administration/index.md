---
order: 25
icon: gear
expanded: true
route: /administration/
---

# Administration

!!!warning
尽管遵循了许多安全最佳实践，SillyTavern 服务器的安全性仍不足以暴露在公共互联网上。

**在未首先确保适当的安全措施的情况下，切勿在开放的互联网上托管任何实例。**

**对于因不当或不充分的安全实施而导致未经授权访问造成的任何损害或损失，我们概不负责。**
!!!

:::callout
**[config.yaml](./config-yaml.md)**

SillyTavern 的主配置文件。它包含各种设置，例如网络、安全和后端特定选项。
:::

:::callout
**[多用户](multi-user)**

要与他人共享您的 SillyTavern 实例，您可以创建多个用户账户。每个用户都有自己的设置、扩展和数据。用户账户也可以设置密码保护。
:::

:::callout
**[远程访问](remote-connections)**

您可以从手机、平板电脑或其他计算机访问您的 SillyTavern 实例。
:::

:::callout
**[VPN 和隧道](tunneling.md)**

要从互联网访问您的 SillyTavern 实例，您可以使用 VPN 或隧道服务，如 Cloudflare Zero Trust、ngrok 或 Tailscale。
:::

:::callout
**[反向代理](reverse-proxying)**

高级用户可以设置反向代理以从互联网访问他们的 SillyTavern 实例。
:::

## 安全检查清单

**这些只是建议。在使您的 ST 实例上线之前，请咨询 Web 应用程序安全专家。**

1. 保持您的操作系统和运行时软件（如 Node.js）更新。这可确保您的系统拥有最新的安全补丁和修复程序，有助于防止潜在的漏洞。
2. 使用[白名单](/Administration/config-yaml.md#ip-whitelisting)和网络防火墙。仅允许受信任的 IP 范围访问服务器。
3. 启用[基本身份验证](/Administration/config-yaml.md#user-authentication)。它在您访问前端应用程序之前充当"主密码"。
4. 或者，配置外部身份验证。一些已知的服务包括 [Authelia](https://www.authelia.com/) 和 [authentik](https://goauthentik.io/)。详情请参阅 [SSO 指南](sso.md)。
5. 永远不要让管理员账户没有密码。如果您有任何未受保护的管理员账户，服务器将在启动时警告您。
6. 在本地网络外使用隐私登录设置。这会对潜在的外部人员隐藏用户列表。
7. 经常检查访问日志。它们被写入服务器控制台和 `access.log` 文件，并提供有关传入连接的信息，例如 IP 地址和用户代理。
8. 配置 HTTPS。对于 localhost 服务器，您可以生成并使用自签名证书。否则，您可能需要部署反向代理 Web 服务器，如 [Traefik](https://traefik.io/) 或 [Caddy](https://caddyserver.com/docs/getting-started)。
9. 配置并启用[主机白名单](/Administration/config-yaml.md#host-whitelisting)，特别是如果您在本地网络上未使用 HTTPS 加密。

在以下指南中查找有关安全代理的更多信息：[反向代理 SillyTavern](reverse-proxying)。
