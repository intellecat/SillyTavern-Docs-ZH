---
route: /usage/api-connections/tabbyapi/
---

# TabbyAPI
一个基于 FastAPI 的应用程序，使用 Exllamav2 后端生成文本，支持 Exl2、GPTQ 和 FP16 模型。

* [GitHub](https://github.com/theroyallab/tabbyAPI)

### 快速开始
1. 按照官方 TabbyAPI GitHub 上的[安装说明](https://github.com/theroyallab/tabbyAPI/wiki/01.-Getting-Started)进行操作。
2. [创建您的 config.yml](https://github.com/theroyallab/tabbyAPI/wiki/02.-Server-options) 来设置您的模型路径、默认模型、序列长度等。如果您愿意，可以忽略大部分（如果不是全部）这些设置。
3. 启动 TabbyAPI。如果成功，您应该看到类似这样的内容：

    ![TabbyAPI 终端](/static/tabby-terminal.png)

4. 在 SillyTavern 的文本补全 API 下，选择 TabbyAPI。
5. 从 TabbyAPI 终端复制您的 API 密钥到"Tabby API key"，并确保您的"API URL"正确（默认应该是 `http://127.0.0.1:5000`）。

如果您正确完成了所有操作，您应该在 SillyTavern 中看到类似这样的内容：

![TabbyAPI SillyTavern](/static/tabby-config.png)

现在您可以使用 TabbyAPI 进行聊天了！

### TabbyAPI 加载器
TabbyAPI 的开发者创建了一个官方扩展，可以直接从 SillyTavern 加载/卸载模型。安装很简单：
1. 在 SillyTavern 中，点击扩展标签并导航到"下载扩展和资源"。
2. 将 `https://raw.githubusercontent.com/theroyallab/ST-repo/main/index.json` 复制到"资源 URL"中，并点击右侧的插头按钮。
3. 您应该看到类似这样的内容。点击 Tabby Loader 旁边的下载按钮。

    ![Tabby 加载器](/static/tabby-assets.png)

4. 如果安装成功，您应该在屏幕顶部看到一个绿色的弹出消息。在扩展标签下，导航到 TabbyAPI 加载器，并将您的管理员密钥从 TabbyAPI 终端复制到"管理员密钥"中。
5. 点击"模型选择"旁边的刷新按钮。当您点击它下面的文本框时，您应该看到模型目录中的所有模型。

![Tabby 加载器扩展](/static/tabby-loader.png)

现在您可以直接从 SillyTavern 加载和卸载您的模型了！

### 支持
还需要帮助？访问 [TabbyAPI GitHub](https://github.com/theroyallab/tabbyAPI) 获取开发者官方 Discord 服务器的链接，并[阅读 wiki](https://github.com/theroyallab/tabbyAPI/wiki/1.-Getting-Started)。
