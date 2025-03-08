# DreamGen

DreamGen 是一个用于 AI 驱动的创意写作的应用程序和 API。他们提供免费层级，以及付费订阅，后者允许无限制地访问他们专门为可控 AI 辅助写作目的制作的高质量内部文本生成模型。创建账户以开始使用：<https://dreamgen.com/>。

（免费）积分在每个日历月初重置。查看[定价](https://dreamgen.com/pricing)以了解每个模型的积分成本，查看[使用情况](https://dreamgen.com/account/usage)以了解您剩余的积分。

## 连接到 DreamGen

### 获取 API 密钥

前往 [DreamGen API 密钥](https://dreamgen.com/account/api-keys)页面并点击"新建 API 密钥"按钮。确保将 API 密钥复制到剪贴板。

![创建新的 DreamGen API 密钥](/static/dreamgen/dreamgen_api_keys_new.jpg)
![复制 DreamGen API 密钥](/static/dreamgen/dreamgen_api_keys_copy.jpg)

### 连接

1. 前往 SillyTavern 连接设置。
2. 选择 API：文本补全
3. 选择 API 类型：DreamGen
4. 输入 API 密钥
5. （可选）选择一个模型

![连接到 DreamGen](/static/dreamgen/dreamgen_st_connection.jpg)

## 模型

DreamGen 提供 `opus-v1-sm`、`opus-v1-lg` 和 `opus-v1-xl`。模型越大，它在遵循指令和写出好故事方面就越好。

## 格式设置

DreamGen 模型需要特定的输入格式，这在[此处有文档说明](https://dreamgen.com/docs/models/opus/v1)。

SillyTavern 带有专为 DreamGen 制作的内置预设。确保使用这些设置作为您的基准。
这些设置尽可能地遵循 DreamGen 格式，但由于角色卡片的格式不规则，它并不总是完美的。

1. 前往"高级格式设置"页面。
2. 在"上下文模板"下选择 `DreamGen Role-Play V1 Llama3 / ChatML`，具体取决于模型（*）。
3. **启用"指令模式"。**
4. 在"指令模式预设"下选择 `DreamGen Role-Play V1 Llama3 / ChatML`。

![DreamGen 上下文设置](/static/dreamgen/dreamgen_st_context_settings.jpg)
![DreamGen 指令设置](/static/dreamgen/dreamgen_st_instruct_settings.jpg)

（*）什么时候使用 Llama 3，什么时候使用 ChatML？截至 2024/06/17，`opus-v1-sm` 是 `ChatML`，所有其他模型都是基于 `Llama3` 的。
在运行本地模型时，模板将在模型的 HuggingFace 卡片中指示。

## 补全设置

DreamGen 支持：

- "温度"、"Top P"、"Top K" 和 "Min P"
- "存在惩罚"、"频率惩罚"和"重复惩罚"（无范围）
- "最小长度" -- 让您强制模型至少生成 `min(min_length, max_tokens)` 个 token

好的起始值可能是：

- Min P：0.05
- 温度：0.8
- 重复惩罚：1.1

## 格式化提示

DreamGen 模型与常规的指令遵循模型（如 OpenAI 的 ChatGPT）不同。

这些模型经过微调，可以根据提供的描述写故事，这些描述通常包括情节描述、风格描述、角色、地点、背景等。模型也可以在故事中途被*引导*，让您成为导演，告诉角色他们应该做什么或情节应该如何展开。

一个格式良好的**系统提示词**消息应该是这样的：

```
You are an intelligent, skilled, versatile writer.

Your task is to write a story based on the information below.


## Plot description:

The librarian sets up a blind date between Lucifer and Mia. Lucifer immediately falls in love with Mia, but Mia needs more space and time to make up her mind.


## Style description:

The narrative is vivid and intensely sensual, with a strong emphasis on raw emotion conveyed from a first-person point of view. The language is explicit, evoking intense imagery and indulging in the erotic exploration of the characters' passionate encounters.


## Characters

### Lucifer

Lucifer, the red-skinned, horned demon, is the embodiment of fallen grace. Wrestling with his notorious heritage and a newfound desire for love, his complex nature ferments with vulnerability. His character oscillates between hedonism and self-reflection, hungering for acceptance by Mia and the librarian. Embracing his mortal love, he yearns for transformation, embodying the notion that even the damned may seek solace in love's redemption.

### Mia

Mia is a kind woman...
```

注意，**提示词应该是故事的描述**，而不是关于如何写故事的指令或指示。**避免使用以下短语：**

- "像...一样写故事"
- "确保..."
- 等等

查看更多关于情节、风格和角色描述应该是什么样子的[示例](https://dreamgen.com/docs/models/opus/v1#task-story-writing)。

默认的"DreamGen Role-Play V1"模板将不同部分替换如下：

- `## Plot description:` 将由 {%{`{{scenario}}`}%} 和 {%{`{{wiBefore}}`}%} 组成。
- `## Style description:` 未提供，您应该将其添加到高级设置中的系统提示词下，或添加到角色卡片中 {%{`{{scenario}}`}%} 的末尾。此部分有助于影响叙事风格（第一人称、第二人称、第三人称）、时态（过去时、现在时）、细节和详细程度等。
- `## Characters:` 将有一个 {%{`{{char}}`}%} 角色，其描述由 {%{`{{description}}`}%} 和 {%{`{{personality}}`}%} 组成，以及一个 {%{`{{user}}`}%} 角色，其描述由 {%{`{{persona}}`}%} 组成。

### 消息示例和初始消息

DreamGen 模型对上下文非常敏感 -- 它们将在很大程度上坚持前面对话轮次中呈现的写作风格（和事实）。
这使得消息示例和初始消息变得非常重要。

#### 格式化消息示例

{%{`{{mesExamples}}`}%} 被附加在系统提示词的末尾。要充分利用指令格式，请确保您的示例用 `<START>` 分隔符分隔。例如：

{%{

```
<START>
{{user}}: (用户的回合)
{{char}}: (角色的回合)
<START>
{{user}}: (用户的回合)
{{char}}: (角色的回合)
```

}%}

### 示例

这里有几个为 DreamGen 适配的示例卡片，考虑到了独特的提示方式。这些卡片还利用了上述描述的 {%{`{{mesExamples}}`}%}。

#### Seraphina

这是对 SillyTavern 默认内置的流行 Seraphina 卡片的编辑。

<div style="width:200px;">

[![Seraphina](/static/dreamgen/cards/Seraphina.png)](/static/dreamgen/cards/Seraphina.png)

</div>

#### Lara Lightland

这是对 Deffcolony 的 Lara Lightland 卡片的编辑。

<div style="width:200px;">

[![Lara Lightland](/static/dreamgen/cards/LaraLightland.png)](/static/dreamgen/cards/LaraLightland.png)

</div>

## 常见问题

### 我应该使用什么采样设置？

您可以从以下设置开始：

- 温度：1.0
- MinP：0.05
- 存在惩罚：0.1
- 频率惩罚：0.1

### 如何让回复更长或更短？

您有几个选项：

- 在系统提示词或模型卡片中更改或添加 `## Style description:`。您可以尝试添加类似"句子通常很长，叙述以极其详细的方式描述场景。"的内容。
- 在补全设置中更改 `最小长度`。
- 在指令模式下的高级格式设置中添加类似以下的 `最后输出序列`：

这是一个使用 Llama 3 模板的 `最后输出序列` 示例，可能有助于让模型以更详细的方式回应：

{%{

```
<|eot_id|>
<|start_header_id|>user<|end_header_id|>

Length: 400 words
Plot: {{char}} replies to {{user}} in detailed and elaborate way.<|eot_id|>
<|start_header_id|>writer character: {{char}}<|end_header_id|>


```

使用 ChatML 模板表达相同内容：

}%}


{%{

```
<|im_end|>
<|im_start|>user
Length: 400 words
Plot: {{char}} replies to {{user}} in detailed and elaborate way.<|im_end|>
<|im_start|>text names= {{char}}
```

}%}

您可以将文本更改为更适合您的场景或上下文的内容。

### 如何阻止模型重复自己？

如果模型重复上下文中的内容，您可以尝试在补全设置中增加"重复惩罚"，或者尝试重新措辞被重复的上下文部分。
如果模型在一条消息中重复自己，您可以尝试增加"存在惩罚"或"频率惩罚"。

### 如何引导故事？

如果您想指导角色做某事，或将情节引向某个方向，您可以使用 `user` 角色（即 `<|im_start|>user` 前言）。

目前，这个功能在 SillyTavern 中没有很好地原生集成，但您可以使用上述描述的 `最后输出序列` 来插入 `user`（指令）回合。在[这里](https://dreamgen.com/docs/models/opus/v1#prompt-instructions)查看指令应该是什么样子的示例。
