---
route: /
---

# 什么是 SillyTavern？

![SillyTavern - 为高级用户打造的 LLM 前端界面](/static/banner.png)

SillyTavern（简称 ST）是一个本地安装的用户界面,允许您与文本生成 LLM、图像生成引擎和 TTS 语音模型进行交互。我们的目标是为用户提供尽可能多的 LLM 提示词实用工具和控制能力,将陡峭的学习曲线视为乐趣的一部分。

SillyTavern 是由一群热衷于 LLM 的社区成员带来的热情项目,将永远保持免费和开源。始于 2023 年 2 月,作为 TavernAI 1.2.8 的一个分支,SillyTavern 现在已有超过 200 名贡献者和 2 年的独立开发历程,并继续作为精通 AI 的爱好者的领先软件。

## 截图

|   [![API 连接](/static/screenshot1.jpg)](/static/screenshot1.jpg)    |  [![聊天界面](/static/screenshot2.jpg)](/static/screenshot2.jpg)   |
|:--------------------------------------------------------------------------:|:-----------------------------------------------------------------:|
| [![高级格式化](/static/screenshot3.jpg)](/static/screenshot3.jpg) | [![世界信息](/static/screenshot4.jpg)](/static/screenshot4.jpg) |

## 安装要求

硬件要求很低：任何能运行 NodeJS 18 或更高版本的设备都可以运行。如果您打算在本地机器上进行 LLM 推理,我们建议使用具有至少 6GB 显存的 3000 系列 NVIDIA 显卡。

请根据您的平台选择相应的安装指南：

* [Windows](/Installation/Windows.md)
* [Linux 和 Mac](/Installation/LinuxMacOS.md)
* [Android](/Installation/Android.md)
* [Docker](/Installation/Docker.md)

## 分支

SillyTavern 使用双分支系统开发,以确保所有用户获得流畅的体验。

* `release` -🌟 **推荐大多数用户使用。** 这是最稳定且推荐的分支,仅在主要版本发布时更新。适合大多数用户。通常每月更新一次。
* `staging` - ⚠️ **不推荐日常使用。** 此分支具有最新功能,但请注意它可能随时出现问题。仅适用于高级用户和爱好者。每天更新多次。

## 除了 SillyTavern 之外我还需要什么？

由于 SillyTavern 只是一个界面,您需要访问 LLM 后端来提供推理服务。您可以使用 AI Horde 进行即时开箱即用的聊天。除此之外,我们还支持许多其他本地和云端 LLM 后端：OpenAI 兼容 API、KoboldAI、Tabby 等等。您可以在 [API 连接](/Usage/API_Connections/index.md) 部分阅读更多关于我们支持的 API 的信息。

## 角色卡片

SillyTavern 是围绕"角色卡片"的概念构建的。角色卡片是一组设置 LLM 行为的提示词,在 SillyTavern 中进行持续对话时需要使用。它们的功能类似于 ChatGPT 的 GPTs 或 Poe 的机器人。角色卡片的内容可以是任何内容：抽象场景、为特定任务定制的助手、著名人物或虚构角色。

要在不选择角色卡片的情况下进行快速对话,或者只是测试 LLM 连接,只需在打开 SillyTavern 后在[欢迎屏幕](/Usage/welcome-assistants.md)的输入栏中输入您的提示即可。这将创建一个空的"助手"角色卡片,您可以稍后自定义。

要了解如何定义角色卡片的一般思路,请参考默认角色（Seraphina）或从"下载扩展和资源"菜单下载精选的社区制作卡片。

您也可以从头开始创建自己的角色卡片。有关更多信息,请参阅[角色设计](/Usage/Characters/characterdesign.md)指南。

## 主要功能

* 带有许多社区制作预设的高级[文本生成设置](/Usage/Prompts/advancedformatting.md)
* [世界信息支持](Usage/worldinfo.md)：为您的角色卡片创建丰富的背景或节省令牌
* [群聊](/Usage/Characters/groupchats.md)：多机器人房间,让角色与您和/或彼此交谈
* [丰富的 UI 自定义选项](/Usage/User_Settings/uicustomization.md)：主题颜色、背景图片、自定义 CSS 等
* [用户角色](/Usage/personas.md)：让 AI 了解您的一些信息,以增强沉浸感
* [内置 RAG 支持](/Usage/Characters/data-bank.md)：将文档添加到您的聊天中供 AI 参考
* 广泛的[聊天命令](/Usage/Chatting/slashcommands.md)子系统和自己的[脚本引擎](/For_Contributors/st-script.md)

## 扩展

SillyTavern 支持扩展功能。

* [角色情感表情（精灵图）](/extensions/Expression-Images.md)
* [聊天历史自动摘要](/extensions/Summarize.md)
* 自动 UI 和[聊天翻译](extensions/Translation.md)
* [Stable Diffusion/FLUX/DALL-E 图像生成](/extensions/Stable-Diffusion.md)
* [AI 回复消息的文本转语音（通过 ElevenLabs、Silero 或系统 TTS）](/extensions/TTS.md)
* [网络搜索功能,为您的提示添加额外的真实世界背景](/extensions/WebSearch.md)
* 更多扩展可从"下载扩展和资源"菜单下载。

## 如何直接联系开发者？

* Discord: cohee, rossascends, wolfsblvt
* Reddit: [/u/RossAscends](https://www.reddit.com/user/RossAscends/), [/u/sillylossy](https://www.reddit.com/user/sillylossy/), [u/Wolfsblvt](https://www.reddit.com/user/Wolfsblvt/)
* [提交 GitHub 问题](https://github.com/SillyTavern/SillyTavern/issues)

## 我喜欢你们的项目！如何贡献？

* 我们欢迎拉取请求！按照[贡献指南](https://github.com/SillyTavern/SillyTavern/blob/release/CONTRIBUTING.md)开始。
* 我们也欢迎有帮助和信息的错误报告,请使用 GitHub 中提供的模板。
* 我们不接受项目本身的金钱捐赠。

## 个人捐赠

我们感谢您对个人贡献者的支持,但这不会影响 SillyTavern 的整体开发方向。

* RossAscends 有个人 [Patreon](https://www.patreon.com/RossAscends) 和 [Kofi](https://ko-fi.com/rossascends)

## 许可证

SillyTavern 是一个免费和开源项目,根据 [AGPL-3.0 许可证](https://github.com/SillyTavern/SillyTavern/blob/release/LICENSE) 发布。
