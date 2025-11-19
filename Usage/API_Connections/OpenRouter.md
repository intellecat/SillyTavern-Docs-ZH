---
order: 10
route: /usage/api-connections/openrouter/
---

# OpenRouter

!!!info
OpenRouter 可作为 Text Completion 和 Chat Completion 两种源使用。所有模型都可以通过任一 API 访问,但它们的功能可能因您选择的 API 类型而异。例如,图像内联和工具调用仅在 Chat Completion API 中可用。
!!!

不想注册十几个 API 服务,但仍然想访问所有最新模型?使用 OpenRouter。

OpenRouter 让您使用单一端点访问 DeepSeek、Claude 和 Gemini 等模型,所有服务都在一个平台上,共享信用额度池。

它提供免费试用(约 1 美元)和付费访问。无需订阅或月费 - 您只需为实际使用付费。某些模型提供免费访问,但每日请求数量有限。

!!!tip
要获得对免费模型的永久访问权限并享受慷慨的每日限制,您需要**一次性**购买至少 10 美元的信用额度。

详情请参阅 [OpenRouter FAQ 页面](https://openrouter.ai/docs/faq)。
!!!

- 创建 OpenRouter 账户:[openrouter.ai](https://openrouter.ai/)
- [OpenRouter 模型列表](https://openrouter.ai/models?order=pricing-low-to-high)

![OpenRouter-ConnectionPanel](/static/openrouter-connection.png)

从上到下(参见上图):

1. 选择 'Chat Completion' API。
2. 选择 OpenRouter 作为源。
3. 点击 "Authorize" 使用 OAuth 流程获取密钥。或者,在[此处](https://openrouter.ai/keys)生成 API 密钥并粘贴到框中。
4. 点击 "Connect" 并选择一个模型。
5. (可选)使用 "Test Message" 按钮验证您的连接。
