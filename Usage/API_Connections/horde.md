---
route: /usage/api-connections/horde/
---

# AI Horde

## 免责声明

- AI Horde 是一个完全由志愿者运行的众包分布式 GPU 集群。
- 默认情况下，您的输入是匿名发送的，运行 Horde Worker 的人无法看到响应。
- 但是，由于它是一个开源程序，恶意 Worker 可能会修改代码以：
  - 记录您的活动（输入提示词、AI 响应）。
  - 产生不良或冒犯性的响应。

!!!warning
使用 Horde 时**切勿发送**任何个人信息，如姓名、电子邮件地址等。
!!!

打开"仅限可信 Worker"复选框将限制可用 Worker 的选择，只选择那些已在 Horde 上托管一段时间且通常被认为是可信的 Worker。但他们仍然可能会看到提示词，例如通过使用未记录的软件进行托管。

为了帮助减少这个问题，SillyTavern 内置了以下功能：

- 当 Horde Worker 生成聊天响应时，SillyTavern 会记录 Worker 的 ID 和他们使用的模型。
- 将鼠标光标悬停在聊天项目上可以看到这些信息（见下图）。
- 如果您认为收到了恶意响应，可以将这些信息传递给 [AI Horde Discord](https://discord.gg/3DxrhksKzn) 上的 Horde 管理员进行审查，并可能对该 Worker 采取纪律处分。

![Horde Worker 信息弹窗](/static/horde-worker.png)

## 设置

- SillyTavern 可以开箱即用地连接 Horde，无需额外设置。
- 在 ST API 面板中的 API 下拉选择器中选择"AI Horde"。
- 从面板底部的模型选择器中选择一个或多个模型（角色的"AI 大脑"）。
- 选择一个角色并开始聊天。

![ST Kobold Horde API 连接面板](/static/horde-config.png)

!!!warning
默认情况下，您的 SillyTavern 实例连接到 Horde 的低优先级"访客账户"。
这意味着您可能需要等待很长时间才能得到回复。
要减少等待时间，请遵循下面的提示。
!!!

## 提示

- [在 Horde 网站上注册账户](https://aihorde.net/register)，然后将您的 Horde 密钥添加到 SillyTavern Horde API 密钥框中。
- [设置 Horde Worker](https://github.com/Haidra-Org/AI-Horde-Worker#readme) 为他人提供您的 GPU。
  - 让他人使用您的 GPU 可以赚取["Kudos"，一种仅限 Horde 的货币](https://github.com/Haidra-Org/AI-Horde/blob/main/FAQ.md#kudos)。
  - 您的账户拥有的 kudos 越多，从其他 Horde Worker 获得聊天响应的速度就越快。
  - Kudos 也可以用于在 [Stable Horde](https://stablehorde.net) 上创建 AI 图像。
    - SillyTavern 开箱即支持 Stable Horde 图像生成。
- 如果您的 GPU 不够强大无法运行 AI，或者您没有电脑，您仍然可以[通过各种方式参与 Horde 社区以赚取 Kudos](https://github.com/Haidra-Org/AI-Horde/blob/main/FAQ.md#i-dont-have-a-powerful-gpu-how-can-i-get-kudos)。
