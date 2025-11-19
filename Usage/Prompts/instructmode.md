---
order: 80
route: /usage/core-concepts/instructmode/
---

# Instruct 模式

Instruct Mode 允许你为训练于各种 prompt 格式的指令遵循模型调整 prompting,如 Alpaca、ChatML、Llama2 等。

!!! Applies to: Text Completion APIs
对于 Chat Completion API 中的等效设置,请使用 [Prompt Manager](prompt-manager.md)。
!!!

## API support

### Text Completion API

完全支持。这包括:

* Text Completion 下的所有源
* KoboldAI Classic
* AI Horde

#### Choosing a formatting

选择的 instruct 模板必须与在 backend 上运行的实际模型的期望相匹配。

这通常反映在 HuggingFace 上的模型卡中,有些甚至提供与 SillyTavern 兼容的 JSON 文件。

示例:[NeverSleep/Noromaid-13b-v0.1.1](https://huggingface.co/NeverSleep/Noromaid-13b-v0.1.1#prompt-template-custom-format-or-alpaca)

### Chat Completion API (OpenAI, Claude, etc)

Chat Completion API **不支持**(也 **不需要**)。它们使用完全不同的 prompt builder。

### NovelAI

虽然 NovelAI *技术上* 支持,但他们的模型都没有接受过理解 instruct 格式的训练。NovelAI 模型可以使用特殊的 instruct 模块,该模块在聊天消息中遇到用花括号包裹的指令时 *自动* 激活,因此对整个 prompt 使用 Instruct Mode 将导致输出质量 **下降**。

以下是为 NovelAI 自动激活 instruct 模块的示例:

```txt
User: { Write a happy song about Nintendo Switch. }
```

## Instruct Mode Settings

### System Prompt

!!!warning Recent change
System Prompt 现在是一个独立的实体。有关更多详细信息,请参阅 [Advanced Formatting](advancedformatting.md#system-prompt) 页面。
!!!

### Templates

提供了一些知名 instruct 模型的带有序列的现成模板。

*更改模板会将未保存的设置重置为上次保存的状态!如果你进行了任何不想丢失的更改,请不要忘记保存你的模板。*

### Activation Regex

如果定义为有效的正则表达式,当连接到模型且其名称与此正则表达式匹配时,将自动选择此模板。

需要预先启用 Instruct mode。仅选择跨模板的第一个正则表达式匹配(按字母顺序评估)。

### Wrap Sequences with Newline

每个序列文本在插入 prompt 时将用换行符包裹。Alpaca 及其衍生品需要此选项。

如果你想完全控制行终止符,请禁用此选项。

### Replace Macro in Sequences

如果启用,在消息包裹序列中定义的已知 \{\{macro\}\} 替换将被替换。

此外,可以在消息前缀中使用特殊的 \{\{name\}\} 宏来引用附加到消息的实际名称(而不是当前活动的 \{\{char\}\} 或 \{\{user\}\}),这在使用群组聊天或 /sendas 命令时很有帮助。如果无法确定名称,则使用 "System" 作为后备占位符。

### Include Names

如果启用,在前缀序列之后在聊天历史日志之前添加角色和用户名称。

可用的选项:

* **Never**: 不在消息内容之前添加名称前缀。
* **Groups and Past Personas**: 仅为群组角色和过去的 persona 的消息添加名称前缀。
* **Always**: 始终在消息内容之前添加名称前缀。

### Sequences: Story String Wrapping

!!!warning Recent change
System Prompt wrapping 已被移除并替换为 Story String wrapping。
!!!

定义当 Position 设置为 "Default (top of context)" 时如何包裹 Story String。

#### Story String Prefix

插入在 Story String 之前。

#### Story String Suffix

插入在 Story String 之后。

### Sequences: Chat Messages Wrapping

这些设置定义了在构建 prompt 时如何包裹属于不同角色的消息。

所有前缀序列也将自动用作停止字符串。

#### User Message Prefix

插入在 User 消息之前,以及在模仿时作为最后一行 prompt。

#### User Message Suffix

插入在 User 消息之后。

#### Assistant Message Prefix

插入在 Assistant 消息之前,以及在生成 AI 回复时作为最后一行 prompt。

#### Assistant Message Suffix

插入在 Assistant 消息之后

#### System Message Prefix

插入在 System(由斜杠命令或扩展添加)消息之前。

#### System Message Suffix

插入在 System 消息之后。

#### System same as User

如果选中为 true,System 消息将使用 User 角色消息序列。

否则,System 消息使用它们自己的序列(如果不为空)或根本不进行任何包裹(如果为空)。

### Misc. Sequences

用于更精细调整 prompt 构建的各种高级配置

#### First Assistant Prefix

插入在第一条 Assistant 消息之前。

!!!info
只有 **聊天历史** 的第一条消息才算,而不是实际进入 prompt 的第一条消息!
!!!

#### Last Assistant Prefix

插入在最后一条 Assistant 消息之前,或在生成 AI 回复时作为最后一行 prompt。

!!!info
在后台生成文本时不使用(例如 Stable Diffusion prompt 或 Summaries)。将改用 System Instruction Prefix 或 Regular Assistant Prefix。
!!!

#### System Instruction Prefix

在后台生成中性/系统文本时作为最后一行 prompt 插入(例如 Stable Diffusion prompt 或 Summaries)。

#### User Filler Message

如果聊天历史不以 User 消息开头,将在聊天历史开始时插入。

**用例:** 当 instruct 格式 *严格要求* prompt 以用户为先且消息仅具有交替角色时,例如:Llama 2 Chat、Mistral Instruct。

#### Stop Sequence

表示回复结束的文本。也作为停止字符串发送到 backend API。

如果生成了停止序列,则将从输出中删除它之后的所有内容(包括序列本身)。
