---
label: VPN 和隧道
order: -40
icon: lock
route: /administration/tunneling/
---

VPN 和隧道是一种从世界任何地方安全访问您的家庭网络的方式。本指南将向您展示如何使用 VPN 或隧道从任何地方访问您的 SillyTavern 实例。

## 方法

1. 使用**自建 VPN**。

   许多路由器都具有在路由器管理页面中托管 VPN 服务器（主要是 OpenVPN 或 WireGuard）的功能。请参考您的路由器手册来设置 VPN 并将您的设备添加到 VPN 中。连接后，只需访问您为 SillyTavern 设置的私有 IP 地址即可正常连接。对用户和 Windows 使用来说都更简单。

2. 使用 [**Cloudflare Zero Trust**](https://developers.cloudflare.com/cloudflare-one/)。

   Cloudflare Zero Trust 是 Cloudflare 中的一项免费组织功能，允许您添加 50 个用户。这将通过 Cloudflare 代理您的流量，通过使用 `cloudflared` 将您的 ST PC 添加为隧道，您可以像在家一样连接到您的 ST 实例。

   请注意，在创建隧道后，您需要添加到路由器私有 IP 地址的路由，并计算 IP CIDR 值，以便使用 Cloudflare Zero Trust 在移动中获得完整的本地访问权限。

3. 使用独立的 **Cloudflare** 或 **[ngrok](https://ngrok.com)** 隧道。

   与 AI 后端可以连接的方式类似，您也可以通过 Cloudflare 隧道连接您的 ST 实例并打开 Cloudflare 隧道页面。但是，每次您想要在移动中使用 ST 时，都必须复制并粘贴由 Cloudflare/NGROK 生成的每个新链接。

4. 使用 **Tailscale**。

   Tailscale 是一个 VPN 提供商，可以实现与您的 PC 的安全远程连接。

## Tailscale 设置

Tailscale 是一个 VPN 提供商，可以实现与您的 PC 的安全远程连接。Tailscale 服务器有一个开源实现，您也可以使用 [Headscale](https://github.com/juanfont/headscale) 托管服务器，但这超出了本教程的范围。

### 1. 创建账户

* 访问 [Tailscale 的网站](https://tailscale.com/) 并创建一个新账户。

**注意：** 对于单人日常使用，Tailscale 将永久免费。如果您担心隐藏费用，只需不添加任何支付选项即可。

### 2. 设置客户端

* 访问 [Tailscale 的下载页面](https://tailscale.com/download) 并在运行 SillyTavern 的设备和您想要从远程位置使用的设备上下载客户端/应用程序。
* 在两个设备上都使用您之前创建的账户登录。
* 访问 [Tailscale 的管理页面](https://login.tailscale.com/admin/machines) 并批准两个设备。
* 记下两个已连接设备的名称。

### 3. 将您的设备添加到白名单

* 按照[管理白名单 IP](remote-connections.md#基于白名单的访问控制) 的说明，将您的连接设备的机器名称（您想要用来使用 SillyTavern 的设备）添加到 SillyTavern 的白名单中。

### 4. 连接

现在，无论何时您想要从任何地方使用 SillyTavern，您只需要：

* 在托管 SillyTavern 的 PC 和想要远程使用它的设备上都打开 Tailscale。
* 在想要连接的设备上打开浏览器，访问 `http://<运行 st 的 PC 的机器名称>:8000/`

### 5. 与朋友共享 SillyTavern 实例（可选）

* 告诉您的朋友创建他们自己的 Tailscale 账户并在他们的设备上下载客户端。
* 访问 [Tailscale 的管理页面](https://login.tailscale.com/admin/machines)。
* 将鼠标悬停在托管 SillyTavern 的 PC 的三点按钮上并点击"共享..."，或点击三点按钮并点击"共享设置..."。
* 取消选中"允许用作出口节点"（除非您想让您的朋友能够通过您的 PC 路由他们的所有互联网流量）。
* 通过电子邮件发送链接，或切换到"复制共享链接"标签，点击相同文本的大蓝色按钮，并以其他方式将其发送给您的朋友。
* 点击您的共享链接后，您的朋友将在他们的 Tailscale 网络中看到您的 PC 出现。
* 按照上一步中的说明，将您用来访问 SillyTavern 的相同链接发送给您的朋友。

**注意：** 这将给予您的朋友对您 PC 上运行的任何本地服务（如 SillyTavern、automatic1111 等）的完全访问权限。只有在您真正信任您的朋友时才这样做。
