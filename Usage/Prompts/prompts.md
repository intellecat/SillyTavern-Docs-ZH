---
order: 140
icon: typography
---

# 提示词

当您向 AI 发送消息时，您写的文本会与其他文本组合，形成发送给 AI 的单个请求。这个组合的文本被称为"提示词"，有时也称为"请求"或"上下文"。

提示词可以包含各种不同类型的文本，包括：

* 关于如何生成回应的[主要指令](#主提示词系统提示词)
* [AI 应该扮演的角色](/Usage/Characters/characterdesign.md)的定义
* [您要扮演的角色](/Usage/personas.md)的定义
* 关于 AI 正在交互的[世界信息](/Usage/worldinfo.md)
* 来自[数据库](/Usage/Characters/data-bank.md)的相关文档或信息
* 过去对话的[摘要](/extensions/Summarize.md)
* [网络搜索](/extensions/WebSearch.md)或其他[外部数据源](/For_Contributors/Function-Calling.md)的结果
* 对话中的前几条消息
* **您给 AI 的消息**
* 关于如何生成回应的[最终指令](#历史后指令)

这可能需要管理很多内容！为了帮助您理解如何构建和修改发送给 AI 的请求，SillyTavern 识别了您可能想要包含在提示词中的不同元素。然后，您可以构建您的提示词，包含适合您想要与 AI 交互方式的内容。

许多这些元素在您需要更改它们的部分中都有解释。例如，要描述您希望 AI 扮演的角色，您可以使用[角色设计](/Usage/Characters/characterdesign.md)中的[描述](/Usage/Characters/characterdesign.md#个性概述)字段。

## 查看提示词

阅读发送给 AI 的最终提示词对于理解 AI 被告知了什么，以及为什么它生成了这样的回应非常有帮助。您可以通过以下几种方式查看提示词：

* 使用 AI 回复消息上的提示词项目化图标
* 使用 [Prompt Inspector](https://github.com/SillyTavern/Extension-PromptInspector) 扩展
* 检查运行 SillyTavern 的终端窗口中的日志
* 检查浏览器开发者工具中的控制台

## 更改提示词的构建方式

以正确的方式向 AI 呈现提示词的所有部分对于获得最佳回应至关重要。您可以控制提示词的构建方式。

+++ Text Completion API

使用[高级格式化](advancedformatting.md)面板为 Text Completion API 自定义提示词构建。

+++ Chat Completion API

使用[提示词管理器](prompt-manager.md)为 Chat Completion API 自定义提示词构建。

+++

## 主提示词（系统提示词）

主提示词（或系统提示词）定义了模型要遵循的一般指令。它为对话设定了基调和上下文。例如，它告诉模型要充当 AI 助手、写作伙伴或虚构角色。

+++ Text Completion API

[系统提示词](advancedformatting.md#系统提示词)是[故事字符串](context-template.md#故事字符串)的一部分，通常是模型接收的提示词的第一部分。

+++ Chat Completion API

主提示词是[提示词管理器](prompt-manager.md)中的默认提示词之一。它通常是模型接收的上下文中的第一条消息，归属于（"发送自"）系统角色。

+++

默认的主提示词是：

> Write \{\{char\}\}'s next reply in a fictional chat between \{\{char\}\} and \{\{user\}\}.

\{\{char\}\} 和 \{\{user\}\} 占位符会被替换为您在对话中定义的角色和个性的名称。

您可以在主提示词中使用任何支持的 [\{\{macro\}\}](/Usage/Characters/macros.md) 标签来包含可能在对话之间变化或随对话进展而变化的信息。

### 调整主提示词

默认的主提示词帮助模型理解它应该如何处理后续的角色和个性信息，如何解释过去的对话，以及生成什么样的回应。这是一个灵活的通用提示词，适用于许多情况，因为它确立了 AI 是作为一个角色在与您的个性进行对话。

但是，您可以调整主提示词以更好地满足您的需求。以下是调整主提示词的一些常见原因：

* **提供额外指令**：例如，您希望 AI 解释其推理过程，遵循特定规则，或避免某些主题
* **明确 AI 的角色**：例如，您希望 AI 充当叙述者、讲故事者或向导
* **改变对话的上下文**：例如，您希望 AI 作为 AI 助手、文字冒险游戏或写作伙伴来回应

!!! 尝试并看看什么最适合您
本指南中的所有示例都对其他用户有效，但适合您的需求和您使用的模型的提示词可能不同。尝试不同的指令和提示词风格，看看什么最适合您。如果您不确定要尝试什么，您可以随时在 [SillyTavern Discord](https://discord.gg/sillytavern) 中寻求帮助。
!!!

在主提示词中给 AI 额外的指令可以帮助它理解您想要从对话中得到什么。

> Write one reply only. Write at least one paragraph, up to four.

> Markdown is enabled. Use it to format your response. Enclose code snippets in triple backticks.

> Write character dialogue in quotation marks. Write \{\{char\}\}'s thoughts in parentheses.

> You are an anime roleplay generation model for users aged 13 to 17. You always generate fun, age-appropriate responses.

> Answer truthfully and write out your thinking step by step to be sure you get the right answer.

AI 更容易遵循关于它应该做什么的指令，而不是它不应该做什么的指令。例如，如果您希望 AI 避免以某种方式写作，最好告诉它您希望它如何写作。虽然 *"Do not decide what \{\{user\}\} says or does"* 经常包含在提示词中以防止 AI 控制您的个性，但一些用户发现 *"Write \{\{char\}\}'s responses in a way that respects \{\{user\}\}'s autonomy"* 更有效。

通常有比主提示词更好的地方来包含关于用户或角色的信息，修改角色的写作和说话风格，或给出其他具体指令。主提示词最适合用于关于整个对话的一般指令，或关于您想要进行的对话类型的指令。

### 消息历史的影响

在调整主提示词以改善 AI 的回应时，请考虑 AI 从消息历史中获取了很多信息。历史是它对过去事件、角色互动和关系的记忆，也是它的词汇选择和写作风格的指南。

通过提供[示例消息](/Usage/Characters/characterdesign.md#对话示例)来展示您希望 AI 如何回应，利用这一点。展示您想要的通常比试图解释它更容易！

当您的对话已经有历史时，更改主提示词对 AI 的回应的影响是有限的。在事件和关系方面，AI 假设主提示词发生在遥远的过去，而消息历史更新了它。在写作风格和词汇选择方面，AI 假设历史中的所有消息都是根据*当前*主提示词中的规则生成的，并且它应该继续以相同的方式生成消息。处理这种情况的一些建议是：

* 在消息历史的末尾或之后插入当前指令，例如使用[作者注释](/Usage/Characters/Author's-Note.md)
* 通过开始新的对话来测试您对主提示词的更改
* 编辑消息历史以删除或纠正不想要的行为的示例
* 使用[历史后指令](#历史后指令)为 AI 提供最终指令

!!! 第一次就做对！
永远不要让 AI "逃脱"您不希望它做的事情。如果您不喜欢 AI 的回应，不要继续对话好像它是正确的。相反，修改提示词，重新生成消息，然后从那里继续。这将帮助 AI 学习您想要什么。
!!!

### 移除"虚构聊天"上下文

在某些情况下，"虚构聊天"可能不是您对话的正确上下文。

您可以从主提示词中移除"虚构"上下文：

> Write \{\{char\}\}'s next reply in a conversation with \{\{user\}\}.

您可能不希望 AI 认为自己在角色扮演。与其移除角色的概念，您可以移除 AI 的概念：

> You are \{\{char\}\}, a helpful assistant. You provide useful information and help \{\{user\}\} with their questions.

### AI 作为叙述者或讲故事者

如果您希望 AI 充当叙述者，从全知的角度描述事件，发明自己的角色和场景呢？

一种方法是为 AI 创建一个命名的角色作为叙述者。这个角色可以叫做"叙述者"或"AI"，表明 AI 是一个通用的讲故事者，或者可以根据特定场景或设定命名，给 AI 在该设定中讲故事的任务。然后可以在[角色](/Usage/Characters/characterdesign.md)或[世界信息](/Usage/worldinfo.md)中定义设定的细节。

您需要调整默认的主提示词以反映 AI 的角色。对于通用叙述者，您可以使用：

> You are \{\{char\}\}, a skilled and versatile storyteller. Narrate the story.

或者对于特定设定：

> You are the narrator of a fantasy scenario. Play as the characters that visit \{\{char\}\}.

明确用户在对话中的角色很有帮助。您的消息是故事的一部分，还是对叙述者关于您的角色做什么或说什么的指令？包含用户在故事中的示例：

> The story should progress by responding to the actions and dialogue of \{\{user\}\}. Narrate the story in third person.

将用户排除在故事之外的示例：

> Enter Adventure Mode. Narrate the story based on \{\{user\}\}'s dialogue and actions after ">". Describe the surroundings in vivid detail. Be detailed, creative, verbose, and proactive. Move the story forward by introducing fantasy elements and interesting characters.

定义用户的角色不仅帮助 AI 理解如何回应您的消息，还帮助它理解它在多大程度上可以控制您的个性。这避免了 AI 为您的个性做出您宁愿自己做出的决定的情况。

## 历史后指令

历史后指令是在主提示词和用户消息之后发送给 AI 的额外指令。它们可以用于根据消息历史为 AI 提供额外的上下文或指令。

由于历史后指令是在用户消息之后发送的，它们是 AI 在生成回应之前收到的最后指令。AI 通常给它们比主提示词更高的优先级，它们可以覆盖主提示词的指令。

+++ Text Completion API

历史后指令不能全局定义。您可以使用[作者注释](/Usage/Characters/Author's-Note.md)达到相同的效果。

要使用每个角色的历史后指令，将它们添加到角色的[历史后指令](/Usage/Characters/characterdesign.md)中，并启用**both** [优先使用角色指令](/Usage/User_Settings/User_Settings.md)和[允许历史后指令](context-template.md#允许历史后指令)。

历史后指令作为一个不可见的用户角色注入添加，位于提示词的最后一行之前（通常包含回应消息"标题"）。

+++ Chat Completion API

历史后指令是[提示词管理器](prompt-manager.md)中的默认提示词之一。它通常是模型接收的上下文中的最后一条消息，归属于（"发送自"）系统角色。如果您的 Chat Completion API 不支持系统角色，它通常会归属于用户角色。

+++

## 添加到提示词（世界信息）

您可以使用[世界信息](/Usage/worldinfo.md)功能在提示词的任何位置插入额外信息。通过设置何时应该插入信息的条件，您可以引导 AI 包含特定细节，改变它的回应方式，或向对话添加新元素。

世界信息的一些常见用途包括：

* 关于世界或设定的"知识书"或"百科全书"
* 管理各种角色和情况的不同系统提示词的方式
* 存储 AI 应该在对话中"回忆"的记忆的地方
* 创建、编辑和共享角色细节的更模块化系统
* AI 要反应的随机事件和惊喜的来源，或让您做出反应的来源！
