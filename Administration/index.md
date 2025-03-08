---
order: 25
icon: gear
expanded: true
---

# 管理

!!! warning

尽管遵循了许多安全最佳实践，但 SillyTavern 服务器的安全性还不足以直接暴露在公共互联网上。

**在确保采取适当的安全措施之前，切勿将任何实例托管到开放的互联网上。**

**对于因安全实施不当或不充分而导致未经授权访问造成的任何损害或损失，我们概不负责。**
!!!

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

对于爱好者来说，您可以设置反向代理以从互联网访问您的 SillyTavern 实例。
:::



## 安全检查清单

**这只是一个建议。在将您的 ST 实例上线之前，请咨询 Web 应用程序安全专家。**

1. 保持操作系统和 Node.js 等运行时软件的更新。这将确保您的系统具有最新的安全补丁和修复程序，有助于防止潜在的漏洞。
2. 使用白名单和网络防火墙。只允许受信任的 IP 范围访问服务器。
3. 启用基本身份验证。它在您进入前端应用程序之前充当"主密码"。
4. 或者，配置外部身份验证。一些已知的服务包括 [Authelia](https://www.authelia.com/) 和 [authentik](https://goauthentik.io/)。更多信息请参见 [SSO 指南](sso.md)。
5. 切勿让管理员账户无密码。如果您有任何未受保护的管理员账户，服务器会在启动时警告您。
6. 在本地网络之外使用谨慎登录设置。这将对任何潜在的外部人员隐藏用户列表。
7. 经常检查访问日志。它们会写入服务器控制台和 `access.log` 文件，并提供有关传入连接的信息，如 IP 地址和用户代理。
8. 配置 HTTPS。对于本地主机服务器，您可以生成并使用自签名证书。否则，您可能需要部署代理 Web 服务器，如 [Traefik](https://traefik.io/) 或 [Caddy](https://caddyserver.com/docs/getting-started)。

在以下指南中了解更多关于安全代理的信息：[SillyTavern 的反向代理](reverse-proxying)。
