---
order: -10
icon: code-review
route: /for-contributors/function-calling/
---

# 函数调用

函数调用允许通过让 LLM 使用结构化数据来为你的扩展添加动态功能，你可以利用这些数据触发扩展的特定功能。

## 使用场景示例

1. 查询外部 API 以获取附加信息（新闻、天气、网页搜索等）。
2. 根据用户输入进行计算或单位换算。
3. 存储和检索重要记忆或事实，包括 RAG 和数据库查询。
4. 在对话中引入真正的随机性（掷骰子、抛硬币等）。

## 官方支持函数调用的扩展

1. [图像生成](/extensions/Stable-Diffusion.md)（内置）——根据用户提示生成图像。
2. [网页搜索](/extensions/WebSearch.md)——触发对指定查询的网页搜索。
3. [RSS](https://github.com/SillyTavern/Extension-RSS/)——从 RSS 订阅源获取最新新闻。
4. [天气](https://github.com/SillyTavern/Extension-Weather)——从天气 API 获取天气信息。
5. [D&D 骰子](https://github.com/SillyTavern/Extension-Dice)——为 D&D 游戏掷骰子。

## 前提条件与限制

1. 此功能仅适用于 Chat Completion API，支持以下来源：Custom（OpenAI 兼容）、AI/ML API、AI21、Azure OpenAI、Chutes、Claude、Cloudflare Workers AI、Cohere、DeepSeek、Electron Hub、Fireworks、Google AI Studio、Google Vertex AI、Groq、MiniMax、MistralAI、Moonshot（Kimi）、NanoGPT、OpenAI、OpenRouter、Pollinations、SiliconFlow、xAI（Grok）以及 Z.AI（GLM）。
2. Text Completion API 不支持函数调用，但部分本地托管的后端（如 Ollama 和 TabbyAPI）可以在 Chat Completion 模式下以 Custom OpenAI 兼容模式运行。
3. 函数调用支持必须由用户显式启用。在 AI 响应配置面板中开启"启用函数调用"选项即可。
4. 无法保证 LLM 一定会执行函数调用。大多数情况下需要通过提示词明确"激活"（例如，用户要求"掷骰子"、"查询天气"等）。
5. 并非所有提示词都能触发工具调用。续写、扮演、后台（"静默"）提示词不允许触发工具调用，但它们仍然可以在响应中使用过去成功的工具调用结果。
6. 某些模型可能不支持函数调用，即使其 API 来源支持也如此。请参阅你的 API 提供商文档，了解哪些模型支持函数调用。

## 工具调用递归限制

为防止工具调用陷入无限循环，系统设有递归限制（默认为 5 轮）。如果 LLM 持续重复调用工具，执行将在达到限制后停止。你可以在 AI 响应配置设置中调整此限制，位置在"启用函数调用"选项旁边。

## 交错思考

在使用带有[推理模型](../Usage/Prompts/reasoning.md)的工具调用时，你可以利用工具调用请求一并返回的推理内容来维护交错思考的上下文。对于某些模型，这是确保工具调用之间一致性和保留重要细节的必要手段。

在 AI 响应配置中选择以下其中一项设置：

- 禁用：工具调用请求中不包含任何推理上下文。兼容性最高，为默认设置。
- 自最新用户消息起：包含最新用户消息之后所有工具调用轮次的推理内容。
- 活跃工具链：仅在当前未解决的工具循环进行中时包含推理内容；一旦产生正常的助手回复，旧的工具链推理内容将不再重新发送。

## 如何创建函数工具

### 检查功能是否受支持

使用 `SillyTavern.getContext()` 中的 `isToolCallingSupported()` 检查当前 API 是否支持函数工具调用以及是否已在设置中启用：

```js
const { isToolCallingSupported } = SillyTavern.getContext();

if (isToolCallingSupported()) {
    console.log('Function tool calling is supported');
}
```

你也可以检查特定生成类型是否能执行工具调用。续写、扮演和后台（"静默"）提示词不允许触发工具调用：

```js
const { canPerformToolCalls } = SillyTavern.getContext();

if (canPerformToolCalls('normal')) {
    console.log('Can perform tool calls for this generation');
}
```

### 注册函数工具

使用 `SillyTavern.getContext()` 中的 `registerFunctionTool()` 注册一个工具。工具定义的参数遵循 [JSON Schema](https://json-schema.org/) 格式：

```js
const { registerFunctionTool } = SillyTavern.getContext();

registerFunctionTool({
    name: 'get_weather',
    displayName: 'Get Weather',
    description: 'Get the current weather for a given location',
    parameters: {
        $schema: 'http://json-schema.org/draft-04/schema#',
        type: 'object',
        properties: {
            location: {
                type: 'string',
                description: 'The city name, e.g. "London"',
            },
            unit: {
                type: 'string',
                enum: ['celsius', 'fahrenheit'],
                description: 'Temperature unit',
            },
        },
        required: ['location'],
    },
    action: async ({ location, unit }) => {
        // Perform your logic here (API calls, computations, etc.)
        const data = await fetchWeatherData(location, unit);
        return JSON.stringify(data);
    },
    formatMessage: ({ location }) => `Checking weather for ${location}...`,
    shouldRegister: () => isWeatherFeatureEnabled(),
    stealth: false,
});
```

### 注册字段说明

| 字段 | 是否必填 | 说明 |
|------|----------|------|
| `name` | 是 | 工具的唯一标识符 |
| `displayName` | 否 | 在 UI 中显示的用户友好名称 |
| `description` | 是 | 发送给 LLM 的描述，说明工具的功能及使用时机 |
| `parameters` | 是 | 定义工具输入参数的 JSON Schema |
| `action` | 是 | LLM 调用工具时执行的函数。接收解析后的参数对象，可以是异步函数，必须返回字符串结果（非字符串值将被 JSON 序列化）。 |
| `formatMessage` | 否 | 返回工具执行期间显示为提示条的字符串的函数。返回空字符串可禁止显示提示条。 |
| `shouldRegister` | 否 | 返回布尔值的函数；若为 `false`，工具将从当前请求中排除。若未提供，工具将在每次提示中注册。 |
| `stealth` | 否 | 若为 `true`，工具调用结果不会记录到可见聊天记录中，也不会触发后续生成 |

### 注销函数工具

要停用某个函数工具，请使用工具名称调用 `unregisterFunctionTool()`：

```js
const { unregisterFunctionTool } = SillyTavern.getContext();

unregisterFunctionTool('get_weather');
```

## 提示与技巧

1. 成功的工具调用会作为可见聊天记录的一部分保存，并显示在聊天界面中，你可以查看实际的参数和结果。如果不希望如此，注册工具时设置 `stealth: true`。
2. 若要用自定义 CSS 美化或隐藏工具调用消息，请针对 `.mes` 元素上的 `toolCall` 类进行操作，例如 `.mes.toolCall { display: none; }` 或 `.mes.toolCall { opacity: 0.5; }`。
3. 为工具编写清晰、具体的描述——LLM 会根据描述决定何时以及如何调用工具。应包含工具使用时机的说明。
4. 保持参数 schema 简洁且文档完善。每个属性的 `description` 有助于 LLM 填入正确的值。
5. 工具调用有递归限制（默认 5 轮）。如果 LLM 持续重复调用工具，执行将在达到限制后停止。
