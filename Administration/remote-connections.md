---
icon: rss
order: -30
route: /usage/remoteconnections/
---

# 远程连接

大多数情况下，此功能适用于希望在手机上使用 SillyTavern、同时让 PC 在同一 WiFi 网络内运行 ST 服务器的用户。

这也是允许从本地网络外部进行远程连接的第一步。

!!!warning
不应使用端口转发将 ST 服务器暴露在互联网上。请改用 VPN 或隧道服务，例如 Cloudflare Zero Trust、ngrok 或 Tailscale。详情请参阅 [VPN 与隧道](tunneling.md) 指南。
!!!

!!!danger 免责声明
**在未确保实施适当安全措施的情况下，切勿将任何实例托管在公开的互联网上。**

**由于安全措施不当或不足导致未经授权的访问而造成的任何损害或损失，我们概不负责。**
!!!

## 允许远程连接 {#allowing-remote-connections}

默认情况下，ST 服务器仅接受来自其运行所在机器（localhost）的连接。要使其监听来自其他设备的连接，请将 `config.yaml` 中的 `listen` 选项设置为 `true`。

!!! 如果在 SillyTavern 文件夹中直接搜索 `config.yaml`，可能会找到两个文件。
本文档中对 `config.yaml` 的所有修改均指位于 SillyTavern 根目录下的文件（`/SillyTavern/config.yaml`），而非 `/SillyTavern/default/config.yaml`。
!!!

```yaml
# Listen for incoming connections
listen: true
```

当 ST 监听远程连接时，控制台中应显示以下消息：

```txt
SillyTavern is listening on IPv4: 0.0.0.0:8000
```

以及对此含义的相关说明。

当 ST **未**监听远程连接时，控制台中应显示以下消息：

```txt
SillyTavern is listening on IPv4: 127.0.0.1:8000
```

## 访问控制配置

启用远程连接监听后，必须配置至少一种访问控制方式，否则服务器将无法启动。

### 基于白名单的访问控制 {#whitelist-based-access-control}

要通过白名单启用访问控制，请编辑 SillyTavern 根目录下的 `config.yaml` 文件（`/SillyTavern/config.yaml`）：

1. 至少启动一次 SillyTavern，以生成必要的配置文件。
2. 用文本编辑器打开 `/SillyTavern/config.yaml`。
3. 找到 `whitelist` 部分，并添加希望允许访问的 IP 地址：
    * 分别列出每个 IP 地址。
    * 确保包含 `127.0.0.1`，否则将无法从宿主机连接。
    * 支持单个 IP、CIDR 掩码（例如 `10.0.0.0/24`）以及通配符（`*`）范围。
4. 保存 `config.yaml` 文件。
5. **重启 SillyTavern 服务器。**

#### `config.yaml` 白名单配置示例

1. 允许本地网络上的任何设备：

    ```yaml
    whitelist:
      - ::1
      - 127.0.0.1
      - 10.0.0.0/8
      - 172.16.0.0/12
      - 192.168.0.0/16
    ```

    如果不确定本地网络的地址范围，请使用上述白名单。

2. 允许两台特定设备连接：

    ```yaml
    whitelist:
      - ::1
      - 127.0.0.1
      - 192.168.0.2
      - 192.168.0.5
    ```

3. 允许 `192.168.0.*` 子网上的任何设备连接：

    ```yaml
    whitelist:
      - ::1
      - 127.0.0.1
      - 192.168.0.*
    ```

4. 允许所有 IPv4 设备的网络连接：

    ```yaml
    whitelist:
      - 0.0.0.0/0
    ```

### 禁用基于白名单的访问控制

要禁用白名单访问控制：

* 在 `/SillyTavern/config.yaml` 中将 `whitelistMode` 设置为 `false`。
* 删除或重命名 SillyTavern 基础安装文件夹中的 `whitelist.txt`（如果存在）。
* 重启 SillyTavern 服务器。

### 不推荐：使用 `whitelist.txt`

!!!info
如果 `whitelist.txt` 存在，它将优先于 `config.yaml` 中的白名单设置。

但是，由于所有其他配置均在 `config.yaml` 中管理，且 `whitelist.txt` 可能遇到权限问题或被锁定，系统可能会静默地回退到使用 `config.yaml` 中的白名单。

**直接编辑 config.yaml 既更简单，也更可靠。**
!!!

如果仍然倾向于使用 whitelist.txt：

1. 在 SillyTavern 基础安装文件夹中创建一个名为 `whitelist.txt` 的新文本文件。
2. 用文本编辑器打开它，并添加允许访问的 IP 地址。
3. 保存文件并重启 SillyTavern 服务器。

#### `whitelist.txt` 配置示例

```txt
10.0.0.0/8
172.16.0.0/12
192.168.0.0/16
127.0.0.1
::1
```

这将允许本地网络上的任何设备连接。

### 通过 HTTP 基本身份验证进行访问控制

!!!warning
HTTP 基本身份验证不能提供强安全性。

没有速率限制来防止暴力破解攻击。如果这是一个顾虑，建议使用带有 TLS 和速率限制的反向代理，以及专用的[身份验证服务](sso.md)。
!!!

每当客户端通过 HTTP 连接时，服务器将要求提供用户名和密码。**此功能仅在启用远程连接（`listen: true`）时有效。**

要启用 HTTP 基本身份验证，请打开 SillyTavern 基础目录中的 `config.yaml`，搜索 `basicAuthMode`，将其设置为 `true`，并设置用户名和密码。注意：`config.yaml` 仅在 ST 至少运行过一次后才会存在。

```yaml
basicAuthMode: true
basicAuthUser:
  username: "MyUsername"
  password: "MyPassword"
```

或者，也可以按如下方式启用基本身份验证：

```yaml
basicAuthMode: true
enableUserAccounts: true
perUserBasicAuth: true
```

在 `perUserBasicAuth` 模式下，基本身份验证的用户名和密码将与任何具有密码的有效多用户账户相同，且 SillyTavern 将直接登录至该账户。**请确保在启用 `perUserBasicAuth` 之前已有一个设置了密码的账户。**

保存文件，如果 SillyTavern 已在运行则重启它。连接到 ST 时，系统应提示输入用户名和密码。用户名和密码均以明文传输。如果对此有顾虑，可以通过 HTTPS 提供 ST 服务。

### 私有地址白名单

虽然默认情况下允许服务器向私有 IP 范围内的地址（例如 `192.168.x.x`、`10.x.x.x`）发出出站 HTTP 请求，但可以使用白名单配置来限制对特定私有地址的访问。当您在本地网络上运行私有 API，希望允许 ST 访问它，但同时希望阻止 ST 访问本地网络上其他设备时，建议使用此功能。

#### 什么是"私有地址"？ {#what-is-considered-a-private-address}

* 回环地址：IPv4 的 `127.0.0.0/8` 和 IPv6 的 `::1/128`。
* IPv4 私有地址范围：A 类（`10.0.0.0/8`）、B 类（`172.16.0.0/12`）、C 类（`192.168.0.0/16`）。
* 链路本地地址：IPv4 的 `169.254.0.0/16` 和 IPv6 的 `fe80::/10`。
* IPv6 的唯一本地地址：`fc00::/7`。

#### 开关私有地址白名单

要启用私有地址白名单，请编辑 SillyTavern 根目录下的 `config.yaml` 文件：

```yaml
privateAddressWhitelist:
    enabled: true
```

#### 向白名单添加私有地址

默认情况下，这仅允许服务器向回环地址（`127.0.0.1` 和 `::1`）发出请求。要向白名单添加更多私有地址，请将其包含在 `privateAddressWhitelist.allowedRanges` 部分中：

```yaml
privateAddressWhitelist:
  allowedRanges:
    - "127.0.0.0/8"
    - "::1/128"
    - "192.168.0.0/16"
```

此示例允许服务器向 `192.168.x.x` 范围内的任何地址以及回环地址发出请求，同时仍阻止对其他私有 IP 范围的访问。

### 主机白名单

在不使用 HTTPS 的情况下通过网络托管服务器时，强烈建议启用请求主机验证。这有助于防止各种攻击，例如 DNS 重绑定攻击。默认情况下，SillyTavern 服务器在首次收到来自未识别主机的连接时，会在控制台中记录一条消息。

#### 开关主机白名单

要启用主机白名单，请编辑 SillyTavern 根目录下的 `config.yaml` 文件：

```yaml
hostWhitelist:
    enabled: true
```

#### 添加受信任的主机

要将主机名添加到受信任主机列表中，请将其包含在 `hostWhitelist.hosts` 部分：

!!!tip 提示
不要添加 `localhost` 或 IP 地址（例如 `127.0.0.1` 或 `::1`）。这些始终被视为受信任的。

要添加一系列主机，请使用前导点。例如，添加 `.trycloudflare.com` 将信任 `trycloudflare.com` 以及任何子域名，如 `example.trycloudflare.com`。
!!!

```yaml
hostWhitelist:
  hosts:
    - "example.com"
    - ".trycloudflare.com"
```

#### 开关控制台消息

要禁用针对未识别主机的控制台消息，请将 `hostWhitelist.scan` 选项设置为 `false`：

```yaml
hostWhitelist:
    scan: false
```

## 连接到您的 SillyTavern 实例

### 获取 ST 宿主机的 IP 地址

设置白名单后，您需要获取运行 ST 的设备的 IP 地址。

如果运行 ST 的设备与您在同一 WiFi 网络，则使用该设备的内部 WiFi IP：

* 在 Windows 上：按 Windows 键 > 在搜索栏中输入 `cmd.exe` > 在控制台中输入 `ipconfig` 并按 Enter > 查找 `IPv4` 条目。

如果您（或其他人）希望在不处于同一网络的情况下连接到您托管的 ST，则需要 ST 宿主设备的公网 IP。

* 在 ST 宿主设备上，访问[此页面](https://whatismyipaddress.com/)并查找 `IPv4`。这就是远程设备连接时所使用的地址。

### 连接到 ST 服务器

无论您获取的是哪种 IP，都需要将该 IP 地址和端口号输入到远程设备的网页浏览器中。

同一 WiFi 网络上 ST 宿主的典型地址如下所示：

`http://192.168.0.5:8000`

使用 http:// 而非 https://

### 连接日志 {#connection-logging}

到服务器的新连接会显示在控制台窗口中，并记录在 SillyTavern 数据目录下的 `access.log` 文件中。

与服务器位于同一台机器上的浏览器的控制台消息如下所示：

```txt
New connection from 127.0.0.1; User Agent: ...
```

与服务器位于同一网络中不同机器上的浏览器的控制台消息可能如下所示：

```txt
New connection from 192.168.116.187; User Agent: ...
```

如果连接被拒绝，控制台消息将如下所示：

```txt
New connection from 192.168.116.211; User Agent: ...

Forbidden: Connection attempt from 192.168.116.211. If you are attempting to connect, 
please add your IP address in whitelist or disable whitelist mode in config.yaml in 
root of SillyTavern folder.
```

`access.log` 将包含带有时间戳的连接信息，但不会记录连接是否被接受或拒绝。

### 故障排除

仍然无法连接？

* 如果连接尝试[出现在控制台](#connection-logging)中但被拒绝，则是[白名单问题](#whitelist-based-access-control)。
* 如果 ST 正在监听远程连接，但连接尝试未出现在控制台中，则是[网络问题](#network-issues)。
* 如果 ST 未在监听远程连接，则是[配置读取问题](#allowing-remote-connections)。

#### 网络问题 {#network-issues}

* 在 Windows 上，应用程序可能被应用程序防火墙阻止。最快的解决方法是卸载并重新安装 node.js，并在防火墙提示时允许其访问网络。否则，您需要手动在 Windows 应用程序防火墙中允许 node.js 应用程序通过。
* 在 Windows 11 上，请在"设置 > 网络和 Internet > 以太网"中启用"私有网络"配置文件类型。这对于 Windows 11 **非常重要**，否则即使配置了上述防火墙规则，也无法连接。
* 在 Linux 上，您可能需要在防火墙中放行该端口。执行命令 `sudo ufw allow 8000` 即可允许端口 8000 上的流量。

不要修改路由器上的端口转发设置。在本地网络内访问 ST 并不需要这样做，而且可能会将您的服务器暴露在互联网上。

如果您正尝试从[本地网络外部](remote-connections.md)访问 ST 服务器但无法连接，请先判断问题是出在远程设备与隧道/VPN 端点之间，还是出在服务器上的隧道端点与 ST 服务之间，以免在错误的地方花费大量时间进行故障排除。

## HTTPS

### 使用 TLS/SSL 启动 SillyTavern

!!!tip
SSL 也可以通过 `config.yaml` 文件进行配置：[SSL 配置](/Administration/config-yaml.md#ssl-configuration)。
!!!

要加密进出 ST 实例的流量，请使用 `--ssl` 标志启动服务器。

示例：

```bash
node server.js --ssl
```

默认情况下，ST 将在 `certs` 文件夹中查找您的证书。如果文件位于其他位置，可以使用 `--keyPath` 和 `--certPath` 参数。

示例：

```bash
node server.js --ssl --keyPath /home/user/certificates/privkey.pem --certPath /home/user/certificates/cert.pem
```

运行 SillyTavern 的用户需要对证书文件拥有读取权限。

### 如何获取证书

获取证书最简单、最快捷的方式是使用 [certbot](https://letsencrypt.org/getting-started/)。

### Docker 中的证书

!!!warning
出于安全和隐私原因，如果您正在构建 Docker 镜像，请勿将 SSL 证书包含在镜像内。请改用卷挂载在运行时提供证书。
!!!

在 Docker 中运行 SillyTavern 时，提供 SSL 证书的推荐方式是将其放置在 `/config` 卷挂载中。这样您无需重新构建容器镜像即可管理证书。

1. 将证书文件（例如 `privkey.pem` 和 `cert.pem`）放置在挂载到容器中 `/config` 的本地配置目录中。

2. 更新 `config.yaml` 以引用证书：

    ```yaml
    ssl:
      enabled: true
      certPath: ./config/cert.pem
      keyPath: ./config/privkey.pem
    ```

3. 重启 Docker 容器以应用更改。
