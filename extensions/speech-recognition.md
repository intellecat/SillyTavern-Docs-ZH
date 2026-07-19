# Speech Recognition（语音识别）

本指南将引导你在 SillyTavern 中设置语音识别，将语音转录为文本。

## 前提条件

开始之前，请确保满足以下前提条件：

- 确保你使用的是最新版本的 SillyTavern。
- 从扩展面板（堆叠方块图标）的"下载扩展和资源"菜单中安装"Speech Recognition"扩展。

## 语音识别设置（浏览器）

1. **配置 SillyTavern**：
   - 启动 SillyTavern，进入 **扩展** > **语音识别**。
   - 从下拉选项中选择"Browser"。
   - 如果你的浏览器不支持语音识别，将弹出错误提示。

2. **选择消息模式**：
   - 选择你想要的"消息模式"：
     - **追加（Append）**：你的消息将追加到当前用户消息文本框中。
     - **替换（Replace）**：你的消息将替换文本框中当前的用户消息。
     - **自动发送（Auto send）**：检测到语音结束后，你的消息将自动发送。

3. **启用消息映射**（可选）：
   - 设置语音快捷方式的短语映射。
   - 例如，添加"command delete = /del2"后，当检测到"command delete"时，将用"/del2"命令替换你的语音消息。
   - 与自动发送模式结合使用，可实现完整的语音控制。通过勾选"启用消息映射"来启用此功能。

4. **选择语言**：
   - 选择你想使用的语言（注意：并非所有浏览器都支持所有语言）。

5. **录音**：
   - 要开始录音，请点击消息区域右侧（发送按钮旁）的麦克风按钮。再次点击以停止录音。如果未检测到语音，录音可能会自动停止。

## 语音识别设置（API 来源）

支持 OpenAI、MistralAI、Groq、Chutes、Z.AI 等提供语音转文本 API 的来源。

设置步骤：

1. 在 Chat Completion API 设置中，为所选提供商填写 API 密钥。
2. 启动 SillyTavern，进入 **扩展** > **语音识别**。
3. 从下拉选项中选择所需的 API 来源。
4. 根据需要配置其他设置，与"浏览器"提供商设置类似。

## 语音识别设置（Extras）- 已废弃

!!!
需要安装 ffmpeg 二进制文件。详见 [RVC 设置](RVC.md#rvc-setup)。
!!!

1. **启用提供商**：
   - 使用以下命令在 extras 服务器上启用所需的语音识别提供商：
     ```shell
     python server.py --enable-modules=whisper-stt
     ```
     或
     ```shell
     python server.py --enable-modules=vosk-stt
     ```
   - 你也可以通过添加选项 `--stt-vosk-model-path` 或 `--stt-whisper-model-path` 并指定模型路径来使用自定义模型。

2. **配置 SillyTavern**：
   - 启动 SillyTavern，进入 **扩展** > **语音识别**。
   - 从下拉选项中选择"Vosk"或"Whisper"（Whisper 更准确）。
   - 设置与"浏览器"提供商设置类似（语言除外），详见上文。

## 语音识别设置（流式传输）- 已废弃

!!!
需要安装 ffmpeg 二进制文件。详见 [RVC 设置](RVC.md#rvc-setup)。
!!!

1. **启用提供商**：
   - 使用以下命令在 SillyTavern-extras 上启用流式语音识别模块：
     ```shell
     python server.py --enable-modules=streaming-stt
     ```

2. **配置 SillyTavern**：
   - （可选）按照上述 Whisper 设置方式指定自定义 Whisper 模型。
   - （可选但推荐）在 SillyTavern 中设置触发词。只有以这些触发词开头的消息才会作为实际消息发送给 SillyTavern，这可以防止随机语音或噪音被转录。通过复选框启用此功能。可以使用复选框选择是否将触发词包含在实际消息中。
   - 其他设置与其他提供商类似。

你现在已准备好在 SillyTavern 中使用语音识别将语音转录为文本。
