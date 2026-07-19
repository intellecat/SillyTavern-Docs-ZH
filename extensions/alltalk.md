# AllTalk TTS V2

AllTalk 是一个基于 Coqui XTTS、F5-TTS、VITS、Piper 及其他 TTS 模型引擎的语音克隆系统，旨在生成高质量的语音复现（零样本语音克隆或内置语音）。AllTalk V2 带来了显著的功能更新和易用性改进，包括多 TTS 引擎支持、扩展的自定义选项以及性能优化。完整功能列表请参阅 [AllTalk Wiki](https://github.com/erew123/alltalk_tts/wiki)。

---

## 🟩 AllTalk V2 主要功能
- **多引擎支持**：可在 Coqui XTTS、VITS、Piper、Parler、F5 及自定义引擎之间轻松切换。
- **语音转换（RVC）**：增强的基于检索的语音克隆流水线。
- **可自定义设置**：调整各引擎设置并保存启动配置。
- **旁白功能**：为旁白和角色分别指定不同的声音。
- **独立及集成使用**：与 SillyTavern 无缝集成。
- **DeepSpeed 与低显存模式**：针对资源受限环境的性能优化。
- **截图**：在[此处](https://github.com/erew123/alltalk_tts/discussions/237)查看 AllTalk V2 的界面截图。

---

## 🟨 安装与配置选项

AllTalk 提供独立安装和集成安装两种方式。最快速的安装方式是使用提供的快速安装选项，脚本会自动处理大部分流程。

- **独立安装**：推荐大多数用户使用（[独立安装指南](https://github.com/erew123/alltalk_tts/wiki/Install-%E2%80%90-Standalone-Installation)）
- **Text-generation-webui 集成**：集成到 Text-generation-webui（[TGWUI 安装指南](https://github.com/erew123/alltalk_tts/wiki/Install-%E2%80%90-Text%E2%80%90generation%E2%80%90webui-Installation)）

#### 🟩 自动安装
**此方法仅适用于 Windows 用户。**
对于希望快速安装的新用户，自动安装使用 SillyTavern-Launcher。
注意：此方法假设你已经安装了 SillyTavern-Launcher。如果尚未安装，请访问 https://github.com/SillyTavern/SillyTavern-Launcher 并按照 readme.md 文件中的说明进行安装。
安装 SillyTavern-Launcher 后：
1. 运行 Launcher.bat
2. 进入：`Home > Toolbox > App Installer > Voice Generation`
3. 选择标有：**Install AllTalk V2** 的选项

#### 🟩 手动安装
对于需要详细控制的高级用户，请参阅[手动安装指南](https://github.com/erew123/alltalk_tts/wiki/Install-%E2%80%90-Manual-Installation-Guide)，在 Windows、Linux 或 Mac（未测试）上逐步完成安装。

#### 🟩 Google Colab 安装
通过 [Google Colab 安装](https://github.com/erew123/alltalk_tts/wiki/Google-COLAB)在云端环境运行 AllTalk，适合不希望本地安装的用户。

---

## 🟨 在 SillyTavern 中使用 AllTalk

AllTalk 加载完成后，在 SillyTavern 的 TTS 页面中选择它，并确保在设置中选择正确的 AllTalk 服务器版本。

- **设置管理**：AllTalk 可能会根据你选择的配置启用或禁用特定设置。
- **加载顺序**：如果 SillyTavern 在 AllTalk 之前加载，请重新加载 TTS 扩展页面。
- **性能优化**：根据系统资源有选择地启用 DeepSpeed 和低显存模式以提升性能。
- **旁白功能**：旁白功能的详细说明请参阅 [AllTalk Wiki](https://github.com/erew123/alltalk_tts/wiki/Narrator-Function)。

SillyTavern AllTalk 扩展的完整详情将在 [AllTalk Wiki 的 SillyTavern 页面](https://github.com/erew123/alltalk_tts/wiki/SillyTavern-Extension)持续更新。

使用 TGWUI AllTalk 扩展的 TGWUI 用户需要在 TGWUI 聊天界面中禁用 `Enable TGWUI TTS`，否则会生成重复的 TTS 音频。

---

## 🟨 故障排除

如果你遇到认为是 AllTalk 在 SillyTavern 中特有的问题，请参阅 [AllTalk Wiki 的 SillyTavern 页面](https://github.com/erew123/alltalk_tts/wiki/SillyTavern-Extension)获取最新信息。

---

### 🟪 支持、帮助与功能请求

如需进一步帮助：
- 参阅 [Wiki](https://github.com/erew123/alltalk_tts/wiki) 和内置文档。
- 在[讨论区](https://github.com/erew123/alltalk_tts/discussions/245)加入讨论。
- 通过[问题追踪器](https://github.com/erew123/alltalk_tts/issues)提交错误报告或功能请求。

---
