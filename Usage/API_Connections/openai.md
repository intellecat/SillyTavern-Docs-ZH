---
order: 20
---
# 聊天补全

## OpenAI

### API 密钥

**如何获取：**

1. 前往 [OpenAI](https://platform.openai.com/) 并登录。
2. 使用"[查看 API 密钥](https://platform.openai.com/account/api-keys)"选项创建新的 API 密钥。

**重要提示！**

*丢失的 API 密钥无法恢复！请确保将其安全保存！*

## Claude

如果您有访问 Anthropic 的 Claude API 的权限：

- 在"聊天补全来源"中选择"Claude"。
- 输入您的 API 密钥。
- 点击连接。

## Mistral AI

Mistral AI 是一个开发开源和专有模型的团队，具有高科学标准和开放性重点。您可以通过本地或通过他们的 API 服务 La Plateforme 运行他们的模型。

### API

- 第一步是在 [La Plateforme](https://console.mistral.ai/) 创建账户。
- 完成后，您可以选择一个[计划](https://console.mistral.ai/billing/plans)并设置您的支付信息，或选择免费层级。
- 接下来，您可以创建您的 [API 密钥](https://console.mistral.ai/api-keys/)。密钥可能需要几分钟才能生效！

**重要提示！**  
*丢失的 API 密钥无法恢复！您需要创建一个新的。请确保将其安全保存！*

## 代理

!!!warning
请注意，我们不为您可能遇到的问题提供支持！
我们不保证与每个可能的 API 端点的兼容性！
!!!

!!!
如果您打算使用此代理功能来使用本地端点，如 TabbyAPI、Oobabooga、Aphrodite 或类似的服务，您可能需要查看[这些服务的内置兼容性](/Usage/API_Connections/index.md)。此代理功能主要用于其他暴露 OpenAI 兼容的 API 聊天补全端点的服务和程序。

大多数文本补全 API 支持比 OpenAI 标准允许的更多的自定义选项。这些更大的自定义选项，如 Min-P 采样器，对 SillyTavern 用户来说可能值得一试，这可以大大提高生成质量。
!!!

可以为 OpenAI 的后端配置代理/替代端点。这个自定义端点可以连接到支持通用 OpenAI API 架构的替代聊天补全 API。

实现此 API 的后端示例包括：

* [LM Studio](https://lmstudio.ai/)
* [LiteLLM](https://www.litellm.ai/)
* [LocalAI](https://localai.io/)

访问此功能的方法是：

- 切换到"聊天补全" API 类型。
- 在"聊天补全来源"中选择"OpenAI"。
- 将 API 密钥等详细信息留空。
- 打开"AI 响应配置"标签，滚动到"OpenAI / Claude 反向代理"部分。

在那里，您可以输入代理/自定义端点，如果需要，还可以在"代理密码"下输入 API 密钥。
例如，TabbyAPI 会提供您必须使用的 API 密钥。

在"AI 连接"标签中，您可以找到两个可选的复选框：

- 绕过 API 状态检查。
- 显示"外部"模型（由 API 提供）。

勾选"绕过 API 状态检查"告诉 SillyTavern 停止提醒您关于不工作的 API 端点。如果您的 API 端点正常工作，但 SillyTavern 仍然发出警告，请勾选此项。

> **提示：** 如果不工作，请尝试在端点 URL 末尾添加 `/v1`！

勾选"显示'外部'模型（由 API 提供）"将在下拉列表中显示您的自定义 API 端点报告的可用外部模型（滚动到 OpenAI 模型之后）。这允许您直接从 SillyTavern 选择不同的 API 模型，而无需进入您的自定义应用程序更改模型。

**此功能不是自定义 API 端点工作所必需的**，并且可能不是在每个后端都可用。
