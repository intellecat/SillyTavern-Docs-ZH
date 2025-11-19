---
order: tts
route: /extensions/tts/
---

# TTS

SillyTavern 拥有广泛的 TTS(text-to-speech,文本转语音)选项,用于让语音朗读聊天的部分内容。本页面解释了设置和使用方法。

## 配置 TTS

### TTS Provider 选择

用于选择您想要使用的 TTS 服务。一些选项是免费的,一些需要付费订阅,还有一些在您的 PC 上本地运行。

可用选项(列表可能随时间变化):

- **AllTalk** - 免费,开源本地安装,提供多种 TTS 引擎。请参阅 [AllTalk](./AllTalk.md) 页面了解设置说明。
- **Azure TTS** - 与 Microsoft Edge 相同的声音。需要 Azure 账户和付费订阅。
- **Coqui-TTS**(已弃用)- 免费,需要 Extras API 运行。高性能 Text2Speech 模型(Tacotron、Tacotron2、Glow-TTS、SpeedySpeech)以及 Bark。
- **Edge** - 免费,通过 Azure 运行。当选择 "Plugin" 作为 provider 运行时,您还需要安装[此服务器插件](https://github.com/SillyTavern/SillyTavern-EdgeTTS-Plugin)。其他选项需要 Extras API(已弃用)运行。
- **Electron Hub** - 重用您的 [Electron Hub](https://electronhub.ai/) API key 来访问云端语音(GPT-4o Mini TTS、Microsoft neural voices 等),具有每个模型的控制选项。
- **ElevenLabs** - 需要付费订阅。从 [ElevenLabs](https://elevenlabs.io/) 获取 API key。
- **Google Translate** - Google 提供的免费语音,每种语言一个,质量可能差异很大。
- **Google Gemini TTS** - 需要来自 [Vertex AI](/Usage/API_Connections/google.md#google-vertex-ai) 或 [AI Studio](/Usage/API_Connections/google.md#google-ai-studio) 的 API key,使用 [Gemini TTS](https://cloud.google.com/text-to-speech/docs/gemini-tts) 模型。
- **Kokoro** - 免费,使用 [kokoro.js](https://www.npmjs.com/package/kokoro-js) 在您的浏览器中本地运行模型。但是,[某些浏览器](https://caniuse.com/webgpu)可能不支持 device 选项的 WebGPU。
- **MiniMax** - 需要来自 [MiniMax](https://www.minimax.io/) 的 API key。请参阅 [MiniMax TTS](./MiniMaxTTS.md) 页面了解设置说明。
- **Novel** - 需要付费的 NovelAI 订阅,由 NovelAI 的 TTS 引擎生成
- **OpenAI** - 需要付费 API key,使用 OpenAI 的 TTS 模型。
- **Pollinations** - 免费访问 OpenAI TTS 模型,但有速率限制。[网站](https://pollinations.ai/)。
- **Silero** - 免费,在您的 PC 上运行,质量可能差异很大。需要[专用 API 服务器](https://github.com/ouoertheo/silero-api-server)安装或 Extras API(已弃用)。
- **System** - 使用您的操作系统 TTS 引擎(如果存在)。质量可能因操作系统而异。
- **XTTS** - 免费,需要专用 API 服务器安装。请参阅 [XTTS](./XTTS.md) 页面了解设置说明。

### 复选框

- **Enabled** - 打开/关闭 TTS 播放
- **Auto Generation** - 让 TTS 在新消息进入聊天时自动开始播放
- **Only narrate "quotes"** - 将 TTS 播放限制为仅包含 `"引号"` 内的文本。这将 `*包括星号行内的 "引号"*`(内部变量名 = `narrate_quoted_only`)
- **Ignore \*text, even "quotes", inside asterisks\*** - TTS 不会播放 `*星号*` 内的任何文本,即使是 "引号"(内部变量名 = `narrate_dialogues_only`)
- *同时勾选 "only narrate quotes" 和 "ignore asterisks" 复选框将导致 TTS 仅读取不在星号内的 "引号",并忽略其他所有内容。*
- **Narrate only the translated text** - 这将使 TTS 仅朗读翻译后的文本。

给定示例文本:`*Cohee approaches you with a faint "nya"* "Good evening, senpai", she says.`
以下表格显示了根据 **Ignore \*text, even "quotes", inside asterisks\*** 和 **Only narrate "quotes"** 的布尔状态,文本将如何修改:

| **Ignore \*text, even "quotes", inside asterisks\*** 	 | **Only narrate "quotes"**	 | **输出**                                                                   |
|:-------------------------------------------------------|:---------------------------|:--------------------------------------------------------------------------|
| Disabled                                               | 	Disabled	                 | Cohee approaches you with a faint "nya" "Good evening, senpai", she says. |
| Disabled                                               | Enabled	                   | "nya"... "Good evening, senpai"                                           |
| Enabled	                                               | Disabled	                  | "Good evening, senpai", she says.                                         |
| Enabled	                                               | Enabled	                   | "Good evening, senpai"                                                    |

### 滑块

这些会根据您选择的 API 而变化。

### 按钮

- **Apply** - 在设置 TTS API 和编辑 voice map 后必须点击此按钮。
- **Refresh** - 从所选 TTS API 重新加载语音列表。
- **Available voices** - 加载一个弹出窗口,显示所选 API 的所有可用语音,并让您使用示例对话预览它们。

## 使用 TTS

1. 点击 "Enable" 复选框,否则什么都不会发生。
2. 如果您希望 TTS 在每次新消息到达聊天时自动启动,请点击 "Auto-generation" 复选框。
3. 可选地,点击任何消息右上角的扩音器图标以按需播放。
4. 点击右下角的 "Stop" 按钮(位于 wand 菜单内)以停止任何播放。

### Voice Map

您必须为 TTS 提供 voice map,否则它不知道应该为每个角色使用什么语音。要设置 voice map,首先打开与您想要分配语音的角色的聊天和/或选择要分配语音的用户角色,然后从下拉菜单中选择 TTS provider 列出的语音。如果您看不到语音和/或角色列表,请确保您的 TTS provider 配置正确并点击 "Refresh"。某些 provider(如 OpenAI-compatible 或 NovelAI)需要您手动填充语音列表。
