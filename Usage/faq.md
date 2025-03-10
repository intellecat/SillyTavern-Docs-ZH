---
order: 180
icon: question
---

# 常见问题

## 解释 SillyTavern 是什么

现代 AI 语言模型（如 ChatGPT）已经变得如此强大，其中一些现在可以令人信服地模拟您创建的角色，您可以与之聊天、一起写小说等。例如，您可以让 AI 假装是一位来自中世纪日本的围棋教练 Jubei，它会相应地行动和回应。您可以与 Jubei 进行长时间的聊天，一起去酒馆，决定与武士打斗，任何您能想象的事情，AI 都会配合并围绕这些内容进行写作/反应，充当您的对手和地下城主。您的想象力就是极限。您可以让 AI 假装它是神奇女侠。您还可以指定场景（"神奇女侠和我正在抢劫银行"）、写作风格（"神奇女侠说黑人英语"），或任何您能想到的其他内容。

SillyTavern 是一个促进这些用途的应用程序：

* 它是一个处理与 AI 语言模型通信的用户界面。
* 它让您可以创建新的角色卡片（提示词），并轻松在它们之间切换。
* 它让您可以导入其他人创建的角色。
* 它会保存您与角色的聊天历史，允许您随时恢复、开始新的聊天、查看旧的聊天等。
* 在后台，它会为您做必要的事情来准备 AI 提示词。具体来说，它会发送一个系统提示（AI 的指令），让 AI 遵循某些规则以提高回应的准确性。

## 给我一个 AI 模型选项的概述

SillyTavern 可以与两种类型的 AI 交互：

1. [Web 服务](/Usage/API_Connections/openai.md)（基于云的，通常是付费的，专有的，封闭的）
2. [自托管](/Usage/API_Connections/self-hosted.md)（本地的，免费的，开源的）

### 付费 Web 服务 AI

付费 Web 模型是黑盒子。您付费给公司使用他们的 AI 服务。您在 SillyTavern 中输入您的账户信息，它将代表您连接到您的提供商以使用 AI。

优点：

* 真的很容易上手。
* 最高质量的 AI 写作。

缺点：

* 使用需要付费。
* 所有内容都记录在他们的服务器上。隐私问题。
* 它们经常被审查，会拒绝与您讨论某些主题。

### 自托管 AI

自托管模型是您可以在 PC 上运行的免费模型，但需要强大的 PC 和更多的设置工作。

优点：

* 一旦设置好，即使没有互联网连接也可以免费使用。
* 完全的隐私。您写的所有内容都保存在您自己的 PC 上。
* 有各种各样的模型。作为一项社区驱动的技术，您可以找到适合您想要的特定任务或行为的模型。

缺点：

* 它们不如 <abbr title="最先进的">SOTA</abbr> 模型强大（即，它们写的对话更差，创造力更低等）。
* 运行本地模型需要至少 6GB VRAM 的 GPU。

如果您对使用这些感兴趣，请参考这里的专门指南：[如何使用自托管模型](/Usage/API_Connections/self-hosted.md)。

## 我可以在手机或平板电脑上使用 SillyTavern 吗？

iPhone 和 iPad 无法运行完整的 SillyTavern 应用程序，但由于它只是一个网络界面，您可以在家庭 Wi-Fi 上的另一台计算机上运行它，然后在移动浏览器中访问它。更多信息请参考[远程连接](/Administration/remote-connections.md)。

对于 Android 用户，除了上述方法外，您还可以使用 Termux 应用程序直接在手机上运行完整的 SillyTavern，无需 PC。请参考[安装（Android）](/Installation/Android.md)。（注意：Termux 安装不受官方支持，我们不能保证它会正常工作。）

## 我试图导入 PNG 角色卡片但收到无效的错误。为什么？

两种可能性：

1. 卡片内部没有嵌入定义，只是一个普通的图像文件。当您保存它们时，某些程序或文件管理器会从卡片中删除嵌入的定义。确保您使用的是分享者发布的原始 PNG 文件。
2. PNG 文件实际上是一个带有 `.png` 文件名的 WEBP 文件。您可以尝试在导入前将卡片重命名为 `.webp`，或寻找图像的正确 PNG 版本。

## 我如何创建自己的 AI 角色？

1. 点击角色管理按钮
2. 点击创建新角色
3. 在角色名称下，给出一个名字，比如 Amanda
4. 可选地，点击选择头像按钮为这个角色选择一个肖像图片
5. 在描述下，描述这个角色，并包含您认为与聊天相关的任何信息。例如：```Amanda 是一个在间隔年旅行的学生。她身高 6 英尺，是一名排球运动员。她有运动员的身材。她有长长的棕色头发。她喜欢维多利亚时期的英国，喜欢看与那个时期有关的电视和小说。```
例如，如果您想让 Amanda 友好，那么您可以添加：```Amanda 非常开朗外向。```
6. 在第一条消息下，写下开始新聊天时角色的问候语。例如：```*Amanda 向您挥手* 嘿！你也是背包客吗？```
7. 点击创建角色按钮

现在您有了一个可以聊天的基本角色。从角色列表中选择 Amanda，新的聊天就会开始。

请注意，您可以使用描述和/或第一条消息来创建更具体的场景，和/或在描述中包含您自己。例如：

```txt
描述：
Amanda 是一个在间隔年旅行的学生。她身高 6 英尺，是一名排球运动员。她有运动员的身材。她有长长的棕色头发。她喜欢维多利亚时期的英国，喜欢看与那个时期有关的电视和小说。她一直保守着一个让她心灵沉重的秘密。她在等待合适的人倾诉，但这可能会导致与一个强大的秘密社团的猫鼠游戏。她最近到达了加尔各答。

您是 Rajesh Nahasmapetilon，一位世界著名的印度排球巨星。您在加尔各答散步。Amanda 看到您并兴奋地尖叫。

第一条消息：
*Amanda 跑向您，脸上洋溢着笑容。* Rajesh！我简直不敢相信！我是你的超级粉丝。我卧室里贴着你的海报。
```

您包含的任何相关信息都可以被使用。使用得有多好取决于 AI 模型的能力水平。

注意：一旦创建了角色，您可以返回编辑任何这些信息，除了名字。

## 我的 API 密钥存储在哪里？为什么我看不到它们？

SillyTavern 将您的 API 密钥保存在服务器目录中的 `secrets.json` 文件中。

默认情况下，在您输入它们并重新加载页面后，它们不会暴露给前端。

要启用通过点击 API 块中的按钮查看您的密钥：

1. 在 `config.yaml` 文件中将 `allowKeysExposure` 的值设置为 `true`。
2. 重启 SillyTavern 服务器。

## 为什么 UI 这么慢/不稳定？

* 尝试在用户设置面板上启用无模糊效果（快速 UI）模式。
* 在 UI 主题设置中启用减少动画以移除装饰性动画。
* 确保您的浏览器正在使用硬件加速。

## 如何让 AI 写得更多？

有时 AI 只会用一句话回应，而您希望它更详细。
这通常是本地运行模型的问题。

如果您只是想让机器人从最近回复的结尾继续写，您可以通过在输入栏中不输入任何内容并点击发送来发送一个空的用户消息。这将强制机器人继续故事。

解决策略：

* 降低 `回应长度` 设置的值
* 为角色设计一个好的 `第一条消息`，显示他们说话很啰嗦。AI 模型在得到您期望的写作风格指导时可以有很大改进。
* 在角色的描述框中添加一个短语，如"喜欢说很多话"或"说话非常详细"
* 对您的 `作者注释` 或 `历史后指令提示词` 做同样的事情
* 作为最后的手段，您可以尝试打开 `自动继续`（在用户设置面板中），但这会使回应变得更慢，因为它让 AI 产生连续的小回复，然后将它们全部组合成一个大回复。它也可能与某些 API 选项不兼容。

## 如何让 AI 写得更少？

这主要只是 ChatGPT 或 Claude 等模型的问题。可以应用相同的策略，但方向相反。

* 降低 `回应长度` 设置的值
* 在角色的描述中给出"说话简短"或"不怎么说话"之类的短语。
* 给角色一个简短的第一条消息，为聊天设定基调和期望。
* 确保关闭 `自动继续`。

## 如何让 AI 停止写我的角色的动作，并自己驱动剧情？

这应该在 `作者注释` 中处理，结合以下短语：

* \{\{char\}\} 的回应应该只是对 \{\{user\}\} 的动作被动和反应性的。
* 您的下一个回应应该仅从 \{\{char\}\} 的视角出发。
* 您永远不允许为 \{\{user\}\} 规定动作或言语
