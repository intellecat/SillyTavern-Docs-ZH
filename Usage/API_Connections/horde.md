---
route: /usage/api-connections/horde/
---

# AI Horde

## 免责声明

- AI Horde 是一个由志愿者运营的众包分布式 GPU 集群。
- 默认情况下，您的输入会匿名发送，运行 Horde Worker 的人无法看到响应。
- 然而，由于这是一个开源程序，恶意 Worker 可能会修改代码以：
  - 记录您的活动（输入提示词、AI 响应）。
  - 产生不良或冒犯性的响应。

!!!warning
使用 Horde 时**切勿发送**任何个人信息，如姓名、电子邮件地址等。
!!!

开启"Trusted Workers Only"复选框将限制只选择那些在 Horde 上托管了一段时间且通常被认为可信的 Worker。但他们仍然可能看到提示词，例如通过使用未经核算的软件进行托管。

为了帮助减少此问题，SillyTavern 内置了以下功能：

- 当 Horde Worker 生成聊天响应时，SillyTavern 会记录该 Worker 的 ID 及其使用的模型。
- 将鼠标光标悬停在聊天项目上可以看到此信息（见下图）。
- 如果您认为收到了恶意响应，可以将此信息提交给 [AI Horde Discord](https://discord.gg/3DxrhksKzn) 上的 Horde 管理员进行审查，并可能对该 Worker 采取纪律处分。

![Horde Worker Info Popup](/static/horde-worker.png)

## 设置

- SillyTavern 能够开箱即用地连接到 Horde，无需额外设置。
- 从 ST API 面板中的 API 下拉选择器中选择 'AI Horde'。
- 从面板底部的模型选择器中选择一个或多个模型（角色的 'AI 大脑'）。
- 选择一个角色并开始聊天。

![ST Kobold Horde API Connection Panel](/static/horde-config.png)

!!!warning
默认情况下，您的 SillyTavern 实例连接到 Horde 的低优先级 '访客账户'。
这意味着您可能需要等待很长时间才能收到回复。
要减少等待时间，请遵循以下提示。
!!!

## 提示

- [在 Horde 网站上注册账户](https://aihorde.net/register)，然后将您的 Horde key 添加到 SillyTavern Horde API Key 框中。
- [设置 Horde Worker](https://github.com/Haidra-Org/AI-Horde-Worker#readme) 为其他人提供您的 GPU。
  - 让其他人使用您的 GPU 可以赚取 ['Kudos'，一种 Horde 专属货币](https://github.com/Haidra-Org/AI-Horde/blob/main/FAQ.md#kudos)。
  - 您的账户拥有的 kudos 越多，从其他 Horde Worker 获得聊天响应的速度就越快。
  - Kudos 还可用于在 [Stable Horde](https://stablehorde.net) 上创建 AI 图像。
    - SillyTavern 开箱即用地支持 Stable Horde 图像生成。
- 如果您的 GPU 不够强大无法运行 AI，或者您没有计算机，您仍然可以[通过各种方式参与 Horde 社区来赚取 Kudos](https://github.com/Haidra-Org/AI-Horde/blob/main/FAQ.md#i-dont-have-a-powerful-gpu-how-can-i-get-kudos)。
