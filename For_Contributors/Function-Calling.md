---
order: -10
icon: code-review
route: /for-contributors/function-calling/
---

# 函数调用

函数调用允许通过让 LLM 使用结构化数据来为您的扩展添加动态功能，然后您可以使用这些数据触发扩展的特定功能。

!!! warning 注意
此功能目前正在开发中。实现细节可能会发生变化。
!!!

## 示例用例

1. 查询外部 API 以获取额外信息（新闻、天气、网络搜索等）。
2. 根据用户输入执行计算或转换。
3. 存储和回忆重要记忆或事实，包括 RAG 和数据库查询。
4. 在对话中引入真正的随机性（掷骰子、抛硬币等）。

## 官方支持使用函数调用的扩展

1. [图像生成](/extensions/Stable-Diffusion.md)（内置）- 根据用户提示生成图像。
2. [网络搜索](/extensions/WebSearch.md) - 触发查询的网络搜索。
3. [RSS](https://github.com/SillyTavern/Extension-RSS/) - 从 RSS 源获取最新新闻。
4. [AccuWeather](https://github.com/SillyTavern/Extension-AccuWeather) - 从 AccuWeather 获取天气信息。
5. [D&D Dice](https://github.com/SillyTavern/Extension-Dice) - 为 D&D 游戏掷骰子。

## 前提条件和限制

1. 此功能仅适用于某些聊天补全源：OpenAI、Claude、MistralAI、Groq、Cohere、OpenRouter 和自定义 API 源。
2. 文本补全 API 不支持函数调用，但一些本地托管的后端（如 Ollama 和 TabbyAPI）可以在聊天补全下以自定义 OpenAI 兼容模式运行。
3. 必须首先由用户明确允许使用函数调用功能。这是通过在 AI 回应配置面板中启用"启用函数调用"选项来完成的。
4. 不能保证 LLM 会执行任何函数调用。大多数都需要通过提示词进行明确的"激活"（例如，用户要求"掷骰子"、"获取天气"等）。
5. 并非所有提示词都能触发工具调用。继续、扮演、背景（'安静'）提示词不允许触发工具调用。它们仍然可以在其响应中使用过去成功的工具调用。

## 如何制作函数工具

### 检查功能是否受支持

要确定是否支持函数工具调用功能，您可以从 `SillyTavern.getContext()` 对象调用 `isToolCallingSupported`。这将检查当前 API 是否支持函数工具调用以及设置中是否启用了该功能。以下是如何检查功能是否受支持的示例：

```ts
if (SillyTavern.getContext().isToolCallingSupported()) {
    console.log("Function tool calling is supported");
} else {
    console.log("Function tool calling is not supported");
}
```

### 注册函数

要注册函数工具，您需要从 `SillyTavern.getContext()` 对象调用 `registerFunctionTool` 函数并传递所需参数。以下是如何注册函数工具的示例：

```ts
SillyTavern.getContext().registerFunctionTool({
    // 函数工具的内部名称。必须唯一。
    name: "myFunction",
    // 函数工具的显示名称。将在 UI 中显示。（可选）
    displayName: "My Function",
    // 函数工具的描述。必须描述函数的功能和使用时机。
    description: "My function description. Use when you need to do something.",
    // 函数工具参数的 JSON schema。参见：https://json-schema.org/
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
    // 工具触发时要调用的函数。可以是异步的。
    // 如果结果不是字符串，它将被 JSON 字符串化。
    action: async ({ param1, param2 }) => {
        // 您的函数代码在这里
        console.log(`Function called with parameters: ${param1}, ${param2}`);
        return "Function result";
    },
    // 可选函数，用于格式化调用函数时显示的 toast 消息。
    // 如果返回空字符串，则不会显示 toast 消息。
    formatMessage: ({ param1, param2 }) => {
        return `Function is called with: ${param1} and ${param2}`;
    },
    // 可选函数，返回一个布尔值，指示是否应为当前提示词注册工具。
    // 如果没有提供 shouldRegister 函数，工具将为每个提示词注册。
    shouldRegister: () => {
        return true;
    },
});
```

### 取消注册函数

要停用函数工具，您需要从 `SillyTavern.getContext()` 对象调用 `unregisterFunctionTool` 函数并传递要禁用的函数工具的名称。以下是如何取消注册函数工具的示例：

```ts
SillyTavern.getContext().unregisterFunctionTool("myFunction");
```

## 提示和技巧

1. 成功的工具调用会作为可见历史的一部分保存，并将在聊天 UI 中显示，因此您可以检查实际的参数和结果。
2. 如果您不想在聊天历史中看到工具调用。如果您想用自定义 CSS 设置样式或隐藏它们，请针对 `.mes` 元素上的 `toolCall` 类，例如 `.mes.toolCall { display: none; }` 或 `.mes.toolCall { color: #999; }`。
