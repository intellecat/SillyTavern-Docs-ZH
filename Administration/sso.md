---
label: Single Sign-On (SSO)
icon: key
order: -20
route: /administration/sso/
---

# Single Sign-On (SSO)

SSO 允许您创建用户并使用登录门户保护许多不同的页面，该门户显示在您想要保护的站点上。虽然设置起来很复杂，但这是学习 SSO 并更安全地在互联网上保护您的 ST 实例的好方法。

SSO 也可以替代 [HTTP Basic Authentication](/Administration/config-yaml.md#user-authentication) 作为[远程连接](/Administration/remote-connections.md)的访问控制机制。

推荐使用此方法，因为 SSO 比 HTTP Basic Authentication 提供更好的安全性和功能。

[**Authelia**](https://www.authelia.com/) 和 [**Authentik**](https://goauthentik.io/) 是可与 SillyTavern 一起使用的开源 SSO 提供商。

## 使用 SSO 登录

如果您的 SSO 提供的用户名与 SillyTavern 用户账户的用户句柄**完全匹配**，您可以通过 SSO 以该用户身份登录 SillyTavern。要启用此功能，请将以下选项之一更改为您的 [config.yaml](/Administration/config-yaml.md#sso-auto-login) 文件：

### Authelia

```yaml
sso:
  autheliaAuth: true
```

### Authentik

```yaml
sso:
  authentikAuth: true
```

这两个选项都增强或替代[多用户模式](/Administration/multi-user.md)设置的内置[密码管理](/Usage/User_Settings/index.md#account-management)组件。
