---
order: tts-alltalk
route: /extensions/alltalk/
---
---

## 🟩 Key Features in AllTalk V2
- **Multi-engine Support**: Easily switch between Coqui XTTS, VITS, Piper, Parler, F5, and custom engines.
- **Voice Conversion (RVC)**: Enhanced retrieval-based voice cloning pipeline.
- **Customizable Settings**: Adjust per-engine settings and save startup configurations.
- **Narrator Functionality**: Specify separate voices for narration and characters.
- **Standalone and Integrated Use**: Seamless integration with SillyTavern.
- **DeepSpeed and Low VRAM Modes**: Performance optimization for resource-limited environments.
- **Screenshots**: See AllTalk V2’s interface [here](https://github.com/erew123/alltalk_tts/discussions/237).

---
---

## 🟨 Using AllTalk within SillyTavern

Once AllTalk is loaded, select it within SillyTavern on the TTS page, ensuring to select the correct AllTalk server version in the settings.

- **Settings Management**: AllTalk may enable or disable specific settings based on your selected configuration.
- **Loading Sequence**: If SillyTavern is loaded before AllTalk, reload the TTS extensions page.
- **Performance Optimization**: Enable DeepSpeed and Low VRAM modes selectively to improve performance based on system resources.
- **Narrator function**: Details of the Narrator function can be found on the [AllTalk Wiki](https://github.com/erew123/alltalk_tts/wiki/Narrator-Function).

Full details of the SillyTavern AllTalk Extension will be updated on the [AllTalk Wiki page for SillyTavern](https://github.com/erew123/alltalk_tts/wiki/SillyTavern-Extension)

TGWUI Users who user the AllTalk extension for TGWUI need to disable `Enable TGWUI TTS` in the TGWUI chat interface, otherwise you will have duplicate TTS audio generated.

---
---

### 🟪 Support, Assistance, and Feature Requests

For further assistance:
- Refer to the [Wiki](https://github.com/erew123/alltalk_tts/wiki) and built-in documentation.
- Join discussions on the [Discussion Board](https://github.com/erew123/alltalk_tts/discussions/245).
- Submit bugs or feature requests through the [Issue Tracker](https://github.com/erew123/alltalk_tts/issues).
---

# AllTalk TTS V2

AllTalk 是一个基于 Coqui XTTS、F5-TTS、VITS、Piper 和其他 TTS 模型引擎的语音克隆系统，旨在产生高质量的语音复制（无论是零样本语音克隆还是内置语音）。在 AllTalk V2 中，重大更新增强了功能性和易用性，包括多个 TTS 引擎支持、扩展的自定义选项和性能优化。有关功能的完整列表，请参阅 [AllTalk Wiki](https://github.com/erew123/alltalk_tts/wiki)。

## 🟩 AllTalk V2 的主要功能
- **多引擎支持**：轻松在 Coqui XTTS、VITS、Piper、Parler、F5 和自定义引擎之间切换。
- **语音转换 (RVC)**：增强的基于检索的语音克隆管道。
- **可自定义设置**：调整每个引擎的设置并保存启动配置。
- **叙述功能**：为叙述和角色指定不同的语音。
- **独立和集成使用**：与 SillyTavern 无缝集成。
- **DeepSpeed 和低 VRAM 模式**：针对资源受限环境的性能优化。
- **截图**：在[此处](https://github.com/erew123/alltalk_tts/discussions/237)查看 AllTalk V2 的界面。

## 🟨 设置和安装选项

AllTalk 提供独立和集成的安装方法。最快的设置方法是使用提供的快速安装选项之一，这些脚本可以自动完成大部分过程。

- **独立安装**：推荐大多数用户使用（[独立安装指南](https://github.com/erew123/alltalk_tts/wiki/Install-%E2%80%90-Standalone-Installation)）
- **Text-generation-webui 集成**：用于集成到 Text-generation-webui（[TGWUI 安装指南](https://github.com/erew123/alltalk_tts/wiki/Install-%E2%80%90-Text%E2%80%90generation%E2%80%90webui-Installation)）

#### 🟩 手动安装
对于需要详细控制的高级用户，请按照[手动安装指南](https://github.com/erew123/alltalk_tts/wiki/Install-%E2%80%90-Manual-Installation-Guide)在 Windows、Linux 或 Mac（未测试）上进行逐步设置。

#### 🟩 Google Colab 安装
使用 [Google Colab 安装](https://github.com/erew123/alltalk_tts/wiki/Google-COLAB)在云环境中运行 AllTalk，适用于不想在本地安装的用户。

## 🟨 在 SillyTavern 中使用 AllTalk

一旦 AllTalk 加载完成，在 TTS 页面中选择它，确保在设置中选择正确的 AllTalk 服务器版本。

- **设置管理**：AllTalk 可能会根据您选择的配置启用或禁用特定设置。
- **加载顺序**：如果在 AllTalk 之前加载了 SillyTavern，请重新加载 TTS 扩展页面。
- **性能优化**：根据系统资源有选择地启用 DeepSpeed 和低 VRAM 模式以提高性能。
- **叙述功能**：叙述功能的详细信息可以在 [AllTalk Wiki](https://github.com/erew123/alltalk_tts/wiki/Narrator-Function) 中找到。

SillyTavern AllTalk 扩展的完整详细信息将在 [AllTalk Wiki 的 SillyTavern 页面](https://github.com/erew123/alltalk_tts/wiki/SillyTavern-Extension)上更新。

使用 TGWUI 的 AllTalk 扩展的 TGWUI 用户需要在 TGWUI 聊天界面中禁用 `Enable TGWUI TTS`，否则会生成重复的 TTS 音频。

## 🟨 故障排除

如果您遇到您认为是 SillyTavern 中 AllTalk 特有的问题，请参阅 [AllTalk Wiki 的 SillyTavern 页面](https://github.com/erew123/alltalk_tts/wiki/SillyTavern-Extension)获取最新信息。

### 🟪 支持、帮助和功能请求

如需进一步帮助：
- 参考 [Wiki](https://github.com/erew123/alltalk_tts/wiki) 和内置文档。
- 在[讨论板](https://github.com/erew123/alltalk_tts/discussions/245)上参与讨论。
- 通过[问题追踪器](https://github.com/erew123/alltalk_tts/issues)提交错误或功能请求。
