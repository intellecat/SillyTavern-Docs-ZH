---
route: /extensions/emulatorjs/
templating: false
---

# EmulatorJS

此扩展允许您直接在 SillyTavern 聊天中玩复古游戏机游戏。

## 安装

**前提条件:**

- 最新发布版本的 SillyTavern。
- 从网上下载的 ROM 文件。您可以在[任何地方](https://archive.org/details/ni-romsets)找到它们。

**如何安装:**

1. 使用 SillyTavern 的扩展下载器安装。
2. 或使用此链接:`https://github.com/SillyTavern/SillyTavern-EmulatorJS`

## 使用方法

- 打开"EmulatorJS"扩展菜单。
- 点击"添加 ROM 文件"。ROM 保存到您的浏览器存储中,不存储在服务器上。
- 选择要添加的游戏文件。输入名称和核心(如果未自动检测)。如果核心需要 BIOS 文件,也要添加它。
- 点击列表中的"播放"按钮或通过魔杖菜单启动。
- 您可以在启动游戏后在模拟器框架中自定义控制和其他设置。
- 如果需要休息,请使用保存/加载状态功能。

查看 EmulatorJS 文档以查看可用核心及其要求列表:[核心](https://emulatorjs.org/docs4devs/cores)。

## 评论模式

借助多模态模型的强大功能,您的 AI 机器人可以看到您的游戏玩法并提供机智的角色内评论。

### 要求

1. 支持 [ImageCapture](https://developer.mozilla.org/en-US/docs/Web/API/ImageCapture#browser_compatibility) 的浏览器。已在桌面 Chrome 上测试。Firefox 需要通过配置启用它。Safari 无法工作。
2. 建议使用带有图像内联模式的聊天完成 API。查看 API 文档以了解所选模型是否支持多模态提示。
3. 如果禁用图像内联,请确保启用[图像描述](./captioning.md#multimodal-source)扩展,然后选择"多模态"描述源。

### 如何启用评论

1. 确保在 EmulatorJS 扩展设置中设置提供评论的间隔。此设置定义使用当前游戏玩法的图像查询角色以获取评论的频率。值为 0 表示不提供评论。
2. 选择一个角色聊天并启动游戏。为获得最佳性能,请确保正确命名 ROM 文件,以便 AI 可以获得更多背景上下文。
3. 像平常一样开始玩游戏。将定期查询视觉模型以根据它"看到"的最新截图编写评论。

### 设置

1. 描述模板 - 用于描述游戏内截图的提示。支持 `{{game}}` 和 `{{core}}` 附加宏。
2. 评论模板 - 用于根据生成的描述编写评论的提示。支持 `{{game}}`、`{{core}}`、`{{caption}}` 附加宏。对于图像内联模式,`{{caption}}` 替换为 `see included image`。
3. 强制描述 - 即使支持并启用图像内联,也将强制使用多模态描述。

### 为什么我看不到任何评论?

在以下情况下,评论会暂时暂停(跳过间隔步骤):

1. 模拟器已暂停(使用暂停按钮,而非游戏内暂停)。
2. 浏览器窗口失去焦点。
3. 用户输入区域不为空。这是为了让您平静地输入回复。
4. 另一个回复生成当前正在进行中。
5. TTS 语音正在朗读。评论将推迟(最多 30 秒)直到完成,但不会跳过。
6. 角色卡或群组当前已打开。从欢迎屏幕启动游戏时,评论模式被禁用。

其他常见问题:

1. 确保在启动游戏之前设置了评论间隔。
2. 确保您已设置多模态 API 密钥,并且 ST 服务器控制台中没有错误。

仍然不起作用?向我们发送您的浏览器调试控制台日志(按 F12)。

## 致谢

- EmulatorJS 引擎 (GPLv3): <https://github.com/EmulatorJS/EmulatorJS>
