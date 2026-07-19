---
order: 40
tags: ['>=1.12.12']
route: /usage/prompts/reasoning/
---

# 推理

在语言模型中，推理（也称为模型思考）是指一种思维链（CoT）技术，通过逐步分析来模拟人类解决问题的过程。SillyTavern 提供了多项功能，使推理模型的使用在支持的后端之间更加高效和一致。

## 常见问题

1. 使用推理模型时，模型的内部推理过程会占用一部分回复 token 配额，即使这些推理内容不会出现在最终输出中（例如 o3-mini 或 Gemini Thinking）。如果你发现回复不完整或为空，请尝试在 **<i class="fa-solid fa-sliders"></i> AI 回复配置**面板中调整最大回复长度设置。对于推理模型，通常需要设置更高的 token 限制——与标准对话模型相比，通常为 1024 到 4096 token。

## 配置 {#configuration}

!!!
大多数推理相关设置可以在 **<i class="fa-solid fa-font"></i> 高级格式化**面板的"推理"部分进行配置。
!!!

推理块在聊天中显示为可折叠的消息区域。它们可以手动添加、由后端自动添加，或通过回复解析添加（见下文）。

默认情况下，推理块会折叠以节省空间。点击块可展开并查看其内容。你可以通过在推理设置中启用**自动展开**来使块自动展开。

当推理块展开时，你可以使用 **<i class="fa-solid fa-copy"></i> 复制**和 **<i class="fa-solid fa-pencil"></i> 编辑**按钮来复制或编辑其内容。

有些模型支持推理，但不会将其思考过程返回给用户。对于此类模型，可以通过开启**显示隐藏**设置来显示带有推理时间的推理块。

## 添加推理

### 手动添加

通过 **<i class="fa-solid fa-pencil"></i> 消息编辑**菜单为任意消息添加推理块。编辑时点击 **<i class="fa-solid fa-lightbulb"></i>** 可添加推理区域。第三方扩展也可以通过在消息对象添加到聊天之前写入 `extra.reasoning` 字段来添加推理。

### 使用命令

使用 `/reasoning-set` STscript 命令为消息添加推理。该命令接受 `at`（消息 ID，默认为最后一条消息）和推理文本作为参数。

```stscript
/reasoning-set at=0 This is the reasoning for the first message.
```

### 由后端提供 {#by-backend}

如果你选择的 LLM 后端和模型支持推理输出，在 **<i class="fa-solid fa-sliders"></i> AI 回复配置**面板中启用"请求模型推理"将添加一个包含模型思考过程的推理块。

支持的来源：

- Claude
- DeepSeek
- Google AI Studio
- Google Vertex AI
- OpenRouter
- xAI (Grok)
- AI/ML API
- Z.AI
- Pollinations
- MistralAI
- Electron Hub
- Chutes
- NanoGPT
- Moonshot

!!!
对于**大多数**来源，"请求模型推理"不决定模型是否进行推理，因为推理无法被禁用。如果后端和模型支持明确请求禁用推理，该设置将执行禁用操作。否则，模型将始终进行推理。
!!!

各提供商的特定说明：

- Claude 和 Google（2.5 Flash）允许切换思考模式；参见[推理力度](#reasoning-effort)。
- 可以为 [Z.AI (GLM)](https://docs.z.ai/api-reference/llm/chat-completion#body-one-of-0-thinking) 和 [Moonshot (Kimi)](https://platform.moonshot.ai/docs/guide/use-kimi-k2-thinking-model) 禁用推理。该设置映射到 `thinking.type` 参数。它们不支持"推理力度"。
- 对于 OpenRouter，当"请求模型推理"开关关闭且推理力度设置为最低时，对于支持的模型，思考将被设置为禁用。具体行为取决于模型；某些提供商可能会拒绝此类请求。

### 通过解析添加

在 **<i class="fa-solid fa-font"></i> 高级格式化**面板中启用"自动解析"，可自动从模型输出中解析推理内容。

回复必须包含用配置的前缀和后缀序列包裹的推理区域。默认提供的序列对应 DeepSeek R1 的推理格式。对于某些返回未解析推理的 API 来源（如 MiniMax 或 Perplexity），需要启用此功能。

前缀为 `<think>`、后缀为 `</think>` 的示例：

```txt
<think>
This is the reasoning.
</think>

This is the main content.
```

## 将推理用于提示词

默认情况下，已识别的推理块内容不会发送回模型。要在提示词中包含推理，请在 **<i class="fa-solid fa-font"></i> 高级格式化**面板中启用"添加到提示词"。推理内容将用配置的前缀和后缀序列包裹，并通过分隔符与主要上下文分开。"最大添加数量"数值设置控制可以包含多少个推理块，从提示词末尾开始计数。

!!!
大多数模型提供商不建议在多轮对话中将思维链发送回模型。
!!!

### 从推理继续

一种特殊情况：当生成被继续时（例如通过 **<i class="fa-solid fa-bars"></i> 选项**菜单按下"继续"），且被继续的消息只包含推理而没有实际内容时，即使未启用"添加到提示词"开关，推理也可以发送回模型。这让模型有机会完成不完整的推理并开始生成主要内容。提示词将按如下方式发送：

```txt
<think>
Incomplete reasoning...
```

## 正则表达式脚本

来自[正则表达式扩展](/extensions/Regex.md)的正则表达式脚本可以应用于推理块的内容。在脚本编辑器的"影响范围"部分勾选"推理"，即可专门针对推理块。

不同的临时性选项对推理块的影响如下：

1. 无临时性：推理内容被永久修改。
2. 编辑时运行：编辑推理块时将重新评估正则表达式脚本。
3. 修改聊天显示：正则表达式应用于推理块的显示文本，而非底层内容。
4. 修改发出的提示词：正则表达式仅在推理块发送给模型之前应用。

## 推理力度 {#reasoning-effort}

推理力度是 **<i class="fa-solid fa-sliders"></i> AI 回复配置**面板中的聊天补全设置，影响推理可能使用的 token 数量。每个选项的效果取决于所连接的来源。对于下表中的来源，"自动"表示请求中不包含相关参数。

| 选项 | Claude（如无流式传输则 ≤ 21333） | OpenAI（关键词） | OpenRouter（关键词） | xAI (Grok)（关键词） | Perplexity（关键词） | NanoGPT（关键词） |
| ---- | -------------------------------- | ---------------- | -------------------- | -------------------- | -------------------- | ----------------- |
| 适用模型 | Opus 4、Sonnet 4/3.7 | o4-mini、o3\*、o1\* | 适用模型 | grok-3-mini | sonar-deep-research | 适用模型 |
| 自动 | 未指定，**不思考** | 未指定 | 未指定，效果取决于模型 | 未指定 | 未指定 | 未指定 |
| 最低 | 预算 1024 token | "low" | "low"，或最大回复的 20% | "low" | "low" | "none" |
| 低 | 最大回复的 15%，最少 1024 | "low" | "low"，或最大回复的 20% | "low" | "low" | "minimal" |
| 中 | 最大回复的 25%，最少 1024 | "medium" | "medium"，或最大回复的 50% | "low" | "medium" | "low" |
| 高 | 最大回复的 50%，最少 1024 | "high" | "high"，或最大回复的 80% | "high" | "high" | "medium" |
| 最高 | 最大回复的 95%，最少 1024 | "high" | "high"，或最大回复的 80% | "high" | "high" | "high" |

- 对于不支持自适应思考的旧版 Claude 模型，如果禁用流式传输，预算上限为 21333。如果计算出的预算低于 1024，则最大回复将更改为 2048。
- Claude 还为 Opus 4.6+ 模型支持自适应思考，可通过 [config.yaml](/Administration/config-yaml.md) 中的 `claude.enableAdaptiveThinking` 启用（Opus 4.7+ 始终开启）。启用后，推理力度设置将映射到自适应思考级别而非 token 预算。此设置优先于适用模型的"详细程度"设置。
- 对于 OpenRouter、Pollinations、Perplexity、xAI、Chutes、DeepSeek、AI/ML API、xAI、Electron Hub，只发送 OpenAI 风格的关键词。
- 对于 OpenAI 上的 GPT-5.4 和 GPT-5.5 模型，"最低"推理力度对应"none"，即禁用推理。
- 对于以聊天补全自定义 API 来源运行的 KoboldCpp，推理力度作为 `reasoning_effort` 参数发送，值为"minimal"、"low"、"medium"、"high"和"xhigh"。
- 对于其他自定义（OpenAI 兼容）来源，仅当模型在官方 OpenAI 来源上支持推理力度时，才会发送推理力度参数。

Google AI Studio 和 Vertex AI 如下：

| 模型 | 自动（动态思考） | 最低 | 低 | 中 | 高 | 最高 |
| ---- | --------------- | ---- | -- | -- | -- | ---- |
| 2.5 Pro | thinkingBudget = -1 | 128 | 最大回复的 15%，最少 128 | 最大的 25% | 最大的 50% | 取最大值与 32768 中的较小值 |
| 2.5 Flash | thinkingBudget = -1 | 0，**不思考** | 最大回复的 15% | 最大的 25% | 最大的 50% | 取最大值与 24576 中的较小值 |
| 2.5 Flash Lite | thinkingBudget = -1 | 0，**不思考** | 最大回复的 15%，最少 512 | 最大的 25% | 最大的 50% | 取最大值与 24576 中的较小值 |
| 3.0/3.1 Pro | thinkingLevel = null | "low" | "low" | "low" | "high" | "high" |
| 3.0/3.1 Flash | thinkingLevel = null | "minimal" | "low" | "medium" | "high" | "high" |

- 对于 Gemini 2.5 Pro 和 2.5 Flash/Lite，无论流式传输设置如何，预算分别上限为 32768 和 24576 token。
