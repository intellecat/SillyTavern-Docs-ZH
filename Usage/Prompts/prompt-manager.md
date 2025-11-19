---
order: 50
templating: false
route: /usage/prompts/prompt-manager/
---

# Prompt 管理器

Prompt Manager 是一个为 Chat Completion API 提供对 [prompt-building](index.md) 策略更多控制的系统。

!!! Applies to: Chat Completion APIs
对于 Text Completion API 中的等效设置,请使用 [Advanced Formatting](advancedformatting.md)。
!!!

!!!tip Naming Presets
如果预设与你的某个角色卡共享名称,它将在开始与该角色聊天时自动被选中。将预设命名为独特的名称以避免此行为。
!!!

通过点击导航栏中的 "AI Response Configuration" 按钮访问 Prompt Manager。Prompt Manager 位于 [common settings](/Usage/Common-Settings.md) 面板下方。

## Quick Prompts Edit

提供快速编辑常见 prompt 部分的空间,如 **Main Prompt**、**Auxiliary Prompt** 和 **Post-History Instructions**。有关这些 prompt 的更多信息可以在 [prompt-building](index.md) 页面找到。

## Utility Prompts

这些 prompt 被发送到 Chat Completion 模型,以帮助它理解发送给它的信息,或在某些类型的交互中指示它以特定方式行动。

### Format Templates

!!!tip
如果未设置格式模板,信息将按原样发送,不进行任何包裹。
!!!

这些是用于包裹从 [World Info](/Usage/worldinfo.md) 和 [Character Cards](/Usage/Characters/characterdesign.md) 提取的信息的字符串模板。

使用特殊标记来指示应插入信息的位置:

- `{0}` 用于 World Info 格式模板。
- `{{scenario}}` 用于 Scenario 格式模板。
- `{{personality}}` 用于 Personality 格式模板。

### Group Nudge Prompt Template

仅在群组聊天中使用。放置在 prompt 末尾以强制特定角色回复。

将此留空以禁用 Group Nudge 功能。

### New Chat, New Group Chat, New Example Chat

这些在聊天历史之前和每个 [Example Dialogue](/Usage/Characters/characterdesign.md#examples-of-dialogue) 块之前发送,以告知模型背景信息在哪里结束,聊天历史在哪里开始。

- **New Chat:** 用于单独聊天。
- **New Group Chat:** 用于群组聊天。
- **New Example Chat:** 用于示例对话块。

将这些留空以禁用此功能。

### Continue Nudge

在 prompt 末尾发送,以指示模型在触发 Continue 时该做什么,例如当按下 Continue 按钮或由 STScript 触发时。

!!! Chat Completion 'Continues'
请记住,Chat Completion 模型处理 Continue 的方式与 **Text Completion** 模型不同,无论你的 Continue Nudge 如何,可能并不总是提供无缝的结果。
!!!

### Replace Empty Message

当文本框为空且按下 **Send a message** 时,发送此字段的内容而不是空白消息。

## Character Names Behavior

提供不同的策略来指示模型如何将消息与角色关联。如果 Chat Completion 模型在确定哪些消息属于哪个角色时遇到困难,可能需要选择不同的策略。

## Continue Postfix

当触发 Continue 时,模型返回的"续写"消息将在开头添加选定的 Continue Postfix。例如,它可以在续写文本之前添加一个空格。

## Additional Settings

### Wrap in Quotes

!!!warning
已弃用的选项。优先使用 [Regex scripts](/extensions/Regex.md)。
!!!

在发送之前将整个用户消息包裹在隐藏的引号中。这对于角色不使用引号表示语言的会话很有用。如果你的会话使用引号表示语言,请不要选中此项。

### Continue Prefill

!!!warning
可能不适用于所有 Chat Completion 源。
!!!

将 Continue Nudge 作为 Assistant 角色消息而不是 System 消息发送。如果启用此选项,将不使用 Continue Nudge prompt。

### Squash system messages

!!!warning
已弃用的选项。优先使用 [Prompt Post-Processing](/Usage/API_Connections/openai.md#prompt-post-processing)。
!!!

将连续的 System 消息合并为单个组合消息(不包括 Example Dialogue)。

### Enable web search

!!!
不要与 [Web Search extension](/extensions/WebSearch.md) 混淆。
!!!

启用 Chat Completion backend 提供的网络搜索功能。prompt 通常由模型提供商使用搜索结果丰富,可能会产生额外费用。

### Enable function calling

请参阅 [Function Calling](/For_Contributors/Function-Calling.md)

### Send inline images, Send inline videos

!!!
不要与 [Image Captioning extension](/extensions/captioning.md) 混淆。
!!!

如果 Chat Completion 模型具有处理提交的图像和视频的多模态能力,这将切换其执行此操作的能力。要将媒体附加到 prompt,请使用 "Magic Wand" 菜单中的 **Attach A File** 选项。

### Request inline images

!!!
不要与 [Image Generation extension](/extensions/Stable-Diffusion.md) 混淆。
!!!

允许模型返回图像附件。

### Use system prompt

!!!
仅由 Google Gemini 和 Anthropic Claude backend 支持。

尽管这两者有非常相似的设置,但它们在技术上是单独的选项,因此可以分别配置。
!!!

将所有系统消息合并到第一条非系统角色(User/Assistant)消息之前,并将它们作为单独的系统指令字段发送。

## Reasoning Settings

如果 Chat Completion 模型使用推理,这些设置会影响其可见性和功能。

### Request model reasoning

请参阅 [Adding Reasoning: By Backend](/Usage/Prompts/reasoning.md#by-backend)。

### Reasoning Effort

请参阅 [Reasoning Effort](/Usage/Prompts/reasoning.md#reasoning-effort)。

## "Prompts"

Prompt Manager 构成了发送到 Chat Completion 模型的 prompt 的骨干。它控制发送什么以及发送的 *顺序*。

### The 'Prompts' Dropdown

包含当前 Chat Completion 预设包含的所有(非默认)prompt 的下拉列表。要将这些 prompt 之一添加到传出消息中,需要从下拉列表中选择它,然后按 **Insert prompt** 按钮将其添加到 Prompt Manager。要创建一个新的 prompt 添加到此下拉列表,请按 **New prompt** 按钮。一旦新 prompt 被编写并保存,它就会被添加到下拉列表中,然后可以插入。

### Prompts List

这是一个拖放界面,列出了选择可能发送到 Chat Completion 模型的 prompt。放置在界面 **顶部** 附近的 prompt 发送得更早。列表的 **底部** 是发送到模型的 **最后一件事**(通常,这将是你的 **Post-History Instructions**)。

!!! 'Pinned' prompts = Default prompts
默认 prompt 不能从选定的 prompt 列表中删除。这包括 Main Prompt、World Info (before/after)、Persona Description、Character Description、Character Personality、Scenario、Enhance Definitions、Auxiliary Prompt、Chat Examples、Chat History 和 Post-History Instructions。如果不需要这些,可以将它们 **切换为 'OFF'**,但不能直接删除。
!!!

## Editing a Prompt

点击 prompt 上的 **铅笔按钮** 将带你到 **Edit 界面**。在这里,你可以直接编辑 prompt。

!!! Make sure to save your changes!
要将这些 prompt 的更改永久保存到你的 Chat Completion 预设中,你必须点击 **Edit 界面** 右下角的 **Save** 按钮,并通过使用位于 **AI Response Configuration** 部分顶部的 **Save** 按钮保存预设本身!否则,当 Chat Completion 预设切换到不同的预设时,所做的更改将丢失。
!!!

### Name

prompt 的名称。这不会发送到 Chat Completion 模型;它仅供你在 Prompt Manager 中参考。

### Role

哪个角色发送 prompt。你可以在 System、AI Assistant 或 User 之间选择。

### Triggers

发送此 prompt 的生成类型。如果未选择任何内容,则 prompt 将针对所有生成类型发送。如果选择了一个或多个,则 prompt 将仅针对这些特定生成类型发送:

- **Normal:** 常规消息生成请求。
- **Continue:** 当按下 Continue 按钮时。
- **Impersonate:** 当按下 Impersonate 按钮时。
- **Swipe:** 当通过滑动触发生成时。
- **Regenerate:** 当在单独聊天中按下 Regenerate 按钮时。
- **Quiet:** 后台生成请求,通常由 [extensions](/extensions/index.md) 或 [STscript](/For_Contributors/st-script.md) 命令触发。

!!!
"Regenerate" 触发器在群组聊天中不可用,因为它使用不同的再生成逻辑:从最后一次回复中删除所有消息,并根据选择的 [Group reply strategy](/Usage/Characters/groupchats.md#reply-order-strategies) 使用 "Normal" 生成类型排队消息。
!!!

### Position

当 Position 设置为 **Relative** 时,此 prompt 在拖放界面中与所有其他 prompt 一起发送到其所在的位置。当它设置为 **In-Chat** 并给定 **Depth** 时,它会以选定的 Role 在 **Chat History 内** 发送,并 **忽略** 拖放界面的顺序。

### Depth

当 Position 设置为 **In-Chat** 时,这定义了 prompt 在聊天历史中发送的深度。数字越大,发送得越深。例如,Depth 为 0 将在最后一条聊天消息之后发送,Depth 为 1 将在最后一条聊天消息之前发送,Depth 为 2 将在倒数第二条聊天消息之前发送,依此类推。

### Order

!!!
具有相同 Role 和 Depth 的 prompt 将被分组在一起,并按其 Order 值排序。
顺序如下(从上到下):User、AI Assistant、System。
!!!

当 Position 设置为 **In-Chat** 时,这定义了 prompt 在聊天历史中发送的顺序。数字越小,发送得越早。

## Building Your Prompt: Tips and Tricks

访问 SillyTavern 文档的 [prompt-building](index.md) 部分,了解有关如何编写有效 prompt 的更多信息。这些信息在很大程度上可以应用于 Chat Completion 预设。
