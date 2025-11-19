---
order: 20
route: /usage/api-connections/openai/
---

# 聊天补全

## 特定来源的说明

!!!warning **重要！**
大多数 API 平台只允许你在创建时查看一次生成的 API key。如果你丢失了它，你需要生成一个新的 key。确保妥善保管！
!!!

### OpenAI

使用 OpenAI 的开发者平台访问各种 OpenAI 模型，包括 gpt-4o、gpt-4.1、o3 等。

**如何获取 API key：**

1. 前往 [OpenAI](https://platform.openai.com/) 并登录。
2. 使用 "[View API keys](https://platform.openai.com/account/api-keys)" 选项创建一个新的 API key。

### Claude

Claude 是由 Anthropic 开发的 AI 模型系列。你可以通过 Anthropic 控制台访问 Claude 模型。

**如何获取 API key：**

1. 前往 [Anthropic Console](https://console.anthropic.com/) 并登录。
2. 使用 "[Get API Key](https://console.anthropic.com/settings/keys)" 部分创建一个新的 API key。

### Mistral AI

Mistral AI 是一个团队，以高科学标准和对开放性的关注开发开放和专有模型。你可以在本地运行他们的模型，或通过他们的 API 服务 La Plateforme 访问。

**如何获取 API key：**

1. 第一步是在 [La Plateforme](https://console.mistral.ai/) 上创建账户。
2. 完成后，你可以选择一个[计划](https://console.mistral.ai/billing/plans)并设置付款信息，或选择免费层。
3. 接下来，你可以创建你的 [API key](https://console.mistral.ai/api-keys/)。你可能需要等待几分钟，key 才会生效！

### DeepSeek

DeepSeek Platform 通过 API 提供对最新 DeepSeek 模型的访问。它们提供一系列模型，包括 DeepSeek V3 和 DeepSeek R1。

**如何获取 API key：**

1. 在 [DeepSeek Platform](https://platform.deepseek.com/) 上注册。
2. 注册并充值账户后，你可以在 "[API keys](https://platform.deepseek.com/api_keys)" 部分创建 API key。

### AI21

AI21 Labs 提供一系列 AI 模型，包括他们的旗舰 Jamba 系列。你可以通过 AI21 Studio API 访问他们的模型。

**如何获取 API key：**

1. 前往 [AI21 Studio](https://studio.ai21.com/) 并登录。
2. 导航到 "Settings => API Keys" 部分创建一个新的 API key。

### Cohere

Cohere 为各种任务提供一套 AI 模型，包括文本生成和 embeddings。你可以通过 Cohere API 访问他们的模型。

**如何获取 API key：**

1. 前往 [Cohere](https://cohere.com/) 并登录。
2. 在账户设置中导航到 "[API Keys](https://dashboard.cohere.com/api-keys)" 部分创建一个新的 API key。

### Perplexity

Perplexity AI 通过其 API 提供对在线启用的 Sonar 模型的访问，用于实时研究和信息检索。

官方入门指南：[Perplexity Quickstart](https://docs.perplexity.ai/getting-started/quickstart)

**如何获取 API key：**

1. 前往 [Perplexity](https://perplexity.ai/) 并登录。
2. 前往 "[API billing](https://www.perplexity.ai/account/api/billing)" 部分购买 API 使用的 credits。
3. 在设置中导航到 "[API keys](https://www.perplexity.ai/account/api/keys)" 部分创建一个新的 API key。

### Fireworks AI

Fireworks AI 是一个高性能平台，提供对最先进的开源语言模型的快速、经济高效的访问。该平台提供具有 OpenAI 兼容 API 的无服务器部署，并支持高达 256,000 tokens 的 context 窗口。

**如何获取 API key：**

1. 前往 [Fireworks AI](https://fireworks.ai/) 并创建账户或登录。
2. 在账户设置中导航到 [API Keys 页面](https://app.fireworks.ai/settings/users/api-keys)。
3. 点击 "Create API key" 并提供一个描述性名称（例如，"SillyTavern"）。

## Electron Hub

Electron Hub 是一个统一的 OpenAI 兼容平台，通过单个 API key 提供对来自多个供应商的模型的访问。

**如何获取 API key：**

1. 在 [Electron Hub](https://playground.electronhub.ai/console) 创建账户。
2. 从 **Console → API Keys** 页面生成 API key。

## 自定义 OpenAI 兼容 endpoint

!!!warning
重要的是要注意，我们不为你可能遇到的问题提供支持！
我们不保证与每个可能的 API endpoint 的兼容性！
!!!

!!!
如果你打算使用此功能来使用本地 endpoint，如 TabbyAPI、Oobabooga、Aphrodite 或任何类似的，你可能想要查看[内置兼容性](/Usage/API_Connections/index.md)。自定义 endpoint 功能主要用于与公开 OpenAI 兼容的 API Chat Completion endpoint 的其他服务和程序一起使用。

大多数 Text Completion API 支持比 OpenAI 标准允许的更多自定义选项。这些更多的自定义选项，如 Min-P sampler，对于 SillyTavern 用户来说可能值得查看，这可以大大提高生成的质量。
!!!

你可以为 Chat Completions 后端配置替代 endpoint。此自定义 endpoint 可以连接到任何支持通用 OpenAI API schema 的服务器。

兼容后端的示例包括：

* [LM Studio](https://lmstudio.ai/)
* [LiteLLM](https://www.litellm.ai/)
* [LocalAI](https://localai.io/)

### 连接

要访问此功能：

1. 切换到 'Chat Completion' API 类型
2. 为 'Chat Completion Source' 选择 'Custom (OpenAI-compatible)'

输入自定义 endpoint URL 和 API key（如果需要）。例如，TabbyAPI 需要 API key 进行身份验证。

!!!tip
**提示：** 如果你遇到连接问题，尝试在 endpoint URL 末尾添加 `/v1`。不要添加 `/chat/completions` 后缀。
!!!

### 选择模型

如果自定义 API 实现了 `/v1/models` endpoint 以提供可用模型列表，你可以从下拉列表中选择。否则，使用文本字段手动输入模型 ID。

勾选 'Bypass API status check' 以防止 SillyTavern 提醒你 API endpoint 无法正常工作。如果你的 API endpoint 工作正常但 SillyTavern 继续显示警告，请启用此选项。

点击 "Test Message" 通过向模型发送一个简单的提示来验证连接性。

## Prompt 后处理

!!!warning
**注意：** 当使用带 "no tools" 的后处理选项时，不支持 Tool Calling！
!!!

某些 endpoint 可能对传入提示的格式施加特定限制，例如只需要一条系统消息或严格交替角色。

SillyTavern 提供内置提示转换器来帮助满足这些要求（从最少到最多限制）：

1. None - 除非 API 严格要求，否则不应用显式处理
2. Merge consecutive messages from the same role
3. Semi-strict - 合并角色并只允许一条可选系统消息
4. Strict - 合并角色，只允许一条可选系统消息，并要求用户消息优先
5. Single user message - 将所有角色的所有消息合并为单个用户消息

Merge、semi-strict 和 strict 还会从提示中删除任何 tool calls，除非选择了 "with tools" 变体。这对于不支持 tool calling 且你现有的提示包含 tool calls 的 API 很有用。

除了"Custom OpenAI-compatible"之外，更少限制的选项对 SillyTavern 中实现的更多限制 endpoint 没有影响；Custom 可能会在无效请求时出错。

在 strict 模式下，如果在第一条助手消息之前不存在用户消息，则将插入 `config.yaml` 中的 `promptPlaceholder`，默认为"\[Start a new chat]"。
