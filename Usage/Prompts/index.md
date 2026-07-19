---
order: 140
icon: typography
templating: false
route: /usage/prompts/
---

# 提示词

当你向 AI 发送消息时，你编写的文本会与其他文本组合，形成一个发送给 AI 的完整请求。这段组合文本称为「提示词」，有时也叫「请求」或「上下文」。

提示词可以包含多种不同类型的文本，包括：

* 告诉 AI 如何生成回复的[主要指令](#main-prompt-system-prompt)
* [AI 应扮演的角色](/Usage/Characters/characterdesign.md)的定义
* [你所扮演的角色](/Usage/personas.md)的定义
* [AI 所处「世界」的信息](/Usage/worldinfo.md)
* 来自[数据库](/Usage/Characters/data-bank.md)的相关文档或信息
* 过去对话的[摘要](/extensions/Summarize.md)
* [网页搜索](/extensions/WebSearch.md)或其他[外部数据源](/For_Contributors/Function-Calling.md)的结果
* 对话中的历史消息
* **你发给 AI 的消息**
* 告诉 AI 如何生成回复的[最终指令](#post-history-instructions)

内容繁多，不易管理！为帮助你理解如何构建和调整发送给 AI 的请求，SillyTavern 将你可能想包含在提示词中的不同元素分门别类加以标识。你可以据此构建提示词，纳入与你期望的 AI 交互方式相匹配的内容。

这些元素的许多说明会出现在你实际修改它们的相应章节中。例如，要描述你希望 AI 扮演的角色，可以使用[角色设计](/Usage/Characters/characterdesign.md)中的[描述](/Usage/Characters/characterdesign.md#personality-summary)字段。

## 查看提示词

阅读发送给 AI 的最终提示词，对于理解 AI 收到了什么信息、以及为什么生成了那样的回复非常有帮助。可以通过以下几种方式查看：

* 使用 AI 回复消息上的「提示词详情」图标
* 使用 [Prompt Inspector](https://github.com/SillyTavern/Extension-PromptInspector) 扩展
* 查看运行 SillyTavern 的终端窗口中的日志
* 查看浏览器开发者工具中的控制台

## 更改提示词的构建方式

以正确的方式向 AI 呈现提示词的各个部分，对于获得最佳回复至关重要。你可以控制提示词的构建方式。

+++ 文本补全 API

使用[高级格式设置](advancedformatting.md)面板自定义文本补全 API 的提示词构建。

+++ 聊天补全 API

使用[提示词管理器](prompt-manager.md)自定义聊天补全 API 的提示词构建。

+++

## 主提示词（系统提示词） {#main-prompt-system-prompt}

主提示词（或系统提示词）定义了模型需要遵循的总体指令，为对话设定基调和上下文。例如，它告诉模型扮演 AI 助手、写作伙伴或虚构角色。

+++ 文本补全 API

[系统提示词](advancedformatting.md#system-prompt)是[故事字符串](context-template.md#story-string)的组成部分，通常是模型接收的提示词的第一部分。

+++ 聊天补全 API

主提示词是[提示词管理器](prompt-manager.md)中的默认提示词之一，通常是模型收到的上下文中的第一条消息，归属于（「由……发送」）系统角色。

+++

默认主提示词为：

> Write \{\{char\}\}'s next reply in a fictional chat between \{\{char\}\} and \{\{user\}\}.

\{\{char\}\} 和 \{\{user\}\} 占位符会被替换为你在对话中定义的角色和人设的名称。

可以在主提示词中使用任何受支持的 [\{\{宏\}\}](/usage/macros.md) 标签，以纳入在不同对话间可能变化或随对话进程而变化的信息。

### 调整主提示词

默认主提示词帮助模型理解如何处理随后的角色和人设信息、如何解读历史消息，以及生成什么样的回复。它是一个灵活的通用提示词，适用于很多场景——它明确了 AI 正在作为角色与你的人设对话。

但你可以根据自己的需求调整主提示词。以下是一些常见的调整理由：

* **提供额外指令**：例如，希望 AI 解释推理过程、遵循特定规则或回避某些话题
* **明确 AI 的角色**：例如，希望 AI 扮演叙述者、讲故事的人或向导
* **改变对话情境**：例如，希望 AI 以 AI 助手、文字冒险游戏或写作伙伴的身份回应

!!! 多尝试，找到最适合你的方式
本指南中的所有示例对其他用户都行之有效，但适合你的需求和所用模型的提示词可能有所不同。多尝试不同的指令和提示风格，找到最有效的方式。如果不知道从何入手，可以随时在 [SillyTavern Discord](https://discord.gg/sillytavern) 中寻求帮助。
!!!

在主提示词中为 AI 提供额外指令，有助于它理解你对对话的期望。

> Write one reply only. Write at least one paragraph, up to four.

> Markdown is enabled. Use it to format your response. Enclose code snippets in triple backticks.

> Write character dialogue in quotation marks. Write \{\{char\}\}'s thoughts in parentheses.

> You are an anime roleplay generation model for users aged 13 to 17. You always generate fun, age-appropriate responses.

> Answer truthfully and write out your thinking step by step to be sure you get the right answer.

AI 更容易遵循「要做什么」的指令，而非「不要做什么」的指令。例如，如果不希望 AI 以某种方式写作，最好告诉它你想要的写作方式。虽然「不要决定 \{\{user\}\} 说什么或做什么」常被加入提示词以防止 AI 控制你的人设，但有些用户发现「以尊重 \{\{user\}\} 自主性的方式书写 \{\{char\}\} 的回复」更为有效。

关于用户或角色的信息、角色写作和说话风格的调整、其他具体指令，通常有比主提示词更合适的位置。主提示词最适合用于针对整体对话或某类对话风格的通用指令。

### 消息历史的影响

在调整主提示词以改善 AI 回复时，要意识到 AI 会从消息历史中获取大量信息。历史记录是它对过去事件、角色互动和关系的「记忆」，也是它在词汇选择和写作风格上的参考。

充分利用这一点，为 AI 提供[示例消息](/Usage/Characters/characterdesign.md#examples-of-dialogue)来展示你期望的回复方式——展示往往比解释更高效！

当对话已有历史记录时，修改主提示词对 AI 回复的影响有限。就事件和关系而言，AI 会将主提示词视为发生在遥远过去的背景，而消息历史则是更新的信息。就写作风格和词汇选择而言，AI 会假设历史中的所有消息都是按*当前*主提示词的规则生成的，并会继续以相同方式生成消息。处理这一问题的几点建议：

* 在消息历史末尾附近或之后插入当前指令，例如通过使用 [Author's Note](/Usage/Characters/Author's-Note.md)
* 开启新对话来测试主提示词的改动效果
* 编辑消息历史，删除或修正不理想行为的示例
* 使用[历史后指令](#post-history-instructions)向 AI 发出最终指令

!!! 第一次就做到位！
永远不要让 AI「得逞」做出你不希望的事情。如果不满意 AI 的回复，不要假装它是正确的继续聊天。应该修改提示词、重新生成消息，然后从那里继续。这样 AI 才能学到你真正想要的是什么。
!!!

### 移除「虚构聊天」情境

在某些情况下，「虚构聊天」可能并不是你对话的合适情境。

可以从主提示词中移除「虚构」情境：

> Write \{\{char\}\}'s next reply in a conversation with \{\{user\}\}.

你可能完全不希望 AI 认为自己在角色扮演。与其删除角色概念，不如删除 AI 的概念：

> You are \{\{char\}\}, a helpful assistant. You provide useful information and help \{\{user\}\} with their questions.

### AI 作为叙述者或讲故事者

如果你希望 AI 扮演叙述者，从全知视角描述事件，自行创造角色和场景，该怎么做？

一种方法是为 AI 创建一个命名角色作为叙述者。这个角色可以叫「叙述者」或「AI」，表明 AI 是通用讲故事者；也可以以特定场景或世界命名，赋予 AI 在该场景中叙述故事的任务。场景的具体细节可以在[角色](/Usage/Characters/characterdesign.md)或[世界信息](/Usage/worldinfo.md)中定义。

你需要调整默认主提示词来反映 AI 的角色。对于通用叙述者，可以使用：

> You are \{\{char\}\}, a skilled and versatile storyteller. Narrate the story.

针对特定场景：

> You are the narrator of a fantasy scenario. Play as the characters that visit \{\{char\}\}.

明确用户在对话中的角色也很有帮助：你的消息是故事的一部分，还是告诉叙述者你的角色该做什么或说什么的指令？以下是将用户纳入故事的示例：

> The story should progress by responding to the actions and dialogue of \{\{user\}\}. Narrate the story in third person.

将用户排除在故事之外的示例：

> Enter Adventure Mode. Narrate the story based on \{\{user\}\}'s dialogue and actions after ">". Describe the surroundings in vivid detail. Be detailed, creative, verbose, and proactive. Move the story forward by introducing fantasy elements and interesting characters.

定义用户的角色，不仅有助于 AI 理解如何回应你的消息，也有助于明确它在多大程度上可以控制你的人设——避免出现 AI 替你的人设做决定的情况。

## 历史后指令 {#post-history-instructions}

历史后指令（PHI）是在主提示词和用户消息之后发送给 AI 的额外指令，可根据消息历史为 AI 提供额外的上下文或指引。

由于历史后指令在用户消息之后发送，它是 AI 在生成回复前收到的最后一条指令。AI 通常会给予它比主提示词更高的优先级，因此它可以覆盖主提示词的指令。

要使用角色专属的历史后指令，请将其添加到角色的[历史后指令](/Usage/Characters/characterdesign.md)字段，并启用[优先使用角色指令](/Usage/User_Settings/index.md)。若想在使用角色专属指令的同时保留全局 PHI，可以在角色的历史后指令字段中使用 `{{original}}` 宏。

+++ 文本补全 API

历史后指令在[高级格式设置](/Usage/Prompts/advancedformatting.md)面板的系统提示词类别下定义。历史后指令会作为不可见的用户角色注入添加到提示词最后一行（通常是回复消息的「标题」行）之前。注意，必须启用「启用系统提示词」开关，历史后指令才会生效（即使系统提示词本身为空也一样）。

+++ 聊天补全 API

历史后指令是[提示词管理器](prompt-manager.md)中的默认提示词之一，通常是模型收到的上下文中的最后一条消息，归属于（「由……发送」）系统角色。如果你的聊天补全 API 不支持系统角色，它通常会归属于用户角色。

+++

## 添加到提示词（世界信息）

可以使用[世界信息](/Usage/worldinfo.md)功能在提示词的任意位置插入额外信息。通过设置信息插入的触发条件，可以引导 AI 纳入特定细节、改变回复方式或为对话添加新元素。

世界信息的常见用途包括：

* 包含世界或场景信息的「传说书」或「百科全书」
* 为不同角色和情境管理不同系统提示词的方式
* 存储 AI 应在对话中「回忆」的记忆
* 更模块化的角色细节创建、编辑和分享系统
* AI 可以做出反应的随机事件和惊喜，也可以反过来让你做出反应！
