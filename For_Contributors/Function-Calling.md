---
order: -10
icon: code-review
route: /for-contributors/function-calling/
---

# Function Calling

Function Calling 允许通过让 LLM 使用结构化数据来为扩展添加动态功能,然后您可以使用这些数据来触发扩展的特定功能。

## 示例用例

1. 查询外部 API 以获取附加信息(新闻、天气、网络搜索等)。
2. 根据用户输入执行计算或转换。
3. 存储和回忆重要的记忆或事实,包括 RAG 和数据库查询。
4. 在对话中引入真正的随机性(掷骰子、抛硬币等)。

## 使用 function calling 的官方支持扩展

1. [Image Generation](/extensions/Stable-Diffusion.md) (内置) - 根据用户 prompts 生成图像。
2. [Web Search](/extensions/WebSearch.md) - 触发查询的网络搜索。
3. [RSS](https://github.com/SillyTavern/Extension-RSS/) - 从 RSS feeds 获取最新新闻。
4. [AccuWeather](https://github.com/SillyTavern/Extension-AccuWeather) - 从 AccuWeather 获取天气信息。
5. [D&D Dice](https://github.com/SillyTavern/Extension-Dice) - 为 D&D 游戏掷骰子。

## 先决条件和限制

1. 此功能仅适用于某些 Chat Completion 源: OpenAI、Claude、MistralAI、Groq、Cohere、OpenRouter、AI21、Google AI Studio、Google Vertex AI、DeepSeek、AI/ML API 和 Custom API 源。
2. Text Completion API 不支持 function calls,但某些本地托管的后端(如 Ollama 和 TabbyAPI)可能在 Chat Completion 下以 Custom OpenAI 兼容模式运行。
3. 必须首先由用户明确允许对 function calling 的支持。这可以通过在 AI Response Configuration 面板中启用"Enable function calling"选项来完成。
4. 不能保证 LLM 会执行任何 function calls。大多数 LLM 需要通过 prompt 进行明确的"激活"(例如,用户要求"Roll a dice"、"Get the weather"等)。
5. 并非所有 prompts 都可以触发 tool call。Continuations、impersonation、background('quiet')prompts 不允许触发 tool call。它们仍然可以在响应中使用过去成功的 tool calls。

## 如何制作 function tool

### 检查是否支持该功能

要确定是否支持 function tool calling 功能,您可以从 `SillyTavern.getContext()` 对象调用 `isToolCallingSupported`。这将检查当前 API 是否支持 function tool calling 以及是否在设置中启用。以下是如何检查是否支持该功能的示例:

```ts
if (SillyTavern.getContext().isToolCallingSupported()) {
    console.log("Function tool calling is supported");
} else {
    console.log("Function tool calling is not supported");
}
```

### 注册函数

要注册 function tool,您需要从 `SillyTavern.getContext()` 对象调用 `registerFunctionTool` 函数并传递所需的参数。以下是如何注册 function tool 的示例:

```ts
SillyTavern.getContext().registerFunctionTool({
    // Internal name of the function tool. Must be unique.
    name: "myFunction",
    // Display name of the function tool. Will be shown in the UI. (Optional)
    displayName: "My Function",
    // Description of the function tool. Must describe what the function does and when to use it.
    description: "My function description. Use when you need to do something.",
    // JSON schema for the parameters of the function tool. See: https://json-schema.org/
    parameters: {
        $schema: 'http://json-schema.org/draft-04/schema#',
        type: 'object',
        properties: {
            param1: {
                type: 'string',
                description: 'Parameter 1 description',
            },
            param2: {
                type: 'string',
                description: 'Parameter 2 description',
            },
        },
        required: [
            'param1', 'param2',
        ],
    },
    // Function to call when the tool is triggered. Can be async.
    // If the result is not a string, it will be JSON-stringified.
    action: async ({ param1, param2 }) => {
        // Your function code here
        console.log(`Function called with parameters: ${param1}, ${param2}`);
        return "Function result";
    },
    // Optional function to format the toast message displayed when the function is invoked.
    // If an empty string is returned, no toast message will be displayed.
    formatMessage: ({ param1, param2 }) => {
        return `Function is called with: ${param1} and ${param2}`;
    },
    // Optional function that returns a boolean value indicating whether the tool should be registered for the current prompt.
    // If no shouldRegister function is provided, the tool will be registered for every prompt.
    shouldRegister: () => {
        return true;
    },
    // Optional flag. If set to true, the function call will be performed, but the result won't be recorded to the visible chat history.
    stealth: false,
});
```

### 注销函数

要停用 function tool,您需要从 `SillyTavern.getContext()` 对象调用 `unregisterFunctionTool` 函数并传递要禁用的 function tool 的名称。以下是如何注销 function tool 的示例:

```ts
SillyTavern.getContext().unregisterFunctionTool("myFunction");
```

## 提示和技巧

1. 成功的 tool calls 作为可见历史的一部分保存,并将显示在聊天 UI 中,因此您可以检查实际参数和结果。如果不希望这样,请在注册 function tool 时设置 `stealth: true` 标志。
2. 如果您不想在聊天历史中看到 tool call。如果您想使用自定义 CSS 进行样式化或隐藏它们,请针对 `.mes` 元素上的 `toolCall` 类,即 `.mes.toolCall { display: none; }` 或 `.mes.toolCall { color: #999; }`。
