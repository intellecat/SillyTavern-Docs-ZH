---
order: 140
icon: typography
templating: false
route: /usage/prompts/
---

# 提示词

当您向 AI 发送消息时，您编写的文本会与其他文本结合形成发送给 AI 的单个请求。这种组合的文本称为"提示词"，有时也称为"请求"或"上下文"。

提示词可以包括各种不同类型的文本,包括：

* [主要指令](#main-prompt-system-prompt)告诉 AI 如何生成响应
* [AI 应扮演的角色](/Usage/Characters/characterdesign.md)的定义
* [您扮演的角色](/Usage/personas.md)的定义
* [关于 AI 交互的"世界"的信息](/Usage/worldinfo.md)
* [数据库](/Usage/Characters/data-bank.md)中的相关文档或信息
* 过去对话的[摘要](/extensions/Summarize.md)
* [网络搜索](/extensions/WebSearch.md)或其他[外部数据源](/For_Contributors/Function-Calling.md)的结果
* 对话中的先前消息
* **您发给 AI 的消息**
* AI 如何生成响应的[最终指令](#post-history-instructions)

这可能需要管理很多内容！为了帮助您了解如何构建和修改发送给 AI 的请求，SillyTavern 识别了您可能想要包含在提示词中的不同元素。然后，您可以构建提示词以包含对您想要与 AI 交互的方式有意义的内容。

这些元素中的许多在您更改它们的部分中进行了说明。例如，要描述您希望 AI 扮演的角色，您可以使用[角色设计](/Usage/Characters/characterdesign.md)中的[描述](/Usage/Characters/characterdesign.md#personality-summary)字段。

## 查看提示词

阅读发送给 AI 的最终提示词对于理解 AI 被告知了什么以及为什么生成该响应非常有帮助。您可以通过多种方式查看提示词：

* 使用 AI 回复消息上的提示词详细信息图标
* 使用 [Prompt Inspector](https://github.com/SillyTavern/Extension-PromptInspector) 扩展
* 检查您运行 SillyTavern 的终端窗口中的日志
* 检查浏览器开发者工具中的控制台

## 更改提示词的构建方式

以正确的方式向 AI 呈现提示词的所有部分对于获得最佳响应至关重要。您可以控制提示词的构建方式。

+++ 文本补全 API

使用[高级格式化](advancedformatting.md)面板自定义文本补全 API 的提示词构建。

+++ 聊天补全 API

使用[提示词管理器](prompt-manager.md)自定义聊天补全 API 的提示词构建。

+++

## 主提示词（系统提示词）

主提示词（或系统提示词）定义了模型要遵循的一般指令。它为对话设定了基调和上下文。例如，它告诉模型作为 AI 助手、写作伙伴或虚构角色行事。

+++ 文本补全 API

[系统提示词](advancedformatting.md#system-prompt)是[故事字符串](context-template.md#story-string)的一部分，通常是模型接收的提示词的第一部分。

+++ 聊天补全 API

主提示词是[提示词管理器](prompt-manager.md)中的默认提示词之一。它通常是模型接收的上下文中的第一条消息，归属于（"由...发送"）系统角色。

+++

默认的主提示词是：

> Write \{\{char\}\}'s next reply in a fictional chat between \{\{char\}\} and \{\{user\}\}.

\{\{char\}\} 和 \{\{user\}\} 占位符会替换为您在对话中定义的角色和人设的名称。

您可以在主提示词中使用任何支持的 [\{\{宏\}\}](/Usage/Characters/macros.md) 标签来包含可能在对话之间变化或随着对话进展而变化的信息。

### 调整主提示词

默认的主提示词帮助模型理解它应该如何处理后续的角色和人设信息，如何解释过去的对话，以及应该生成什么样的响应。这是一个灵活的通用提示词，适用于许多情况，因为它确立了 AI 正在作为与您的人设对话的角色进行写作。

但是，您可以调整主提示词以更好地满足您的需求。以下是调整主提示词的一些常见原因：

* **提供额外的指令**：例如，您希望 AI 解释其推理、遵循特定规则或避免某些主题
* **澄清 AI 的角色**：例如，您希望 AI 作为叙述者、讲故事的人或向导行事
* **更改对话的上下文**：例如，您希望 AI 的响应就像它是 AI 助手、文字冒险游戏或写作伙伴一样

!!! 尝试不同的方法，看看什么最适合您
本指南中的所有示例都对其他用户效果很好，但适合您的需求和您使用的模型的提示词可能有所不同。尝试不同的指令和提示风格，看看什么最适合您。如果您不确定要尝试什么，您可以随时在 [SillyTavern Discord](https://discord.gg/sillytavern) 中寻求帮助。
!!!

在主提示词中为 AI 提供额外的指令可以帮助它理解您想要从对话中得到什么。

> Write one reply only. Write at least one paragraph, up to four.

> Markdown is enabled. Use it to format your response. Enclose code snippets in triple backticks.

> Write character dialogue in quotation marks. Write \{\{char\}\}'s thoughts in parentheses.

> You are an anime roleplay generation model for users aged 13 to 17. You always generate fun, age-appropriate responses.

> Answer truthfully and write out your thinking step by step to be sure you get the right answer.

AI 更容易遵循关于它应该做什么的指令，而不是它不应该做什么的指令。例如，如果您希望 AI 避免以某种方式写作，最好告诉它您希望它如何写作。虽然 *"Do not decide what \{\{user\}\} says or does"* 通常包含在提示词中以防止 AI 控制您的人设，但一些用户发现 *"Write \{\{char\}\}'s responses in a way that respects \{\{user\}\}'s autonomy"* 更有效。

通常有比主提示词更好的地方来包含有关用户或角色的信息、修改角色的写作和说话风格或提供其他特定指令。主提示词最适合用于关于整个对话的一般指令，或关于您想要进行的对话类型的指令。

### 消息历史的影响

在调整主提示词以改善 AI 的响应时，请考虑 AI 从消息历史中获取了很多信息。历史是它对过去事件、角色互动和关系的记忆，以及它的词汇选择和写作风格的风格指南。

通过提供[对话示例](/Usage/Characters/characterdesign.md#examples-of-dialogue)来展示您希望 AI 如何响应，从而利用这一点。展示您想要的内容通常比试图解释它更容易！

当您的对话已经有历史记录时，更改主提示词对 AI 的响应影响有限。就事件和关系而言，AI 假设主提示词发生在遥远的过去，而消息历史会更新它。就写作风格和词汇选择而言，AI 假设历史中的所有消息都是根据*当前*主提示词中的规则生成的，并且它应该继续以相同的方式生成消息。处理此问题的一些建议包括：

* 在消息历史结束附近或之后插入当前指令，例如通过使用[作者注释](/Usage/Characters/Author's-Note.md)
* 通过开始新对话来测试对主提示词的更改
* 编辑消息历史以删除或纠正不需要的行为示例
* 使用[历史后指令](#post-history-instructions)向 AI 提供最终指令

!!! 第一次就做对！
永远不要让 AI"逃脱"您不希望它做的事情。如果您不喜欢 AI 的响应，不要像它是正确的那样继续对话。相反，修改提示词，重新生成消息，然后从那里继续。这将帮助 AI 学习您想要什么。
!!!

### 删除"虚构聊天"上下文

在某些情况下，"虚构聊天"可能不是您对话的正确上下文。

您可以从主提示词中删除"虚构"上下文：

> Write \{\{char\}\}'s next reply in a conversation with \{\{user\}\}.

您可能不希望 AI 认为自己在角色扮演。您可以删除角色的想法，而不是删除 AI 的想法：

> You are \{\{char\}\}, a helpful assistant. You provide useful information and help \{\{user\}\} with their questions.

### AI 作为叙述者或讲故事的人

如果您希望 AI 作为叙述者，从全知的角度描述事件，创造自己的角色和设定，该怎么办？

一种方法是为 AI 创建一个命名角色作为叙述者。此角色可以称为"叙述者"或"AI"，表明 AI 是通用讲故事的人，或者可以根据特定场景或设定命名，赋予 AI 在该设定中叙述故事的任务。然后可以在[角色](/Usage/Characters/characterdesign.md)或[世界信息](/Usage/worldinfo.md)中定义设定的详细信息。

您需要调整默认的主提示词以反映 AI 的角色。对于通用叙述者，您可以使用：

> You are \{\{char\}\}, a skilled and versatile storyteller. Narrate the story.

或者对于特定设定：

> You are the narrator of a fantasy scenario. Play as the characters that visit \{\{char\}\}.

澄清用户在对话中的角色很有帮助。您的消息是故事的一部分，还是关于您的角色做什么或说什么的叙述者指令？包含用户在故事中的示例：

> The story should progress by responding to the actions and dialogue of \{\{user\}\}. Narrate the story in third person.

将用户排除在故事之外的示例：

> Enter Adventure Mode. Narrate the story based on \{\{user\}\}'s dialogue and actions after ">". Describe the surroundings in vivid detail. Be detailed, creative, verbose, and proactive. Move the story forward by introducing fantasy elements and interesting characters.

定义用户的角色不仅有助于 AI 理解如何响应您的消息，而且有助于了解它在多大程度上被允许控制您的人设。这避免了 AI 为您的人设做出您宁愿自己做的决定的情况。

## 历史后指令

历史后指令（PHI）是在主提示词和用户消息之后发送给 AI 的额外指令。它们可用于根据消息历史向 AI 提供额外的上下文或指令。

由于历史后指令是在用户消息之后发送的，因此它们是 AI 在生成响应之前接收的最终指令。AI 通常赋予它们比主提示词更高的优先级，它们可以覆盖主提示词的指令。

要使用每个角色的历史后指令，请将它们添加到角色的[历史后指令](/Usage/Characters/characterdesign.md)中并启用[优先使用角色指令](/Usage/User_Settings/index.md)。要在使用特定于角色的指令的同时保留全局定义的 PHI，您可以在角色的历史后指令字段中使用 `{{original}}` 宏。

+++ 文本补全 API

历史后指令在系统提示词类别下的[高级格式化](/Usage/Prompts/advancedformatting.md)面板中定义。历史后指令被添加为不可见的用户角色注入，位于提示词的最后一行（通常包含响应消息"标题"）之前。请注意，必须启用"启用系统提示词"切换才能应用历史后指令（即使系统提示词本身为空）。

+++ 聊天补全 API

历史后指令是[提示词管理器](prompt-manager.md)中的默认提示词之一。它通常是模型接收的上下文中的最后一条消息，归属于（"由...发送"）系统角色。如果您的聊天补全 API 不支持系统角色，它通常会归属于用户角色。

+++

## 添加到提示词（世界信息）

您可以使用[世界信息](/Usage/worldinfo.md)功能在提示词的任何位置插入额外信息。通过设置何时应插入信息的条件，您可以引导 AI 包含特定详细信息、更改其响应方式或向对话添加新元素。

世界信息的一些常见用途包括：

* 包含有关世界或设定信息的"传说书"或"百科全书"
* 为各种角色和情况管理不同系统提示词的方法
* 存储 AI 应在对话中"回忆"的记忆的地方
* 用于创建、编辑和共享角色详细信息的更模块化的系统
* AI 要反应的随机事件和惊喜的来源，或让您做出反应！
