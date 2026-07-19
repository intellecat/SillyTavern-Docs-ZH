# 单点登录（SSO）

!!!warning
尽管 SSO 有诸多优点，但它是一个配置和维护都较为复杂的系统。若配置不当，可能导致他人未经授权访问你的实例。请在启用 SSO 前务必正确配置并充分测试。如果你对其安全影响没有把握，建议保持 SSO 自动登录为禁用状态，转而使用其他身份验证方式。
!!!

SSO 允许你创建用户并通过登录门户保护多个不同页面。虽然配置较为复杂，但这是学习 SSO 并在互联网上更安全地保护 ST 实例的好方法。

SSO 还可以替代 [HTTP 基本身份验证](/Administration/config-yaml.md#user-authentication)，作为[远程连接](/Administration/remote-connections.md)的访问控制机制。

这是推荐的做法，因为 SSO 比 HTTP 基本身份验证提供更好的安全性和功能性。

[**Authelia**](https://www.authelia.com/) 和 [**Authentik**](https://goauthentik.io/) 是可与 SillyTavern 配合使用的开源 SSO 提供商。

## 配置可信代理

只有来自已配置为可信代理的 IP 地址的请求，才能通过转发必要的请求头来进行用户身份验证。默认情况下，IPv4 和 IPv6 的回环地址均受信任。若要允许其他 IP 通过 SSO 请求头进行身份验证，请将其添加到 [config.yaml](/Administration/config-yaml.md#sso-auto-login) 文件中的 `sso.trustedProxies` 列表：

!!!tip
同样支持 CIDR 和通配符表示法来指定多个 IP 或范围，例如 `192.168.0.*` 或 `10.0.0.0/24`。
!!!

```yaml
sso:
  trustedProxies:
    - ::1           # IPv6 回环地址 - 默认受信任
    - 127.0.0.1     # IPv4 回环地址 - 默认受信任
    - '192.168.0.1' # 可信代理 IP 地址示例
```

## 使用 SSO 登录

如果你的 SSO 提供的用户名与某个 SillyTavern 用户账户的用户标识符**完全匹配**，则可以通过 SSO 以该用户身份登录 SillyTavern。要启用此功能，请在 [config.yaml](/Administration/config-yaml.md#sso-auto-login) 文件中修改以下选项之一：

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

这两个选项均可增强或替代[多用户模式](/Administration/multi-user.md)下内置的[密码管理](/Usage/User_Settings/index.md#account-management)功能。
