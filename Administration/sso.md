---
label: 单点登录 (SSO)
icon: key
order: -20
route: /administration/sso/
---

# 单点登录 (SSO)
SSO 允许您创建用户并使用登录门户来保护您想要保护的多个网站。虽然设置比较复杂，但这是一个很好的方式来学习 SSO 并更好地保护您在互联网上的 ST 实例。

[**Authelia**](https://www.authelia.com/) 和 [**Authentik**](https://goauthentik.io/) 是可以与 SillyTavern 一起使用的开源 SSO 提供商。

## 使用 SSO 登录

如果您的 SSO 提供的用户名与 SillyTavern 用户账户的用户名完全匹配，您可以通过 SSO 以该用户身份登录 SillyTavern。

要实现这一点，请在 *config.yaml* 中启用 `autheliaAuth`。
    
```yaml
autheliaAuth: true
```

这增强或替代了[多用户模式](/Administration/multi-user.md)设置中内置的[密码管理](/Usage/User_Settings/User_Settings.md#账户管理)组件。

## 替换 HTTP BA

SSO 还可以替换 [HTTP 基本认证](/Administration/remote-connections.md#通过-http-基本认证进行访问控制)作为[远程连接](/Administration/remote-connections.md#访问控制配置)的访问控制机制。

这是推荐的，因为 SSO 提供比 HTTP BA 更好的安全性和功能。

要使用 SSO 提供商来替代 HTTP BA，请在 *config.yaml* 中启用 `securityOverride`。否则，SillyTavern 将[拒绝启动](remote-connections.md#访问控制配置)。

```yaml
autheliaAuth: true
basicAuthMode: false
securityOverride: true
```
