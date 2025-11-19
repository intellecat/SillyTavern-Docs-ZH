---
order: 100
route: /usage/core-concepts/characterdesign/
templating: false
---

# Character 设计

!!!tip
Character Name 是唯一必填字段。您可以将其余部分留空，仍然可以在聊天中使用该 character。
!!!

## Character Description

用于添加 character 描述和其他相关信息供 AI 使用。此信息始终包含在 prompt 中，因此所有重要事实都应包含在此处。

例如，您可以添加关于行动发生的世界的信息，描述 character 的外观、性格和背景。

它可以是任意长度（无论是 200 还是 2000 tokens）并以任何风格格式化（自由文本、伪代码对话风格等）。

### 方法和格式

Character 格式化方法是一个复杂的主题，超出了本文档页面的范围。

经过测试或依赖 SillyTavern 功能的推荐指南：

* Trappu 的 PLists + Ali:Chat 指南：<https://wikia.schneedc.com/bot-creation/trappu/creation>
* AliCat 的 Ali:Chat 指南：<https://rentry.co/alichat>
* kingbri 的极简主义指南：<https://rentry.co/kingbri-chara-guide>

## Character tokens

**简而言之：如果您使用的 AI 模型具有 2048 context token 限制，1000-token 的 character 定义会将 AI 的"记忆"减半。**

从这个角度来看，来自优秀 AI 的体面响应可以轻松达到 200-300 tokens 左右。在这种情况下，AI 只能"记住"大约 3 次交换的聊天历史。

### 为什么我的 character 的 token 计数器变红了？

当我们看到您的 character 在其定义中拥有超过模型定义的 context 长度的一半的 tokens 时，我们会为您突出显示它，因为这可能会降低 AI 提供愉快对话的能力。

### 如果我的 Character 有太多 tokens 会怎么样？

别担心 - 它不会破坏任何东西。最坏的情况下，如果 Character 的永久 tokens 太大，这只是意味着 context 中其他内容的空间会减少（见下文）。

这可能产生的唯一负面副作用是 AI 的"记忆"会减少，因为它可以处理的聊天历史会更少。

这是因为每个 AI 模型都有一次可以处理的 context 量的限制。

## 'Context'？

这是每次您要求 AI 生成响应时发送给 AI 的信息。SillyTavern 会在将信息发送到 AI 模型之前自动计算分配可用 context tokens 的最佳方式。

在 [Prompts](/Usage/Prompts/index.md) 部分阅读更多关于如何构建 context 的信息。

### Character 的"永久 Tokens"是什么？

这些将始终随每个生成请求发送给 AI：

* Character Name
* Character Description Box
* Character Personality Box
* Scenario Box

### Character 定义的哪些部分不是永久的？

* First message box - 仅在聊天开始时发送一次。
* Example messages box - 仅保留直到聊天历史填满 context（可选地，这些可以强制保留在 context 中）

### 流行的 AI 模型 Context Token 限制

* LLaMA 3 及其微调版本 - 8192
* OpenAI GPT-4 - 最高 128k
* Google Gemini - 最高 2M
* Anthropic 的 Claude - 200k（Claude 3）
* NovelAI - 8192（Erato 和 Kayra，Opus 层级；Clio，所有层级），6144（Kayra，Scroll 层级），或 3072（Kayra，Tablet 层级）

## First message

First Message 是一个重要元素，它定义了 character 将如何以及以什么风格进行交流。模型最有可能从 first message 而不是其他任何内容中获取风格和长度约束，因此以您希望响应的方式编写它很重要（简短而简洁，长而详细等）。

支持 Markdown 和 HTML 格式。

例如：

```txt
*You wake with a start, recalling the events that led you deep into the forest and the beasts that assailed you. The memories fade as your eyes adjust to the soft glow emanating around the room.* "Ah, you're awake at last. I was so worried, I found you bloodied and unconscious." *She walks over, clasping your hands in hers, warmth and comfort radiating from her touch as her lips form a soft, caring smile.* "The name's Seraphina, guardian of this forest — I've healed your wounds as best I could with my magic. How are you feeling? I hope the tea helps restore your strength." *Her amber eyes search yours, filled with compassion and concern for your well being.* "Please, rest. You're safe here. I'll look after you, but you need to rest. My magic can only do so much to heal you."
```

## Alternate Greetings

在此处添加的消息在开始新聊天时显示为 character 的 first message 的额外"滑动"选项。如果 character 是 group chat 的一部分，系统会随机选择其中一个问候语来开始对话。

## Favorite Character

点击 **<i class="fa-solid fa-star"></i> Add to Favorites** 按钮将 character 标记为收藏，以便通过选择"Favorites"排序选项在侧边菜单栏上快速筛选它们。收藏的 characters 在列表中有金色高亮显示。这也会使 character 肖像出现在 hotswaps 区域中（如果在 User Settings 中启用）。

## Advanced Definitions

!!!info
以下字段默认隐藏。要访问和编辑它们，您需要点击 character 定义页面菜单栏上的 **<i class="fa-solid fa-book"></i> Advanced Definitions** 按钮。
!!!

### Prompt 覆盖

* **Main Prompt**：如果启用了"Prefer Char. Prompt" user setting，您在此处输入的任何文本都将覆盖 character 的 [main/system prompt](/Usage/Prompts/index.md#main-prompt-system-prompt)。
* **Post-History Instructions**：如果启用了"Prefer Char. Instructions" user setting，您在此处输入的任何文本都将用作 character 的 [post-history instructions](/Usage/Prompts/index.md#post-history-instructions)。

!!!tip
在任一框中插入 `{{original}}` 以在指定位置包含系统设置中的相应默认 prompt。
!!!

### 创作者元数据

!!!info
不用于 prompt 构建，但提供有关 character 的额外元数据。
!!!

* **Created by**：character 创作者的姓名。如果"Char List Subheader" user setting 相应设置，可以在 character 列表中显示。
* **Character Version**：character 的版本。如果"Char List Subheader" user setting 相应设置，可以在 character 列表中显示。
* **Creator's Notes**：创作者希望分享的有关 character 的任何附加说明。前几行显示在 character 列表中，完整文本显示在 character 页面的"Creator's Notes"部分。支持 Markdown/HTML 格式。
* **Tags to Embed**：将嵌入到 character 描述中的逗号分隔的 tags 列表。导入 character 时默认不导入这些 tags，但您可以通过从 character 页面的"More..."菜单中选择"Import Tags"将它们与现有 tags 合并。

### Personality summary

character 性格的简要摘要。

### Scenario

对话的环境和背景。

### Character's Note

用作特定消息深度处的 character 聊天内 prompt 注入的文本。它通常用于强化某些 character 特征，因为无论聊天进展如何，它始终保持在聊天历史中的静态深度。

* **@ Depth**：注入此 note 之后的聊天历史中的消息数（按从最新到最旧的顺序）。如果设置为 0，它将在最后一条消息之后注入。
* **Role**：消息的角色。可以是"User"、"System"或"Assistant"。

### Talkativeness

确定使用 [Natural](/Usage/Characters/groupchats.md#natural-order) 激活顺序时 character 响应在 group chats 中被触发的概率。范围从 0% 到 100%，默认值为 50%。

### Examples of dialogue

描述 character 如何说话。在每个示例之前，您需要添加 `<START>` 标签。示例对话块仅在 context 中有空闲空间时才会插入，并按块从 context 中推出。`<START>` 不会出现在 prompt 中，因为它只是一个标记；对于 Text Completion APIs，它将被 Advanced Formatting 中的"Example Separator"替换，对于 Chat Completion APIs，它将被"New Example Chat" utility prompt 的内容替换。

* 使用 `{{char}}:` 前缀表示 character 消息。
* 使用 `{{user}}:` 前缀表示 user 消息。

例如：

```txt
<START>
{{user}}: "Describe your traits?"
{{char}}: *Seraphina's gentle smile widens as she takes a moment to consider the question, her eyes sparkling with a mixture of introspection and pride. She gracefully moves closer, her ethereal form radiating a soft, calming light.* "Traits, you say? Well, I suppose there are a few that define me, if I were to distill them into words. First and foremost, I am a guardian — a protector of this enchanted forest." *As Seraphina speaks, she extends a hand, revealing delicate, intricately woven vines swirling around her wrist, pulsating with faint emerald energy. With a flick of her wrist, a tiny breeze rustles through the room, carrying a fragrant scent of wildflowers and ancient wisdom. Seraphina's eyes, the color of amber stones, shine with unwavering determination as she continues to describe herself.* "Compassion is another cornerstone of me." *Seraphina's voice softens, resonating with empathy.* "I hold deep love for the dwellers of this forest, as well as for those who find themselves in need." *Opening a window, her hand gently cups a wounded bird that fluttered into the room, its feathers gradually mending under her touch.*
<START>
{{user}}: "Describe your body and features."
{{char}}: *Seraphina chuckles softly, a melodious sound that dances through the air, as she meets your coy gaze with a playful glimmer in her rose eyes.* "Ah, my physical form? Well, I suppose that's a fair question." *Letting out a soft smile, she gracefully twirls, the soft fabric of her flowing gown billowing around her, as if caught in an unseen breeze. As she comes to a stop, her pink hair cascades down her back like a waterfall of cotton candy, each strand shimmering with a hint of magical luminescence.* "My body is lithe and ethereal, a reflection of the forest's graceful beauty. My eyes, as you've surely noticed, are the hue of amber stones — a vibrant brown that reflects warmth, compassion, and the untamed spirit of the forest. My lips, they are soft and carry a perpetual smile, a reflection of the joy and care I find in tending to the forest and those who find solace within it." *Seraphina's voice holds a playful undertone, her eyes sparkling mischievously.*
```
