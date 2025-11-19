---
order: tts-alltalk
route: /extensions/alltalk/
---
# AllTalk TTS V2

AllTalk 是一个基于 Coqui XTTS、F5-TTS、VITS、Piper 和其他 TTS 模型引擎的语音克隆系统,旨在产生高质量的语音再现(零样本语音克隆或内置语音)。在 AllTalk V2 中,重大更新增强了功能和易用性,包括多个 TTS 引擎支持、扩展的自定义和性能优化。有关功能的完整列表,请参阅 [AllTalk Wiki](https://github.com/erew123/alltalk_tts/wiki)。

---

## 🟩 AllTalk V2 中的主要功能
- **多引擎支持**: 轻松在 Coqui XTTS、VITS、Piper、Parler、F5 和自定义引擎之间切换。
- **语音转换(RVC)**: 增强的基于检索的语音克隆管道。
- **可自定义设置**: 调整每个引擎的设置并保存启动配置。
- **叙述者功能**: 为叙述和角色指定单独的语音。
- **独立和集成使用**: 与 SillyTavern 无缝集成。
- **DeepSpeed 和低 VRAM 模式**: 针对资源有限环境的性能优化。
- **屏幕截图**: 在[此处](https://github.com/erew123/alltalk_tts/discussions/237)查看 AllTalk V2 的界面。

---

## 🟨 设置和安装选项

AllTalk 提供独立和集成安装方法。最快的设置涉及使用提供的快速安装选项之一,脚本会自动执行大部分过程。

- **独立安装**: 推荐给大多数用户([独立指南](https://github.com/erew123/alltalk_tts/wiki/Install-%E2%80%90-Standalone-Installation))
- **Text-generation-webui 集成**: 用于集成到 Text-generation-webui([TGWUI 安装指南](https://github.com/erew123/alltalk_tts/wiki/Install-%E2%80%90-Text%E2%80%90generation%E2%80%90webui-Installation))

#### 🟩 自动安装
**此方法仅适用于 Windows 用户。**
对于想要快速设置的新用户,自动安装使用 SillyTavern-Launcher。
注意:这假设您已经安装了 SillyTavern-Launcher。如果没有,请访问 https://github.com/SillyTavern/SillyTavern-Launcher 并按照 readme.md 文件中的说明进行安装。
安装 SillyTavern-Launcher 后:
1. 运行 Launcher.bat
2. 转到: `Home > Toolbox > App Installer > Voice Generation`
3. 选择标有以下内容的选项: **Install AllTalk V2**

#### 🟩 手动安装
对于需要详细控制的高级用户,请遵循 [手动安装指南](https://github.com/erew123/alltalk_tts/wiki/Install-%E2%80%90-Manual-Installation-Guide),以获取 Windows、Linux 或 Mac(未测试)上的分步设置。

#### 🟩 Google Colab 安装
使用 [Google Colab 安装](https://github.com/erew123/alltalk_tts/wiki/Google-COLAB)在云环境中运行 AllTalk,适用于不想在本地安装的用户。

---

## 🟨 在 SillyTavern 中使用 AllTalk

加载 AllTalk 后,在 SillyTavern 的 TTS 页面上选择它,确保在设置中选择正确的 AllTalk 服务器版本。

- **设置管理**: AllTalk 可能会根据您选择的配置启用或禁用特定设置。
- **加载顺序**: 如果在 AllTalk 之前加载 SillyTavern,请重新加载 TTS 扩展页面。
- **性能优化**: 根据系统资源有选择地启用 DeepSpeed 和低 VRAM 模式以提高性能。
- **叙述者功能**: 叙述者功能的详细信息可以在 [AllTalk Wiki](https://github.com/erew123/alltalk_tts/wiki/Narrator-Function) 上找到。

SillyTavern AllTalk 扩展的完整详细信息将在 [AllTalk Wiki 页面的 SillyTavern](https://github.com/erew123/alltalk_tts/wiki/SillyTavern-Extension) 上更新

使用 TGWUI AllTalk 扩展的 TGWUI 用户需要在 TGWUI 聊天界面中禁用 `Enable TGWUI TTS`,否则您将生成重复的 TTS 音频。

---

## 🟨 故障排除

如果您遇到您认为是 SillyTavern 中 AllTalk 特有的问题,请参阅 [AllTalk Wiki 页面的 SillyTavern](https://github.com/erew123/alltalk_tts/wiki/SillyTavern-Extension) 以获取最新信息。

---

### 🟪 支持、帮助和功能请求

如需进一步帮助:
- 参考 [Wiki](https://github.com/erew123/alltalk_tts/wiki) 和内置文档。
- 在[讨论板](https://github.com/erew123/alltalk_tts/discussions/245)上加入讨论。
- 通过[问题跟踪器](https://github.com/erew123/alltalk_tts/issues)提交错误或功能请求。

---
