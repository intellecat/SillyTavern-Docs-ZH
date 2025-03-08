---
label: Extensions
icon: plug
expanded: true
order: 35
---

# 扩展

SillyTavern 自带许多可以在扩展面板中启用或禁用的扩展。扩展可以添加新功能、改变现有功能的行为或为您的 AI 提供额外的内容。您可以从扩展面板中的"下载扩展和资源"菜单安装更多扩展。

## 扩展面板

要打开或关闭扩展面板，请在顶部栏中选择 **<i class="fa-solid fa-cubes fa-fw"></i> Extensions**。

- **<i class="fa-solid fa-cubes"></i> 管理扩展**：激活、停用和更新扩展
- **下载扩展和资源**：从 SillyTavern 仓库安装[更多扩展](#可安装扩展)、角色、声音和背景
- **扩展更新通知**：选中此项以在已安装的扩展有更新时收到通知
- **<i class="fa-solid fa-cloud-arrow-down"></i> 安装扩展**：从 Git 仓库 URL 导入扩展

!!! warning
使用第三方扩展可能会产生意想不到的副作用并可能带来安全风险。
在通过 **<i class="fa-solid fa-cloud-arrow-down"></i> Install extension** 导入扩展之前，请务必确保您信任来源。
我们对第三方扩展造成的任何损害不承担责任。
!!!

## 内置扩展

这些扩展内置于 SillyTavern 中，无需安装。可以在扩展面板中启用或禁用它们。

:::callout
**[Chat Translation](Translation.md)**

将聊天消息翻译成不同的语言
:::

:::callout
**[Image Captioning](captioning.md)**

从图像生成文本，让您的 AI 能够"看到"并回应对话中的视觉内容
:::

:::callout
**[Image Generation](Stable-Diffusion.md)**

使用本地或基于云的 Stable Diffusion、FLUX 或 DALL-E API 生成图像
:::

:::callout
**[Expression Images](Expression-Images.md)**

您的 AI 角色的图像（又称"精灵图"），显示在聊天窗口旁边或后面
:::

:::callout
**[Summarize](Summarize.md)**

聊天历史的自动摘要
:::

:::callout
**[Chat Vectorization](Chat-vectorization.md)**

从聊天历史中找到相关消息并将其添加到上下文中
:::

:::callout
**[Text To Speech](TTS.md)**

通过 ElevenLabs、Silero、您的系统 TTS、**[AllTalk](AllTalk.md)**、**[XTTS](XTTS.md)** 等为您的聊天消息提供语音叙述
:::

:::callout
**[Quick Reply](/For_Contributors/st-script.md#quick-replies-script-library-and-auto-execution)**

一键回复聊天消息，运行命令和 STscripts 等
:::

:::callout
**Token Counter**

将文本转换为 token 并计算 token 数量
:::

---

## 可安装扩展

从**扩展**中的"下载扩展和资源"菜单安装这些扩展。

!!! info 扩展不是 Extras
Extras 项目已于 2024 年 4 月停止。您不需要安装 Extras 来使用扩展。
!!!

:::callout
**[Blip](Blip.md)**

以可变速度播放角色消息的文本动画，并伴随声音播放。
:::

:::callout
**[Dynamic Audio](Dynamic-Audio.md)**

为您的聊天添加沉浸式背景音乐和环境声音。
:::

:::callout
**[EmulatorJS](EmulatorJS.md)**

直接在 SillyTavern 聊天中玩复古游戏机游戏。
:::

:::callout
**[Live2d](Live2d.md)**

添加对 live2d 模型的支持。可自定义表情、动画和互动。
:::

:::callout
**[Objective](Objective.md)**

为 AI 设置在聊天过程中要达到的目标。
:::

:::callout
**[RVC](RVC.md)**

为文本转语音模块添加实时语音克隆功能。
:::

:::callout
**[Speech Recognition](Speech-Recognition.md)**

使用浏览器或 extras 将您的语音转换为文本。
:::

:::callout
**[VRM](VRM.md)**

添加对 VRM 模型的支持。可自定义表情、动画和互动。
:::

:::callout
**[Web Search](WebSearch.md)**

将网络搜索结果添加到 LLM 提示词中。
:::

:::callout
**[AccuWeather](https://github.com/SillyTavern/Extension-AccuWeather)**

使用 AccuWeather API 提供天气信息作为斜杠命令或函数工具。
:::

:::callout
**[Chat Top Bar](https://github.com/SillyTavern/Extension-TopInfoBar)**

在聊天窗口顶部添加一个带有快速操作快捷方式的栏。
:::

:::callout
**[Chess](https://github.com/SillyTavern/SillyTavern-Chess)**

与 LLM 下国际象棋。
:::

:::callout
**[Code Runner](https://github.com/SillyTavern/Extension-CodeRunner)**

允许从聊天中的代码块运行 JavaScript 和 STscript 代码。
:::

:::callout
**[D&D Dice](https://github.com/SillyTavern/Extension-Dice)**

一套 7 个经典的 D&D 骰子，满足您所有的掷骰需求。
:::

:::callout
**[Duplicate Finder](https://github.com/SillyTavern/Extension-DupeFinder)**

添加按相似度组对角色进行聚类的功能，以轻松找到重复项。
:::

:::callout
**[Emoji Picker](https://github.com/SillyTavern/Extension-EmojiPicker)**

添加一个按钮，可以快速在聊天消息中插入表情符号。
:::

:::callout
**[Group Greetings](https://github.com/SillyTavern/Extension-GroupGreetings)**

允许设置特定于群组聊天的替代问候语。
:::

:::callout
**[Group SendAs](https://github.com/SillyTavern/SillyTavern-GroupSendAs)**

添加一个按钮，可以快速为选定的群组成员插入 /sendas 命令模板。
:::

:::callout
**[HypeBot](https://github.com/SillyTavern/Extension-HypeBot)**

使用 NovelAI 的 HypeBot 引擎根据您最近的聊天显示个性化建议。需要有效的 NovelAI 订阅。
:::

:::callout
**[Idle](https://github.com/SillyTavern/Extension-Idle)**

在用户空闲一段时间后添加"空闲提示"以自然地继续对话。
:::

:::callout
**[Image Metadata Viewer](https://github.com/SillyTavern/Extension-ImageMetadataViewer)**

查看附加到聊天中的放大图像的元数据。
:::

:::callout
**[LaTeX](https://github.com/SillyTavern/Extension-LaTeX)**

在聊天消息中渲染 LaTeX 和 AsciiMath 公式。
:::

:::callout
**[Mermaid](https://github.com/SillyTavern/Extension-Mermaid)**

为 SillyTavern 聊天添加 Mermaid 图表和流程图渲染功能。
:::

:::callout
**[Notebook](https://github.com/SillyTavern/Extension-Notebook)**

添加一个存储笔记的地方。支持富文本格式。
:::

:::callout
**[Parameter Randomizer](https://github.com/SillyTavern/Extension-Randomizer)**

添加在每次生成时随机化 API 设置滑块的功能。
:::

:::callout
**[Prome Visual Novel Extension](https://github.com/Bronya-Rand/Prome-VN-Extension)**

通过更多功能（焦点模式、信箱模式等）增强当前的视觉小说体验！
:::

:::callout
**[Prompt Inspector](https://github.com/SillyTavern/Extension-PromptInspector)**

在发送到服务器之前添加检查和编辑输出提示词的选项。
:::

:::callout
**[Push Notifications](https://github.com/SillyTavern/SillyTavern-PushNotifications)**

允许接收聊天消息的推送通知。
:::

:::callout
**[Quick Persona](https://github.com/SillyTavern/Extension-QuickPersona)**

在聊天栏中添加一个用于选择用户个性的下拉菜单。
:::

:::callout
**[RSS](https://github.com/SillyTavern/Extension-RSS)**

通过斜杠命令或函数工具获取 RSS 订阅源的最新新闻。
:::

:::callout
**[Screen Share](https://github.com/SillyTavern/Extension-ScreenShare)**

在您发送消息时为多模态模型提供屏幕图像。
:::

:::callout
**[Silence Player](https://github.com/SillyTavern/Extension-Silence)**

在扩展菜单中添加静音音频播放器。可以帮助防止浏览器标签页在后台被关闭。
:::

:::callout
**[Timelines](https://github.com/SillyTavern/SillyTavern-Timelines)**

为聊天历史添加时间线导航。
:::

:::callout
**[Variable Viewer](https://github.com/LenAnderson/SillyTavern-Variable-Viewer)**

查看和修改变量的简单方法。
:::

:::callout
**[WebLLM](https://github.com/SillyTavern/Extension-WebLLM)**

为扩展提供直接在浏览器中使用语言模型的接口。
:::


