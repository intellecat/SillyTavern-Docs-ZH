---
label: VPN 与隧道
order: -40
icon: lock
route: /administration/tunneling/
---

VPN 和隧道是从世界任何地方安全访问您的家庭网络的方式。本指南将向您展示如何使用 VPN 或隧道从任何地方访问您的 SillyTavern 实例。

## 方法

1. 使用**自建 VPN**。

   一些路由器具有在路由器管理页面中托管 VPN 服务器（主要是 OpenVPN 或 WireGuard）的能力。请参阅您的路由器手册以设置 VPN 并将您的设备添加到 VPN。连接后，只需转到您为 SillyTavern 设置的私有 IP，您就可以正常连接。对用户和 Windows 使用更简单。

2. 使用 [**Cloudflare Zero Trust**](https://developers.cloudflare.com/cloudflare-one/)。

   Cloudflare Zero Trust 是 Cloudflare 中的一项免费组织功能，允许您添加 50 个用户。这将通过 Cloudflare 代理您的流量，通过使用 `cloudflared` 将您的 ST PC 添加为隧道，您可以像在家一样连接到您的 ST 实例。

   请注意，在创建隧道后，您必须向路由器的私有 IP 地址添加路由并计算 IP CIDR 值，以便在外出时使用 Cloudflare Zero Trust 拥有完全的本地访问权限。

3. 使用独立的 **Cloudflare** 或 **[ngrok](https://ngrok.com)** 隧道。

   与 AI 后端连接方式类似，您也可以通过 Cloudflare Tunnel 连接您的 ST 实例并打开 Cloudflare Tunnel 页面。但是，每次您想在外出时使用 ST 时，都必须复制并粘贴 Cloudflare/NGROK 生成的每个新链接。

4. 使用 **Tailscale**。

   Tailscale 是一个 VPN 提供商，可实现与您的 PC 的安全远程连接。

## Tailscale 设置

Tailscale 是一个 VPN 提供商，可实现与您的 PC 的安全远程连接。Tailscale 服务器的开源实现存在，您也可以使用 [Headscale](https://github.com/juanfont/headscale) 托管服务器，但这超出了本教程的范围。

### 1. 创建账户

* 访问 [Tailscale 网站](https://tailscale.com/)并创建一个新账户。

**注意：** 对于单人日常使用，Tailscale 将永久免费。如果您担心隐藏费用，只需不要添加任何付款选项。

### 2. 设置客户端

* 访问 [Tailscale 下载页面](https://tailscale.com/download)并在运行 SillyTavern 的设备和您想从远程位置使用的设备上下载客户端/应用程序。
* 在两台设备上使用您之前创建的账户登录。
* 访问 [Tailscale 管理页面](https://login.tailscale.com/admin/machines)并批准两台设备。
* 记下两台连接设备的名称。

### 3. 将您的设备添加到白名单

* 通过遵循[管理白名单 IP](./remote-connections.md#基于白名单的访问控制)，将您的连接设备的机器名称（您想要使用 SillyTavern 的那个）添加到 SillyTavern 的白名单。

### 4. 连接

现在，每当您想从任何地方使用 SillyTavern 时，您只需：

* 在托管 SillyTavern 的 PC 和您想远程使用它的设备上打开 Tailscale。
* 在想要连接的设备上打开浏览器并访问 `http://<运行 st 的 PC 的机器名称>:8000/`

### 5. 与朋友共享 SillyTavern 实例（可选）

* 告诉您的朋友创建他们自己的 Tailscale 账户并在他们的设备上下载客户端。
* 访问 [Tailscale 管理页面](https://login.tailscale.com/admin/machines)。
* 将鼠标悬停在托管 SillyTavern 的 PC 上的三点按钮上，然后按"Share..."或按三点按钮并按"Sharing settings..."。
* 取消选中"Allow use as an exit node"（除非您希望您的朋友能够通过您的 PC 路由所有互联网流量）。
* 通过电子邮件发送链接或将选项卡更改为"Copy share link"，按相同文本的大蓝色按钮，并以任何其他方式将其发送给您的朋友。
* 在点击您的共享链接后，您的朋友将看到您的 PC 出现在他们的 Tailscale 网络中。
* 向您的朋友发送您用于访问 SillyTavern 的相同链接，如上一步中所述。

**注意：** 这将使您的朋友完全访问在您的 PC 上本地运行的任何服务，如 SillyTavern、automatic1111 等。只有在您真正信任您的朋友时才这样做。
