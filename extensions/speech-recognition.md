# Speech Recognition (语音识别)

本指南将帮助您在 SillyTavern 中设置语音识别以将您的语音转录为文本。

## 前提条件

在开始之前，请确保满足以下前提条件：

- 确保您使用的是最新版本的 SillyTavern。
- 从扩展面板（堆叠块图标）的"Download Extensions & Assets"菜单中安装"Speech Recognition"扩展。
- 已安装 ffmpeg 二进制文件。有关详细信息，请参阅 [RVC 设置](RVC.md#rvc-设置)。

## 语音识别设置（浏览器）

1. **配置 SillyTavern**：
   - 启动 SillyTavern 并转到 **Extensions** > **Speech Recognition**。
   - 从下拉选项中选择"Browser"。
   - 如果您的浏览器不支持语音识别，将出现错误弹窗。

2. **选择消息模式**：
   - 选择您想要的"Message Mode"：
     - **Append**：您的消息将附加到当前用户消息文本区域。
     - **Replace**：您的消息将替换文本区域中的当前用户消息。
     - **Auto send**：一旦检测到语音结束，您的消息将自动发送。

3. **启用消息映射**（可选）：
   - 设置语音快捷方式的短语映射。
   - 例如，通过添加"command delete = /del2"，当检测到"command delete"时，"/del2"命令将替换您的语音消息。
   - 与自动发送模式结合使用时很有用，可实现完全的语音控制。通过勾选"Enable messages mapping"启用此功能。

4. **选择语言**：
   - 选择您要使用的语言（注意：并非所有浏览器都支持所有语言）。

5. **录音**：
   - 要开始录音，点击消息区域右侧发送按钮旁边的麦克风按钮。再次点击停止录音。如果未检测到声音，录音可能会自动停止。

## 语音识别设置（Whisper/Vosk）

1. **启用提供商**：
   - 使用以下命令在 extras 服务器上启用所需的语音识别提供商：
     ```shell
     python server.py --enable-modules=whisper-stt
     ```
     或
     ```shell
     python server.py --enable-modules=vosk-stt
     ```
   - 您也可以通过添加选项 `--stt-vosk-model-path` 或 `--stt-whisper-model-path` 并指定模型路径来使用自定义模型。

2. **配置 SillyTavern**：
   - 启动 SillyTavern 并转到 **Extensions** > **Speech Recognition**。
   - 从下拉选项中选择"Vosk"或"Whisper"（whisper 更准确）。
   - 设置与"Browser"提供商设置类似（除了语言），请参见上文。

## 语音识别设置（流式传输）

1. **启用提供商**：
   - 使用以下命令在 Sillytavern-extras 上启用流式语音识别模块：
     ```shell
     python server.py --enable-modules=streaming-stt
     ```

2. **配置 SillyTavern**：
   - （可选）如上述 Whisper 设置中所述指定自定义 Whisper 模型。
   - （可选但推荐）在 SillyTavern 中设置触发词。只有以这些触发词开头的消息才会作为实际消息发送到 SillyTavern。这可以防止随机语音或噪音被转录。使用复选框启用此功能。可以使用复选框选择是否在实际消息中包含/排除触发词。
   - 其他设置与其他提供商类似。

现在您已准备好在 SillyTavern 中使用语音识别将您的语音转录为文本。
