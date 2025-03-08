# TTS（文本转语音）

SillyTavern 有多种 TTS 选项。本页面将解释其设置和使用方法。

### 这是什么？

TTS 用于为聊天的部分内容添加语音叙述。

### 配置 TTS

#### TTS 提供商选择框

用于选择您想使用的 TTS 服务。

- **ElevenLabs** - 需要付费订阅，目前提供最高质量的语音。
- **Silero** - 免费，在您的电脑上运行，质量可能差异很大。
- **System** - 使用您操作系统的 TTS 引擎（如果存在）。质量可能因操作系统而异。
- **Edge** - 免费，通过 Azure 运行，通常速度很快，语音自然但缺乏情感。像听晚间新闻或电台播音员。当选择"Plugin"作为提供商时，您还需要安装[此服务器插件](https://github.com/SillyTavern/SillyTavern-EdgeTTS-Plugin)，否则 TTS 将无法工作。
- **Coqui-TTS** - 免费，目前没有 API 实现。高性能文本转语音模型（Tacotron、Tacotron2、Glow-TTS、SpeedySpeech）以及 Bark。
- **Novel** - 需要 NovelAI 付费订阅，由 NovelAI 的 TTS 引擎生成。
- **RVC** - 免费，语音克隆。

#### 复选框

- **启用** - 开启/关闭 TTS 播放
- **自动生成** - 允许 TTS 在新消息进入聊天时自动开始播放
- **仅叙述"引号内容"** - 将 TTS 播放限制为仅包含 `"引号"`内的文本。这将包括`*星号内的"引号"*`（内部变量名 = `narrate_quoted_only`）
- **忽略\*星号内的文本，即使是"引号"内容\*** - TTS 不会播放任何 `*星号*`内的文本，即使是"引号"内容（内部变量名 = `narrate_dialogues_only`）
- *同时勾选"仅叙述引号内容"和"忽略星号"复选框将导致 TTS 只读取不在星号内的"引号"内容，并忽略其他所有内容。*
- **仅叙述翻译后的文本** - 这将使 TTS 只叙述翻译后的文本。

给定示例文本：`*Cohee approaches you with a faint "nya"* "Good evening, senpai", she says.`
下表显示了基于**忽略\*星号内的文本，即使是"引号"内容\***和**仅叙述"引号内容"**的布尔状态，文本将如何修改：

| **忽略\*星号内的文本，即使是"引号"内容\*** | **仅叙述"引号内容"** | **输出**                                                                |
|:-------------------------------------------------------|:---------------------------|:--------------------------------------------------------------------------| 
| 禁用                                               | 禁用                    | Cohee approaches you with a faint "nya" "Good evening, senpai", she says. |
| 禁用                                               | 启用                    | "nya"... "Good evening, senpai"                                           |
| 启用                                               | 禁用                    | "Good evening, senpai", she says.                                         |
| 启用                                               | 启用                    | "Good evening, senpai"                                                    |

#### 滑块

这些将根据您选择的 API 而改变。

（解释即将推出）

#### 按钮

- **应用** - 设置 TTS API 后和编辑语音映射后必须点击此按钮。
- **可用语音** - 加载一个弹出窗口，显示所选 API 的所有可用语音，并让您通过示例对话预览它们。

### 使用 TTS

1. 点击"启用"复选框，否则什么都不会发生。
2. 如果您希望每次聊天中出现新消息时 TTS 自动开始，请点击"自动生成"复选框。
3. 可选择点击任何消息右上角的扬声器图标，按需播放。
4. 点击右下角的"停止"按钮（在魔杖菜单内）以停止任何播放。

#### 语音映射

您必须为 TTS 提供语音映射，否则它不知道应该为每个角色使用什么语音。

这些必须按照以下格式精确设置：

`角色名:TTS语音,角色名2:TTS语音2`

对于 Coqui-TTS，格式需要包含来自 WebGUI 的说话者和语言：

`角色名:TTS语音[说话者id][语言id]`
或
`Aqua:tts_models--multilingual--multi-dataset--your_tts\model_file.pth[2][1]`

#### Bark 零样本语音克隆说话者

如果使用 Bark，您必须创建一个带有要克隆的语音文件的语音文件夹。确保将语音添加到 homedir\tts\bark_v0\speakers\。在 Windows 上，可能是 C:\Users\用户账户\AppData\Local\tts\bark_v0\speakers\，在 Windows 资源管理器中输入 %appdata% 然后向上一级目录到 local，您应该能看到 tts。

目录结构应如下所示：
- homedir
  - tts
    - bark_v0
      - speakers
        - customvoice1
          - speaker.wav
          - speaker.npz
        - robinwilliams
          - speaker.mp3
        - me
          - speaker.mp3

首次加载此模型和语音时，bark 将克隆语音并创建一个 .npz 文件，这是为了更快的 TTS 所需的。
