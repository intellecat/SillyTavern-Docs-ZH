---
order: 150
icon: repo-forked
expanded: false
route: /usage/api-connections
---

# API 连接

SillyTavern 可以连接到多种 LLM API。
以下是对它们各自的优势、劣势和使用场景的描述。

## 简单解释：聊天补全与文本补全的区别

当您第一次进入 ST 的"API 连接"页面时，您会注意到有一个下拉选项，可以在"聊天补全"和"文本补全"等选项之间选择。了解这些概念很有帮助。

它不是：很容易把"文本补全"理解为本地模型，把"聊天补全"理解为基于云的 LLM，但事实并非如此。同样，例如"Novel AI"或"Kobold"实际上也不是完全不同类型的模型，尽管它们在 ST 的 API 下拉菜单中是单独的选项。您可以通过适当的后端强制模型使用不同的 API 结构，但这不是本节的重点。

当您使用 ST 发送消息时，您的聊天、角色描述和其他提示词（如知识库或作者注释）会被构建成一个单一的"提示词"发送给模型。您使用的模型的 API"类型"决定了这个提示词将如何构建（ST 在后台会自动为您处理这些 - 您可以打开 ST 终端，看到发送给 AI 的提示词的确切内容）。

### 聊天补全

顾名思义，聊天补全模型会将您的提示词构建成用户（您）和助手（AI）或系统（中立）之间的一系列消息。为聊天补全训练的模型有助于营造"聊天"的感觉，让 AI"回应"最后一条消息。当您使用 ChatGPT 网站时，您在后台使用的就是聊天补全 API。

### 文本补全（又称"补全"）

另一方面，文本补全（同样顾名思义）会将您的提示词转换为一个长字符串，模型只是试图继续这个字符串（就像，字面上想象您所有的文本、数百条消息、所有格式、换行符等都被压缩成一个很长的句子）。

如果您在 ST 中的消息恰好被格式化为 YourPersona: 和 Character: 之间的一系列消息，文本补全模型会尝试继续这个模式，ST 会将其呈现为新的聊天消息，但实际上模型只是在尝试继续文本。如果您输入"太阳从东方"，文本补全模型很可能会用"升起"来完成这条消息。

大多数文本补全模型都有推荐的"指令模板"（通常在模型的文档或下载页面中提到），这些模板帮助它们像聊天补全模型一样"回应"消息和指令。ST 通常在"高级格式设置"页面中提供了大多数（如果不是全部）指令模板供您选择。

## 本地 API

- 这些 LLM API 可以在您的电脑上运行。
- 它们免费使用且没有内容过滤。
- 安装过程可能比较复杂（**SillyTavern 开发团队不提供此方面的支持**）。
- 需要从 [HuggingFace](https://huggingface.co/models?other=LLM) 单独下载 LLM 模型，每个模型可能有 5-50GB。
- 大多数模型不如云端 LLM API 强大。

### KoboldAI

- 在您的电脑上运行，100% 私密，可使用多种模型
- 提供对 AI 生成设置的最直接控制
- 需要 GPU 中大量的 VRAM（6-24GB，取决于 LLM 模型）
- 模型限制在 2k 上下文
- 不支持流式传输
- 流行的 KoboldAI 版本：
  - [Henky's United](https://github.com/henk717/KoboldAI)
  - [0cc4m's 4bit-supporting United](https://github.com/0cc4m/KoboldAI)

### KoboldCpp

- 易于使用的 API，支持 CPU 卸载（对低 VRAM 用户有帮助）和流式传输
- 在 Windows 上可以从单个 .exe 文件运行（在 MacOS 和 Linux 上必须从源代码编译）
- 支持 GGUF/GGML 模型
- 比仅 GPU 加载器（如 AutoGPTQ 和 Exllama/v2）慢
- [GitHub](https://github.com/LostRuins/koboldcpp)

### Oobabooga TextGeneration WebUI

- 带有流式传输的一体化 Gradio UI
- 对量化模型（AWQ、Exl2、GGML、GGUF、GPTQ）和 FP16 模型的最广泛支持
- 提供一键安装程序
- 定期更新，有时可能会破坏与 SillyTavern 的兼容性
- [GitHub](https://github.com/oobabooga/text-generation-webui#one-click-installers)

**正确连接 SillyTavern 到 Ooba 新 OpenAI API 的方法**

1. 确保您使用的是 Oobabooga's TextGen 的最新更新（截至 2023 年 11 月 14 日）。
2. 编辑 CMD_FLAGS.txt 文件，在其中添加 `--api` 标志。然后重启 Ooba 的服务器。
3. 将 ST 连接到 <http://localhost:5000/>（默认），不要勾选"Legacy API"框。您可以删除 Ooba 控制台提供的 URL 中的 `/v1` 后缀。

*您可以使用 `--api-port 5001` 标志更改 API 托管端口，其中 5001 是您的自定义端口。*

### TabbyAPI

- 基于 [Exllamav2](https://github.com/turboderp/exllamav2) 的轻量级 API，支持流式传输
- 支持 Exl2、GPTQ 和 FP16 模型
- [官方扩展](https://github.com/theroyallab/ST-tabbyAPI-loader) 允许直接从 SillyTavern 加载/卸载模型
- 不推荐给低 VRAM 用户（无 CPU 卸载）
- [GitHub](https://github.com/theroyallab/tabbyAPI)

## 云端 LLM API

- 这些 LLM API 作为云服务运行，不需要您电脑的资源
- 它们比大多数本地 LLM 更强大/更智能
- 但它们都有不同程度的内容过滤，大多数需要付费

### OpenAI (ChatGPT)

- 易于设置和获取 API 密钥
- 需要预付费购买积分，按提示词收费
- 非常逻辑化。创意风格可能重复和可预测
- 大多数较新的模型（gpt-4-turbo、gpt-4o）支持多模态
- [网站](https://platform.openai.com/)，[设置说明](/Usage/API_Connections/openai.md)

### Claude (by Anthropic)

- 推荐给希望 AI 聊天具有创意、独特写作风格的用户
- 需要预付费购买积分，按提示词收费
- 最新的模型（Claude 3）支持多模态
- 需要特定的提示词风格和使用 [prefills](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/prefill-claudes-response) 进行回复引导
- [网站](https://console.anthropic.com/)

### Mistral (by Mistral AI)

- 各种规模和用例的高效模型。您可以在[他们的平台](https://console.mistral.ai/api-keys/)上创建账户和 API 密钥。
- 一般用途的上下文大小从 32k 到 128k，编码用途的上下文大小从 32k 到 256k。
- 有限制的免费层级。
- 合理的审核，Mistral 的主要原则是保持中立并赋能用户，更多信息[在此](https://mistral.ai/terms/)。

### OpenRouter

- WindowAI 浏览器扩展允许您使用自己的 API 密钥连接到上述云端 LLM
- 使用 OpenRouter 付费使用他们的 API 密钥
- 如果您不想在每个服务上创建单独的账户，这很有用
- [WindowAI 网站](https://windowai.io) 和 [OpenRouter 网站](https://openrouter.ai)

### DreamGen

- 针对可控创意写作调优的无审查模型
- 提供免费月度积分和付费订阅
- 模型范围从 7B 到 70B
- [设置说明](DreamGen.md)

### AI Horde

- SillyTavern 可以开箱即用地访问此 API，无需额外设置
- 使用个人志愿者（Horde Workers）的 GPU 来处理您的聊天输入的回应
- 在生成等待时间、AI 设置和可用模型方面受制于 Worker
- [网站](https://aihorde.net/)

### Mancer AI

- 托管各种系列的无限制模型的服务
- 使用"积分"为各种模型的 token 付费
- 默认不记录提示词，但您可以启用它以获得 token 积分折扣
- 使用类似于 `Oobabooga TextGeneration WebUI` 的 API，详情请参见 [Mancer 文档](https://mancer.tech/docs/clients/#sampling-parameters)
- [网站](https://mancer.tech/)，[设置说明](/Usage/API_Connections/mancer.md)

### NovelAI

- 无内容过滤
- 需要付费订阅
- [网站](https://novelai.net/)，[设置说明](/Usage/API_Connections/novelai.md)
