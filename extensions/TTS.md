---
order: tts
route: /extensions/tts/
---

# TTS

SillyTavern 提供多种 TTS（文字转语音）选项，用于让语音朗读聊天的部分内容。本页介绍设置和使用方法。

## 配置 TTS

### TTS 提供商选择

用于选择您想使用的 TTS 服务。部分选项免费，部分需要付费订阅，部分在您的 PC 上本地运行。

可用选项（列表可能随时间变化）：

- **AllTalk** - 免费、开源的本地安装，提供多种 TTS 引擎。设置说明请参阅 [AllTalk](./AllTalk.md) 页面。
- **Azure TTS** - 与 Microsoft Edge 使用相同的语音。需要 Azure 账户和付费订阅。
- **Coqui-TTS**（已弃用）- 免费，需要 Extras API 才能运行。高性能的文字转语音模型（Tacotron、Tacotron2、Glow-TTS、SpeedySpeech）以及 Bark。
- **Edge** - 免费，通过 Azure 运行。当提供商选择"插件"时，您还需要安装[此服务器插件](https://github.com/SillyTavern/SillyTavern-EdgeTTS-Plugin)。另一种选项需要 Extras API（已弃用）才能运行。
- **Electron Hub** - 复用您的 [Electron Hub](https://electronhub.ai/) API 密钥来访问云端语音（GPT-4o Mini TTS、Microsoft 神经语音等），支持按模型控制。
- **ElevenLabs** - 需要付费订阅。从 [ElevenLabs](https://elevenlabs.io/) 获取 API 密钥。
- **Google Translate** - 谷歌提供的免费语音，每种语言一个，质量差异较大。
- **Google Gemini TTS** - 需要来自 [Vertex AI](/Usage/API_Connections/google.md#google-vertex-ai) 或 [AI Studio](/Usage/API_Connections/google.md#google-ai-studio) 的 API 密钥，使用 [Gemini TTS](https://cloud.google.com/text-to-speech/docs/gemini-tts) 模型。
- **Kokoro** - 免费，使用 [kokoro.js](https://www.npmjs.com/package/kokoro-js) 在浏览器中本地运行模型。但是，[部分浏览器](https://caniuse.com/webgpu)可能不支持 WebGPU 的设备选项。
- **MiniMax** - 需要来自 [MiniMax](https://www.minimax.io/) 的 API 密钥。设置说明请参阅 [MiniMax TTS](./MiniMaxTTS.md) 页面。
- **Novel** - 需要付费 NovelAI 订阅，由 NovelAI 的 TTS 引擎生成。
- **OpenAI** - 需要付费 API 密钥，使用 OpenAI 的 TTS 模型。
- **Pollinations** - 免费访问 OpenAI TTS 模型，但有速率限制。[网站](https://pollinations.ai/)。
- **Silero** - 免费，在您的 PC 上运行，质量差异较大。需要[专用 API 服务器](https://github.com/ouoertheo/silero-api-server)安装或 Extras API（已弃用）。
- **System** - 使用您操作系统的 TTS 引擎（如果存在）。质量因操作系统而异。
- **XTTS** - 免费，需要专用 API 服务器安装。设置说明请参阅 [XTTS](./XTTS.md) 页面。

### 复选框

- **已启用** - 开启/关闭 TTS 播放
- **自动生成** - 让 TTS 在新消息进入聊天时自动开始播放
- **仅朗读"引号"内容** - 将 TTS 播放限制为仅包含 `"引号"` 内的文本。这将`*包括星号行内的"引号"*`（内部变量名 = `narrate_quoted_only`）
- **忽略\*星号内的文本，即使是"引号"\*** - TTS 不会播放 `*星号*` 内的任何文本，即使是"引号"（内部变量名 = `narrate_dialogues_only`）
- *如果同时勾选"仅朗读引号"和"忽略星号"两个复选框，TTS 将只朗读不在星号内的"引号"，而忽略其他所有内容。*
- **仅朗读翻译后的文本** - 这将使 TTS 只朗读翻译后的文本。
- **应用正则表达式** - 在将文本发送到 TTS 提供商之前，对文本应用提供的正则表达式模式。用于从输入文本中删除不需要的部分，例如 TTS 引擎处理不好的表情符号或非本地语言字符。

以示例文本为例：`*Cohee approaches you with a faint "nya"* "Good evening, senpai", she says.`
下表显示了根据**忽略\*星号内的文本，即使是"引号"\***和**仅朗读"引号"内容**的布尔状态，文本将如何被修改：

| **忽略\*星号内的文本，即使是"引号"\*** 	 | **仅朗读"引号"内容**	 | **输出**                                                                |
|:-------------------------------------------------------|:---------------------------|:--------------------------------------------------------------------------| 
| 禁用                                               | 	禁用	                 | Cohee approaches you with a faint "nya" "Good evening, senpai", she says. |
| 禁用                                               | 启用	                   | "nya"... "Good evening, senpai"                                           |
| 启用	                                               | 禁用	                  | "Good evening, senpai", she says.                                         |
| 启用	                                               | 启用	                   | "Good evening, senpai"                                                    |

### 滑块

这些将根据您选择的 API 而有所不同。

### 按钮

- **应用** - 设置 TTS API 后以及编辑语音映射后，必须单击此按钮。
- **刷新** - 从所选 TTS API 重新加载语音列表。
- **可用语音** - 加载一个弹出窗口，显示所选 API 的所有可用语音，并让您通过示例对话预览它们。

## 使用 TTS

1. 单击"启用"复选框，否则什么都不会发生。
2. 如果您希望每次聊天中有新消息到达时 TTS 自动开始，请单击"自动生成"复选框。
3. 可选地，单击任意消息右上角内部的扬声器图标，按需播放。
4. 单击右下角的"停止"按钮（在魔杖菜单内）以停止任何播放。

### 语音映射

您必须提供语音映射，否则 TTS 将不知道每个角色应使用哪种语音。要设置语音映射，首先打开与您想要分配语音的角色的聊天，和/或选择要分配语音的用户人物设定，然后从 TTS 提供商的下拉列表中选择一个语音。如果您看不到语音和/或角色列表，请确保您的 TTS 提供商配置正确，然后单击"刷新"。部分提供商（如兼容 OpenAI 的或 NovelAI）需要您手动填充语音列表。
