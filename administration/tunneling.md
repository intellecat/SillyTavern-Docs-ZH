VPN 和隧道是从世界任何地方安全访问家庭网络的方式。本指南将介绍如何使用 VPN 或隧道从任何地方访问你的 SillyTavern 实例。

## 方法

1. 使用**自建 VPN**。

   许多路由器在管理页面中内置了 VPN 服务器功能（主要是 OpenVPN 或 WireGuard）。请参阅路由器手册进行配置，并将你的设备添加到 VPN。连接后，只需访问你为 SillyTavern 设置的私网 IP 即可正常连接。对于 Windows 用户而言，此方法相对简便。

2. 使用 [**Cloudflare Zero Trust**](https://developers.cloudflare.com/cloudflare-one/)。

   Cloudflare Zero Trust 是 Cloudflare 提供的免费组织功能，支持最多 50 名用户。它将通过 Cloudflare 代理你的流量，并通过 `cloudflared` 将运行 ST 的电脑注册为隧道节点，使你能够像在家一样连接到 ST 实例。

   请注意，创建隧道后，你还需要在路由器中为私网 IP 地址段添加路由并计算 IP CIDR 值，才能通过 Cloudflare Zero Trust 实现完整的本地访问。

3. 使用独立的 **Cloudflare** 或 **[ngrok](https://ngrok.com)** 隧道。

   类似于 AI 后端的连接方式，你也可以通过 Cloudflare 隧道连接你的 ST 实例并打开隧道页面。但每次在外使用 ST 时，都需要复制粘贴 Cloudflare/NGROK 生成的新链接。

4. 使用 **Tailscale**。

   Tailscale 是一款 VPN 服务，可实现对你电脑的安全远程连接。

## Tailscale 配置

Tailscale 是一款 VPN 服务，可实现对你电脑的安全远程连接。Tailscale 服务端有开源实现，你也可以使用 [Headscale](https://github.com/juanfont/headscale) 自建服务端，但这超出了本教程的范围。

### 1. 创建账户

* 前往 [Tailscale 官网](https://tailscale.com/)并创建一个新账户。

**注意：** 对于单人日常使用，Tailscale 永久免费。如果担心隐藏费用，不添加任何付款方式即可。

### 2. 配置客户端

* 前往 [Tailscale 下载页面](https://tailscale.com/download)，在运行 SillyTavern 的设备和你想远程使用的设备上分别下载并安装客户端/应用。
* 在两台设备上使用同一账户登录。
* 前往 [Tailscale 管理页面](https://login.tailscale.com/admin/machines)，批准两台设备。
* 记下两台已连接设备的名称。

### 3. 将设备添加到白名单

* 按照[管理白名单 IP](./remote-connections.md#whitelist-based-access-control) 的说明，将你想用于访问 SillyTavern 的连接设备的机器名添加到 SillyTavern 白名单中。

**注意：** 编辑配置时，如果尚未参考[远程连接](./remote-connections.md#allowing-remote-connections)文档，需将 listen 设置为 true。

### 4. 连接

之后，只要你想从任何地方使用 SillyTavern，只需：

* 在托管 SillyTavern 的电脑和你想远程使用的设备上同时开启 Tailscale。
* 在想要连接的设备上打开浏览器，访问 `http://<运行 ST 的电脑机器名>:8000/`

### 5. 与朋友共享 SillyTavern 实例（可选）

* 让你的朋友创建自己的 Tailscale 账户，并在其设备上安装客户端。
* 前往 [Tailscale 管理页面](https://login.tailscale.com/admin/machines)。
* 将鼠标悬停在托管 SillyTavern 电脑的三点按钮上，点击"Share..."；或点击三点按钮后选择"Sharing settings..."。
* 取消勾选"Allow use as an exit node"（除非你希望朋友能够通过你的电脑路由所有网络流量）。
* 通过邮件发送链接，或切换到"Copy share link"选项卡，点击蓝色大按钮复制链接，然后通过其他方式发送给朋友。
* 朋友点击分享链接后，你的电脑将出现在他们的 Tailscale 网络中。
* 将你自己访问 SillyTavern 使用的链接发送给朋友（参见上一步骤）。

**注意：** 这将使朋友完全访问你电脑上本地运行的所有服务，例如 SillyTavern、automatic1111 等。只有在充分信任对方的情况下才可这样做。
