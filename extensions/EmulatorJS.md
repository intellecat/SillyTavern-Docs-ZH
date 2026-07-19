---
route: /extensions/emulatorjs/
templating: false
---

# EmulatorJS

此扩展允许你直接在 SillyTavern 聊天界面中玩复古主机游戏。

## 安装

**前提条件：**

- 最新发布版本的 SillyTavern。
- 从网上下载的 ROM 文件。你可以在[任何地方](https://archive.org/details/ni-romsets)找到它们。

**安装方法：**

1. 使用 SillyTavern 的扩展下载器安装。
2. 或使用此链接：`https://github.com/SillyTavern/SillyTavern-EmulatorJS`

## 使用方法

- 打开"EmulatorJS"扩展菜单。
- 点击"添加 ROM 文件"。ROM 保存在浏览器存储中，不存储在服务器上。
- 选择要添加的游戏文件。输入名称和核心（如果未自动检测到）。如果核心需要 BIOS 文件，也请一并添加。
- 点击列表中的"播放"按钮或通过魔杖菜单启动。
- 在启动游戏后，可以在模拟器框架中自定义控制方式和其他设置。
- 如果需要休息，可以使用存档/读档功能。

查看 EmulatorJS 文档以了解可用核心及其要求：[核心列表](https://emulatorjs.org/docs4devs/cores)。

## 实况解说模式

借助多模态模型的能力，你的 AI 机器人可以"看到"你的游戏画面并给出符合角色风格的即时评论。

### 要求

1. 支持 [ImageCapture](https://developer.mozilla.org/en-US/docs/Web/API/ImageCapture#browser_compatibility) 的浏览器。已在桌面版 Chrome 上测试。Firefox 需要通过配置启用。Safari 不支持。
2. 推荐使用带有图像内联模式的 Chat Completion API。请查阅 API 文档以确认所选模型是否支持多模态提示词。
3. 如果禁用了图像内联，请确保已启用[图像描述](./captioning.md#multimodal-source)扩展，然后选择"多模态"描述来源。

### 如何启用解说

1. 确保已在 EmulatorJS 扩展设置中设置提供解说的间隔时间。此设置定义了多久向角色查询一次以当前游戏截图为基础的评论。值为 0 表示不提供评论。
2. 选择一个角色聊天并启动游戏。为了获得最佳效果，请确保 ROM 文件命名规范，以便 AI 能获得更多背景信息。
3. 正常开始游戏。视觉模型将定期根据最新截图生成评论。

### 设置

1. 描述模板——用于描述游戏内截图的提示词，支持 `{{game}}` 和 `{{core}}` 附加宏。
2. 评论模板——用于根据生成的描述撰写评论的提示词，支持 `{{game}}`、`{{core}}`、`{{caption}}` 附加宏。在图像内联模式下，`{{caption}}` 会替换为 `see included image`。
3. 强制使用描述——即使支持并启用了图像内联，也强制使用多模态图像描述。

### 为什么我看不到任何评论？

在以下情况下，评论会暂时暂停（跳过间隔步骤）：

1. 模拟器已暂停（使用暂停按钮，而非游戏内暂停）。
2. 浏览器窗口失去焦点。
3. 用户输入区域不为空。这是为了让你安静地输入回复。
4. 另一个回复生成正在进行中。
5. TTS 语音正在朗读。评论会等待（最多 30 秒）直到朗读结束，但不会跳过。
6. 角色卡或群组当前处于打开状态。从欢迎界面启动游戏时，评论模式会被禁用。

其他常见问题：

1. 确保在启动游戏之前已设置评论间隔。
2. 确保已设置多模态 API 密钥，且 ST 服务器控制台中没有报错。

仍然无法正常工作？请发送你的浏览器调试控制台日志（按 F12）。

## 致谢

- EmulatorJS 引擎（GPLv3）：<https://github.com/EmulatorJS/EmulatorJS>
