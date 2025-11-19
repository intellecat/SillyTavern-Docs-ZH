---
route: /usage/api-connections/tabbyapi/
---

# TabbyAPI
一个基于 FastAPI 的应用程序，使用 Exllamav2 backend 生成文本，支持 Exl2、GPTQ 和 FP16 模型。

* [GitHub](https://github.com/theroyallab/tabbyAPI)

### Quickstart
1. 按照官方 TabbyAPI GitHub 上的[安装说明](https://github.com/theroyallab/tabbyAPI/wiki/01.-Getting-Started)进行操作。
2. [创建您的 config.yml](https://github.com/theroyallab/tabbyAPI/wiki/02.-Server-options) 来设置您的模型路径、默认模型、sequence length 等。如果您愿意，可以忽略大部分（如果不是全部）这些设置。
3. 启动 TabbyAPI。如果成功，您应该看到类似这样的内容：

    ![TabbyAPI terminal](/static/tabby-terminal.png)

4. 在 SillyTavern 的 Text Completion API 下，选择 TabbyAPI。
5. 从 TabbyAPI terminal 复制您的 API key 到 `Tabby API key`，并确保您的 `API URL` 正确（默认应该是 `http://127.0.0.1:5000`）。

如果您正确完成了所有操作，您应该在 SillyTavern 中看到类似这样的内容：

![TabbyAPI SillyTavern](/static/tabby-config.png)

现在您可以使用 TabbyAPI 进行聊天了！

### TabbyAPI Loader
TabbyAPI 的开发者创建了一个官方 extension，可以直接从 SillyTavern 加载/卸载模型。安装很简单：
1. 在 SillyTavern 中，点击 Extensions 标签并导航到 Download Extensions & Assets。
2. 将 `https://raw.githubusercontent.com/theroyallab/ST-repo/main/index.json` 复制到 Assets URL 中，并点击右侧的插头按钮。
3. 您应该看到类似这样的内容。点击 Tabby Loader 旁边的下载按钮。

    ![Tabby Loader](/static/tabby-assets.png)

4. 如果安装成功，您应该在屏幕顶部看到一个绿色的弹出消息。在 extensions 标签下，导航到 TabbyAPI Loader，并将您的 admin key 从 TabbyAPI terminal 复制到 Admin Key 中。
5. 点击 Model Select 旁边的刷新按钮。当您点击它下面的文本框时，您应该看到模型目录中的所有模型。

![Tabby Loader Extension](/static/tabby-loader.png)

现在您可以直接从 SillyTavern 加载和卸载您的模型了！

### Support
还需要帮助？访问 [TabbyAPI GitHub](https://github.com/theroyallab/tabbyAPI) 获取开发者官方 Discord server 的链接，并[阅读 wiki](https://github.com/theroyallab/tabbyAPI/wiki/1.-Getting-Started)。
