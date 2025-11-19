---
route: /
---

# SillyTavern 是什么？

![SillyTavern - LLM Frontend for Power Users](/static/banner.png)

SillyTavern（简称 ST）是一个本地安装的用户界面，允许您与文本生成 LLM、图像生成引擎和 TTS 语音模型进行交互。我们的目标是为用户提供尽可能多的实用功能和对 LLM 提示词的控制能力，并将陡峭的学习曲线视为乐趣的一部分。

SillyTavern 是一个由热衷于 LLM 的专注社区带来的激情项目，并将永远保持免费和开源。从 2023 年 2 月作为 TavernAI 1.2.8 的分支开始，SillyTavern 现在拥有超过 200 名贡献者和 2 年的独立开发历史，并继续作为精通 AI 的爱好者的领先软件。

## 截图

|   [![API Connection](/static/screenshot1.jpg)](/static/screenshot1.jpg)    |  [![Chat UI](/static/screenshot2.jpg)](/static/screenshot2.jpg)   |
|:--------------------------------------------------------------------------:|:-----------------------------------------------------------------:|
| [![Advanced Formatting](/static/screenshot3.jpg)](/static/screenshot3.jpg) | [![World Info](/static/screenshot4.jpg)](/static/screenshot4.jpg) |

## 安装要求

硬件要求很低：它可以在任何能够运行 NodeJS 18 或更高版本的设备上运行。如果您打算在本地机器上进行 LLM 推理，我们建议使用至少 6GB 显存的 3000 系列 NVIDIA 显卡。

按照您的平台选择安装指南：

* [Windows](/Installation/Windows.md)
* [Linux and Mac](/Installation/LinuxMacOS.md)
* [Android](/Installation/Android.md)
* [Docker](/Installation/Docker.md)

## 分支

SillyTavern 使用双分支系统进行开发，以确保所有用户都能获得流畅的体验。

* `release` -🌟 **推荐给大多数用户。** 这是最稳定和推荐的分支，仅在推送主要版本时更新。适合大多数用户。通常每月更新一次。
* `staging` - ⚠️ **不推荐日常使用。** 此分支具有最新功能，但请注意它可能随时出现问题。仅供高级用户和爱好者使用。每天更新数次。

## 除了 SillyTavern 之外我还需要什么？

由于 SillyTavern 只是一个界面，您需要访问 LLM 后端来提供推理。您可以使用 AI Horde 进行即时开箱即用的聊天。除此之外，我们支持许多其他本地和基于云的 LLM 后端：OpenAI 兼容 API、KoboldAI、Tabby 等等。您可以在 [API Connections](/Usage/API_Connections/index.md) 部分阅读更多关于我们支持的 API 的信息。

## 角色卡

SillyTavern 围绕"角色卡"的概念构建。角色卡是设置 LLM 行为的提示词集合，是在 SillyTavern 中进行持久对话所必需的。它们的功能类似于 ChatGPT 的 GPT 或 Poe 的机器人。角色卡的内容可以是任何东西：抽象场景、为特定任务量身定制的助手、名人或虚构角色。

要在不选择角色卡的情况下进行快速对话，或者只是测试 LLM 连接，只需在打开 SillyTavern 后在[欢迎屏幕](/Usage/welcome-assistants.md)的输入栏中键入您的提示词输入。这将创建一个空的"助手"角色卡，您可以稍后自定义。

要了解如何定义角色卡的大致想法，请参阅默认角色（Seraphina）或从"下载扩展和资源"菜单下载精选的社区制作卡片。

您还可以从头开始创建自己的角色卡。有关更多信息，请参阅[角色设计](/Usage/Characters/characterdesign.md)指南。

## 主要功能

* 高级[文本生成设置](/Usage/Prompts/advancedformatting.md)，包含许多社区制作的预设
* [World Info 支持](Usage/worldinfo.md)：创建丰富的背景知识或节省角色卡上的 token
* [群组聊天](/Usage/Characters/groupchats.md)：多机器人房间，让角色与您和/或彼此交谈
* [丰富的 UI 自定义选项](/Usage/User_Settings/uicustomization.md)：主题颜色、背景图像、自定义 CSS 等
* [用户人设](/Usage/personas.md)：让 AI 了解您的一些信息，以获得更强的沉浸感
* [内置 RAG 支持](/Usage/Characters/data-bank.md)：向您的聊天添加文档供 AI 参考
* 广泛的[聊天命令](/Usage/Chatting/slashcommands.md)子系统和自己的[脚本引擎](/For_Contributors/st-script.md)

## 扩展

SillyTavern 支持可扩展性。

* [角色情感表情（精灵图）](/extensions/Expression-Images.md)
* [聊天历史自动摘要](/extensions/Summarize.md)
* 自动 UI 和[聊天翻译](extensions/Translation.md)
* [Stable Diffusion/FLUX/DALL-E 图像生成](/extensions/Stable-Diffusion.md)
* [AI 响应消息的文本转语音（通过 ElevenLabs、Silero 或操作系统的系统 TTS）](/extensions/TTS.md)
* [Web 搜索功能，用于向提示词添加额外的真实世界上下文](/extensions/WebSearch.md)
* 更多扩展可从"下载扩展和资源"菜单下载。

## 如何直接联系开发者？

* Discord: cohee, rossascends, wolfsblvt
* Reddit: [/u/RossAscends](https://www.reddit.com/user/RossAscends/), [/u/sillylossy](https://www.reddit.com/user/sillylossy/), [u/Wolfsblvt](https://www.reddit.com/user/Wolfsblvt/)
* [提交 GitHub issue](https://github.com/SillyTavern/SillyTavern/issues)

## 我喜欢你们的项目！如何贡献？

* 我们欢迎 pull request！按照[贡献指南](https://github.com/SillyTavern/SillyTavern/blob/release/CONTRIBUTING.md)开始。
* 我们也欢迎使用 GitHub 中提供的模板提交有帮助且信息充分的错误报告。
* 我们不接受对项目本身的金钱捐赠。

## 个人捐赠

感谢您对个人贡献者的支持，但这不会影响 SillyTavern 的整体发展方向。

* RossAscends 有个人 [Patreon](https://www.patreon.com/RossAscends) 和 [Kofi](https://ko-fi.com/rossascends)

## 许可证

SillyTavern 是一个免费开源项目，采用 [AGPL-3.0 许可证](https://github.com/SillyTavern/SillyTavern/blob/release/LICENSE)发布。
