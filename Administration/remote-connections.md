---
icon: rss
order: -30
route: /usage/remoteconnections/
---

# 远程连接

最常见的情况是人们想在同一个 WiFi 网络内通过手机使用 SillyTavern，而 PC 运行着 ST 服务器。

这也是允许来自本地网络外部的远程连接的第一步。

!!!warning
您不应使用端口转发将您的 ST 服务器暴露到互联网。相反，使用 VPN 或隧道服务，如 Cloudflare Zero Trust、ngrok 或 Tailscale。有关更多信息，请参阅 [VPN 和隧道](tunneling.md)指南。
!!!

!!!danger 免责声明
**在未首先确保适当的安全措施的情况下，切勿在开放的互联网上托管任何实例。**

**对于因不当或不充分的安全实施而导致未经授权访问造成的任何损害或损失，我们概不负责。**
!!!

## 允许远程连接

默认情况下，ST 服务器仅接受来自运行它的机器（localhost）的连接。要允许它监听来自其他设备的连接，请在 `config.yaml` 中将 `listen` 选项设置为 `true`。

!!! 如果您直接在 SillyTavern 文件夹中搜索 `config.yaml`，您可能会找到两个文件。
本文档中对 `config.yaml` 的所有修改都是指 SillyTavern 根目录中的那个（/SillyTavern/config.yaml），而不是 `/SillyTavern/default/config.yaml`。
!!!

```yaml
# 监听传入连接
listen: true
```

当 ST 监听远程连接时，您应该在控制台中看到此消息：

```txt
SillyTavern is listening on IPv4: 0.0.0.0:8000
```

以及一些关于这意味着什么的说明。

当 ST **未**监听远程连接时，您应该在控制台中看到此消息：

```txt
SillyTavern is listening on IPv4: 127.0.0.1:8000
```

## 访问控制配置

启用远程连接监听后，您必须配置至少一种访问控制方法。否则，服务器将不会启动。

### 基于白名单的访问控制

要通过白名单启用访问控制，请编辑 SillyTavern 根目录中的 `config.yaml` 文件（`/SillyTavern/config.yaml`）：

1. 至少启动一次 SillyTavern 以生成必要的配置文件。
2. 在文本编辑器中打开 `/SillyTavern/config.yaml`。
3. 找到 `whitelist` 部分并添加您希望允许的 IP 地址：
    * 分别列出每个 IP 地址。
    * 确保包含 `127.0.0.1`，否则您将无法从主机连接。
    * 支持单个 IP、CIDR 掩码（例如 `10.0.0.0/24`）和通配符（`*`）范围。
4. 保存 `config.yaml` 文件。
5. **重新启动您的 SillyTavern 服务器。**

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

    如果不确定您的本地网络地址范围，请使用上面的白名单。

2. 允许两个特定设备连接：

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

要禁用通过白名单进行的访问控制：

* 在 `/SillyTavern/config.yaml` 中将 `whitelistMode` 设置为 `false`。
* 删除或重命名 `whitelist.txt`（如果存在）在 SillyTavern 基本安装文件夹中。
* 重新启动您的 SillyTavern 服务器。

### 不推荐：使用 `whitelist.txt`

!!!info
如果 `whitelist.txt` 存在，它会优先于 `config.yaml` 中的白名单设置。

然而，由于所有其他配置都在 `config.yaml` 中管理，而 `whitelist.txt` 可能会遇到权限问题或被锁定，系统可能会悄悄地恢复使用 `config.yaml` 白名单。

**直接编辑 config.yaml 既简单又可靠。**
!!!

如果您仍然更喜欢使用 whitelist.txt：

1. 在 SillyTavern 基本安装文件夹中创建一个名为 `whitelist.txt` 的新文本文件。
2. 在文本编辑器中打开它并添加允许的 IP 地址。
3. 保存文件并重新启动您的 SillyTavern 服务器。

#### `whitelist.txt` 配置示例

```txt
10.0.0.0/8
172.16.0.0/12
192.168.0.0/16
127.0.0.1
::1
```

这允许本地网络上的任何设备连接。

### 通过 HTTP 基本认证进行访问控制

!!!warning
HTTP 基本认证不提供强大的安全性。

没有速率限制来防止暴力攻击。如果这是一个问题，建议使用具有 TLS 和速率限制的反向代理，以及专用的[认证服务](sso.md)。
!!!

当客户端通过 HTTP 连接时，服务器将要求用户名和密码。**这仅在启用远程连接（listen: true）时有效。**

要启用 HTTP BA，在 SillyTavern 基本目录中打开 `config.yaml` 并搜索 `basicAuthMode`。将 basicAuthMode 设置为 true 并设置用户名和密码。注意：只有在 ST 至少执行过一次后，`config.yaml` 才会存在。

```yaml
basicAuthMode: true
basicAuthUser:
  username: "MyUsername"
  password: "MyPassword"
```

或者，您可以按如下方式启用基本认证：

```yaml
basicAuthMode: true
enableUserAccounts: true
perUserBasicAuth: true
```

在这种 `perUserBasicAuth` 模式下，基本认证的用户名和密码将与任何有密码的有效多用户账户相同。此外，SillyTavern 将直接登录到该账户。**在启用 `perUserBasicAuth` 之前，请确保您有一个带密码的账户。**

保存文件，如果 SillyTavern 已经在运行，请重启它。连接到您的 ST 时，系统应该会提示您输入用户名和密码。用户名和密码都以明文传输。如果您担心这一点，可以通过 HTTPS 提供 ST 服务。

### 主机白名单

在没有 HTTPS 的情况下在网络上托管服务器时，强烈建议启用请求主机验证。这有助于防止各种攻击，例如 DNS 重新绑定。默认情况下，SillyTavern 服务器将在第一次从无法识别的主机连接时记录控制台消息。

### 切换主机白名单

要启用主机白名单，请编辑 SillyTavern 根目录中的 `config.yaml` 文件：

```yaml
hostWhitelist:
    enabled: true
```

### 添加受信任的主机

要将主机名添加到受信任主机列表，请将其包含在 `hostWhitelist.hosts` 部分中：

!!!tip 提示
不要添加 `localhost` 或 IP（例如 `127.0.0.1` 或 `::1`）。这些始终被视为受信任的。

要添加一系列主机，请使用前导点。例如，添加 `.trycloudflare.com` 将信任 `trycloudflare.com` 以及任何子域，如 `example.trycloudflare.com`。
!!!

```yaml
hostWhitelist:
  hosts:
    - "example.com"
    - ".trycloudflare.com"
```

### 切换控制台消息

要禁用无法识别主机的控制台消息，请将 `hostWhitelist.scan` 选项设置为 `false`：

```yaml
hostWhitelist:
    scan: false
```

## 连接到您的 SillyTavern 实例

### 获取 ST 主机的 IP 地址

设置白名单后，您需要 ST 主机设备的 IP。

如果 ST 主机设备在同一个 wifi 网络上，您将使用 ST 主机的内部 wifi IP：

* 对于 Windows：Windows 按钮 > 在搜索栏中输入 `cmd.exe` > 在控制台中输入 `ipconfig`，按 Enter > 查找 `IPv4` 列表。

如果您（或其他人）想在不在同一网络上时连接到您托管的 ST，您需要 ST 主机设备的公共 IP。

* 使用 ST 主机设备时，访问[此页面](https://whatismyipaddress.com/)并查找 `IPv4`。这就是您从远程设备连接时要使用的地址。

### 连接到 ST 服务器

无论您在您的情况下得到了什么 IP，您都将在远程设备的网络浏览器中输入该 IP 地址和端口号。

同一 wifi 网络上的 ST 主机的典型地址如下所示：

`http://192.168.0.5:8000`

使用 http:// 而不是 https://

### 连接日志

新的服务器连接会显示在控制台窗口中，并记录在 SillyTavern 数据目录中的 `access.log` 文件中。

来自服务器所在机器上的浏览器的控制台消息看起来像：

```txt
New connection from 127.0.0.1; User Agent: ...
```

来自与服务器在同一网络上的不同机器上的浏览器的控制台消息可能看起来像：

```txt
New connection from 192.168.116.187; User Agent: ...
```

如果连接被拒绝，控制台消息将看起来像：

```txt
New connection from 192.168.116.211; User Agent: ...

Forbidden: Connection attempt from 192.168.116.211. If you are attempting to connect,
please add your IP address in whitelist or disable whitelist mode in config.yaml in
root of SillyTavern folder.
```

`access.log` 将包含带有时间戳的连接信息，但不包含连接是否被接受或拒绝的信息。

### 故障排除

仍然无法连接？

* 如果连接尝试[出现在控制台中](#连接日志)，但被禁止，这是一个[白名单问题](#基于白名单的访问控制)。
* 如果 ST 正在监听远程连接但连接尝试没有出现在控制台中，这是一个[网络问题](#网络问题)。
* 如果 ST 没有监听远程连接，这是一个[读取问题](#允许远程连接)。

#### 网络问题

* 在 Windows 上，应用程序可能被应用程序防火墙阻止。最快的解决方法是卸载并重新安装 node.js，当防火墙提示时，允许它访问网络。否则，您需要手动允许 node.js 应用程序通过 Windows 应用程序防火墙。
* 在 Windows 11 上，在设置 > 网络和 Internet > 以太网中启用私有网络配置文件类型。这对 Windows 11 来说非常重要，否则，即使有上述防火墙规则，您也无法连接。
* 在 Linux 上，您可能需要允许端口通过防火墙。执行此操作的命令是 `sudo ufw allow 8000`。这将允许端口 8000 上的流量。

不要修改路由器上的端口转发设置。这对于在本地网络内访问 ST 不是必需的，并且可能会将您的服务器暴露到互联网。

如果您正在尝试从[本地网络外部](remote-connections.md)访问您的 ST 服务器，并且它不工作，请确定问题是出在远程设备和隧道/VPN 端点之间，还是在服务器上的隧道端点和 ST 服务之间。否则，您将花费大量时间排除错误的问题。

## HTTPS

### 使用 TLS/SSL 启动 SillyTavern

!!!tip
SSL 也可以使用 `config.yaml` 文件进行配置：[SSL 配置](/Administration/config-yaml.md#ssl-configuration)。
!!!

要加密往返于您的 ST 实例的流量，请使用 `--ssl` 标志启动服务器。

示例：

```bash
node server.js --ssl
```

默认情况下，ST 将在 `certs` 文件夹中搜索您的证书。如果您的文件位于其他位置，您可以使用 `--keyPath` 和 `--certPath` 参数。

示例：

```bash
node server.js --ssl --keyPath /home/user/certificates/privkey.pem --certPath /home/user/certificates/cert.pem
```

您运行 SillyTavern 的用户需要对证书文件具有读取权限。

### 如何获取证书

获取证书的最简单、最快的方法是使用 [certbot](https://letsencrypt.org/getting-started/)。
