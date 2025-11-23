---
order: 40
tags: ['>=1.12.12']
route: /usage/prompts/reasoning/
---

# 推理

在语言模型中，推理（也称为模型思考）指的是一种思维链（CoT）技术，通过逐步分析来模拟人类解决问题的过程。SillyTavern 提供了多项功能，使推理模型在支持的后端上的使用更加高效和一致。

## 常见问题

1. 使用推理模型时，模型的内部推理过程会消耗部分响应令牌额度，即使此推理未在最终输出中显示（例如 o3-mini 或 Gemini Thinking）。如果您注意到响应不完整或为空，应尝试调整 **<i class="fa-solid fa-sliders"></i> AI 响应配置**面板中的最大响应长度设置。对于推理模型，通常使用明显更高的令牌限制 - 从 1024 到 4096 个令牌 - 相比标准对话模型。

## 配置

!!!
大多数与推理相关的设置可以在 **<i class="fa-solid fa-font"></i> 高级格式化**面板的"推理"部分中配置。
!!!

推理块在聊天中显示为可折叠的消息部分。它们可以手动添加、由后端自动添加，或通过响应解析（见下文）。

默认情况下，推理块是折叠的以节省空间。点击块以展开并查看其内容。您可以通过在推理设置中启用**自动展开**来设置块自动展开。

当推理块展开时，您可以使用 **<i class="fa-solid fa-copy"></i> 复制**和 **<i class="fa-solid fa-pencil"></i> 编辑**按钮复制或编辑其内容。

某些模型支持推理，但不会发回它们的思考。通过切换**显示隐藏**设置，仍然可以显示带有推理时间的推理块。

## 添加推理

### 手动添加

通过 **<i class="fa-solid fa-pencil"></i> 消息编辑**菜单向任何消息添加推理块。在编辑时点击 **<i class="fa-solid fa-lightbulb"></i>** 以添加推理部分。第三方扩展也可以通过在将消息对象添加到聊天之前写入消息对象的 `extra.reasoning` 字段来添加推理。

### 使用命令

使用 `/reasoning-set` STscript 命令向消息添加推理。该命令将 `at`（消息 ID，默认为最后一条消息）和推理文本作为参数。

```stscript
/reasoning-set at=0 This is the reasoning for the first message.
```

### 通过后端

如果您选择的 LLM 后端和模型支持推理输出，在 **<i class="fa-solid fa-sliders"></i> AI 响应配置**面板中启用"请求模型推理"将添加包含模型思考过程的推理块。

支持的来源：

- Claude
- DeepSeek
- Google AI Studio
- Google Vertex AI
- OpenRouter
- xAI (Grok)
- AI/ML API

"请求模型推理"不决定模型是否进行推理。Claude 和 Google（2.5 Flash）允许切换思考模式；请参见[推理努力程度](#reasoning-effort)。

### 通过解析

在 **<i class="fa-solid fa-font"></i> 高级格式化**面板中启用"自动解析"以自动从模型输出中解析推理。

响应必须包含包裹在配置的前缀和后缀序列中的推理部分。默认提供的序列对应于 DeepSeek R1 推理格式。

前缀 `<think>` 和后缀 `</think>` 的示例：

```txt
<think>
This is the reasoning.
</think>

This is the main content.
```

## 带推理的提示

默认情况下，已识别的推理块内容不会发送回模型。要在提示词中包含推理，请在 **<i class="fa-solid fa-font"></i> 高级格式化**面板中启用"添加到提示词"。推理内容将包裹在配置的前缀和后缀序列中，并通过分隔符与主上下文分开。最大添加数量设置控制可以包含多少个推理块，从提示词末尾开始计数。

!!!
大多数模型提供商不建议在多轮对话中将 CoT 发送回模型。
!!!

### 从推理继续

当推理可以在不启用"添加到提示词"切换的情况下发送回模型的特殊情况是，当生成继续时（例如通过从 **<i class="fa-solid fa-bars"></i> 选项**菜单按"继续"），但正在继续的消息仅包含推理而没有实际内容。这使模型有机会完成不完整的推理并开始生成主要内容。提示词将如下发送：

```txt
<think>
Incomplete reasoning...
```

## 正则表达式脚本

[正则表达式扩展](/extensions/Regex.md)中的正则表达式脚本可以应用于推理块的内容。在脚本编辑器的"影响"部分中选中"推理"以专门针对推理块。

不同的临时性选项以以下方式影响推理块：

1. 无临时性：推理内容被永久更改。
2. 编辑时运行：编辑推理块时将重新评估正则表达式脚本。
3. 更改聊天显示：正则表达式应用于推理块的显示文本，而不是底层内容。
4. 更改传出提示词：仅在将推理块发送到模型之前应用正则表达式。

## 推理努力程度

推理努力程度是 **<i class="fa-solid fa-sliders"></i> AI 响应配置**面板中的聊天补全设置，它影响推理可能使用的令牌数量。每个选项的效果取决于连接的来源。对于下面的来源，Auto 简单地意味着相关参数不包含在请求中。

| 选项 | Claude（如果无流式传输则 ≤ 21333） | OpenAI（关键词） | OpenRouter（关键词） | xAI (Grok)（关键词） | Perplexity（关键词） |
| --- | --- | --- | --- | --- | --- |
| 模型 | Opus 4, Sonnet 4/3.7 | o4-mini, o3\*, o1\* | 适用的模型 | grok-3-mini | sonar-deep-research |
| Auto | 未指定，**无思考** | 未指定 | 未指定，效果取决于 | 未指定 | 未指定 |
| Minimum | 预算 1024 个令牌 | "low" | "low"，或最大响应的 20% | "low" | "low" |
| Low | 最大响应的 15%，最小 1024 | "low" | "low"，或最大响应的 20% | "low" | "low" |
| Medium | 最大响应的 25%，最小 1024 | "medium" | "medium"，或最大响应的 50% | "low" | "medium" |
| High | 最大响应的 50%，最小 1024 | "high" | "high"，或最大响应的 80% | "high" | "high" |
| Maximum | 最大响应的 95%，最小 1024 | "high" | "high"，或最大响应的 80% | "high" | "high" |

- 对于 Claude，如果禁用流式传输，预算上限为 21333。如果计算的预算少于 1024，则最大响应更改为 2048。
- 对于 OpenRouter、Perplexity 和 AI/ML API，仅发送 OpenAI 样式的关键词。

Google AI Studio 和 Vertex AI 如下：

| 模型 | Auto（动态思考） | Minimum | Low | Medium | High | Maximum |
| --- | --- | --- | --- | --- | --- | --- |
| 2.5 Pro | thinkingBudget = -1 | 128 | 最大响应的 15%，最小 128 | 最大的 25% | 最大的 50% | 最大或 32768 中的较小值 |
| 2.5 Flash | thinkingBudget = -1 | 0，**无思考** | 最大响应的 15% | 最大的 25% | 最大的 50% | 最大或 24576 中的较小值 |
| 2.5 Flash Lite | thinkingBudget = -1 | 0，**无思考** | 最大响应的 15%，最小 512 | 最大的 25% | 最大的 50% | 最大或 24576 中的较小值 |

- 对于 Gemini 2.5 Pro 和 2.5 Flash/Lite，预算分别上限为 32768 或 24576 个令牌，无论流式传输设置如何。
