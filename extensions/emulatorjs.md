# EmulatorJS

此扩展允许您直接在 SillyTavern 聊天中玩复古游戏机游戏。

## 安装

**前提条件：**

- SillyTavern 的最新发布版本。
- 从网上下载的 ROM 文件。您可以在[任何地方](https://archive.org/details/ni-romsets)找到它们。

**如何安装：**

1. 使用 SillyTavern 的扩展下载器安装。
2. 或使用此链接：`https://github.com/SillyTavern/SillyTavern-EmulatorJS`

## 使用方法

- 打开"EmulatorJS"扩展菜单。
- 点击"Add ROM file"。ROM 保存在您的浏览器存储中，不会存储在服务器上。
- 选择要添加的游戏文件。输入名称和核心（如果未自动检测）。如果核心需要 BIOS 文件，也要添加它。
- 点击列表中的"Play"按钮或通过魔杖菜单启动。
- 启动游戏后，您可以在模拟器框架中自定义控制和其他设置。
- 如果需要休息，请使用保存/加载状态功能。

查看 EmulatorJS 文档以了解可用核心及其要求的列表：[Systems](https://emulatorjs.org/docs/systems)。

## 评论模式

借助 GPT-4 Vision 等多模态模型的力量，您的 AI 机器人可以看到您的游戏画面并提供有趣的角色评论。

### 要求

1. 支持 [ImageCapture](https://developer.mozilla.org/en-US/docs/Web/API/ImageCapture#browser_compatibility) 的浏览器。在桌面版 Chrome 上测试通过。Firefox 需要通过配置启用。Safari 无法工作。
2. 推荐使用带有图像内联模式的 Chat Completion API。需要 OpenAI 或 OpenRouter API 密钥，选择"gpt-4-turbo"或"gpt-4o"作为模型；Google AI Studio 使用 Gemini 1.5 Pro 或 Gemini 1.5 Flash 模型；Anthropic Claude（推荐 Opus 3 或 Sonnet 3.5 模型）。查看所选 API 的文档，以了解所选模型是否支持多模态提示。
3. 如果禁用了图像内联，请确保启用了"Image Captioning"扩展，然后选择"Multimodal"描述源：
  - OpenAI、Claude、MistralAI、Google AI Studio 使用任何支持视觉的模型。
  - OpenRouter API 使用兼容的多模态模型。
  - 在 Ollama、KoboldCpp、oobabooga TextGen WebUI 或 vLLM 中本地托管的 Llava 模型。

### 如何启用评论

1. 确保在 EmulatorJS 扩展设置中设置了提供评论的间隔。此设置定义了使用当前游戏画面快照查询角色评论的频率。值为 0 表示不提供评论。
2. 选择角色聊天并启动游戏。为了获得最佳性能，请确保 ROM 文件正确命名，以便 AI 可以获得更多背景上下文。
3. 像平常一样开始玩游戏。视觉模型将定期查询，根据它"看到"的最新截图写评论。

### 设置

1. Caption template - 用于描述游戏截图的提示。支持 \{\{game\}\} 和 \{\{core\}\} 额外宏。
2. Comment template - 用于根据生成的描述写评论的提示。支持 \{\{game\}\}、\{\{core\}\}、\{\{caption\}\} 额外宏。对于图像内联模式，\{\{caption\}\} 将替换为 `see included image`。
3. Force captions - 即使支持并启用了图像内联，也会强制使用多模态描述。

### 为什么我看不到任何评论？

在以下情况下，评论会暂时暂停（跳过间隔步骤）：

1. 模拟器已暂停（使用暂停按钮，而不是游戏内暂停）。
2. 浏览器窗口失去焦点。
3. 用户输入区域不为空。这是为了让您安静地输入回复。
4. 另一个回复生成当前正在进行中。
5. TTS 语音正在朗读。评论会暂停（最多 20 秒）直到语音结束，但不会跳过。

其他常见问题：

1. 确保在启动游戏之前设置了评论间隔。
2. 确保您已设置多模态 API 密钥，并且 ST 服务器控制台中没有错误。

仍然不工作？请发送您的浏览器调试控制台日志（按 F12）。

## 致谢

- EmulatorJS 引擎 (GPLv3)：https://github.com/EmulatorJS/EmulatorJS
