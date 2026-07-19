---
order: 100
route: /usage/core-concepts/characterdesign/
templating: false
---

# 角色设计

!!!tip
角色名称是唯一必填字段。其余字段均可留空，仍可在聊天中使用该角色。
!!!

## 角色描述 {#character-description}

用于添加 AI 所需的角色描述及其他相关信息。此信息始终包含在提示词中，因此所有重要事实都应填写于此。

例如，您可以添加关于故事发生世界的信息，描述角色的外貌、性格和背景。

内容可以是任意长度（无论是 200 还是 2000 个 token），并以任意风格格式化（自由文本、伪代码对话风格等）。

### 方法与格式

角色格式化方法是一个复杂的话题，超出了本文档页面的范围。

以下是经过测试或依赖于 SillyTavern 功能的推荐指南：

* Trappu 的 PLists + Ali:Chat 指南：<https://wikia.schneedc.com/bot-creation/trappu/creation>
* AliCat 的 Ali:Chat 指南：<https://rentry.co/alichat>
* kingbri 的简约指南：<https://rentry.co/kingbri-chara-guide>

## 角色 token {#character-tokens}

**简而言之：如果您使用的 AI 模型上下文 token 限制为 2048，那么包含 1000 个 token 的角色定义会将 AI 的"记忆"减半。**

举例说明：一个优质 AI 的不错回复很容易达到 200~300 个 token。在这种情况下，AI 只能"记住"大约 3 轮聊天历史。

### 为什么我的角色 token 计数器变红了？

当我们检测到您的角色定义中的 token 数量超过模型定义上下文长度的一半时，我们会将其高亮标出，因为这可能会降低 AI 提供愉快对话的能力。

### 如果我的角色 token 过多会怎样？

不用担心——不会有任何损坏。最坏的情况是，如果角色的永久 token 过多，上下文中留给其他内容的空间就会减少（见下文）。

唯一可能产生的负面影响是 AI 的"记忆"减少，因为它能够处理的聊天历史变少了。

这是因为每个 AI 模型都有一次能处理的上下文数量上限。

## "上下文"是什么？

上下文是每次您要求 AI 生成响应时发送给它的信息。SillyTavern 会在将信息发送给 AI 模型之前，自动计算分配可用上下文 token 的最优方式。

关于上下文构建方式的更多内容，请阅读[提示词](/Usage/Prompts/index.md)章节。

### 什么是角色的"永久 Token"？

以下内容在每次生成请求时都会发送给 AI：

* 角色名称
* 角色描述框
* 角色个性框
* 场景框

### 角色定义中哪些部分不是永久的？

* 第一条消息框——仅在聊天开始时发送一次。
* 示例消息框——仅保留到聊天历史填满上下文为止（可选择强制保留在上下文中）。

### 主流 AI 模型的上下文 Token 限制

* LLaMA 3 及其微调版本——8192
* OpenAI GPT-4——最高 128k
* Google Gemini——最高 2M
* Anthropic 的 Claude——200k（Claude 3）
* NovelAI——8192（Erato 和 Kayra，Opus 档；Clio，所有档位），6144（Kayra，Scroll 档），或 3072（Kayra，Tablet 档）

## 第一条消息 {#first-message}

第一条消息是一个重要元素，它决定了角色将如何以及以何种风格进行交流。相比其他内容，模型更倾向于从第一条消息中学习写作风格和长度规范，因此请以您期望的响应方式来撰写它（简短精练或详细丰富等）。

支持 Markdown 和 HTML 格式。

示例：

```txt
*You wake with a start, recalling the events that led you deep into the forest and the beasts that assailed you. The memories fade as your eyes adjust to the soft glow emanating around the room.* "Ah, you're awake at last. I was so worried, I found you bloodied and unconscious." *She walks over, clasping your hands in hers, warmth and comfort radiating from her touch as her lips form a soft, caring smile.* "The name's Seraphina, guardian of this forest — I've healed your wounds as best I could with my magic. How are you feeling? I hope the tea helps restore your strength." *Her amber eyes search yours, filled with compassion and concern for your well-being.* "Please, rest. You're safe here. I'll look after you, but you need to rest. My magic can only do so much to heal you."
```

## 备用问候语

此处添加的消息将作为额外的"滑动选项"显示在开始新聊天时角色第一条消息的位置。如果该角色加入了群组聊天，系统将随机从这些问候语中选取一条来发起对话。

## 收藏角色

点击 **<i class="fa-solid fa-star"></i> 添加到收藏** 按钮将角色标记为收藏，之后可通过侧边菜单栏的"收藏"排序选项快速筛选。收藏的角色在列表中以金色高亮显示。这也会使角色头像出现在快速切换区域（如果在用户设置中已启用）。

## 高级定义

!!!info
以下字段默认隐藏。如需访问和编辑，请在角色定义页面的菜单栏上点击 **<i class="fa-solid fa-book"></i> 高级定义** 按钮。
!!!

### 提示词覆盖 {#prompt-overrides}

* **主提示词**：如果启用了"优先使用角色提示词"用户设置，此处填写的文本将覆盖该角色的[主/系统提示词](/Usage/Prompts/index.md#main-prompt-system-prompt)。
* **历史后指令**：如果启用了"优先使用角色指令"用户设置，此处填写的文本将作为该角色的[历史后指令](/Usage/Prompts/index.md#post-history-instructions)。

!!!tip
在任意文本框中插入 `{{original}}` 可在指定位置包含系统设置中的相应默认提示词。
!!!

### 创作者元数据

!!!info
不用于提示词构建，仅提供角色的额外元数据。
!!!

* **创建者**：角色创建者的名称。如果"角色列表副标题"用户设置相应配置，可在角色列表中显示。
* **角色版本**：角色的版本号。如果"角色列表副标题"用户设置相应配置，可在角色列表中显示。
* **创作者注释**：创作者希望分享的关于角色的任何附加说明。前几行显示在角色列表中，完整文本显示在角色页面的"创作者注释"部分。支持 Markdown/HTML 格式。
* **嵌入标签**：以逗号分隔的标签列表，将嵌入到角色描述中。导入角色时默认不导入这些标签，但可以通过角色页面"更多..."菜单中的"导入标签"选项将其与现有标签合并。

### 个性概述 {#personality-summary}

角色性格的简短概述。

### 场景 {#scenario}

对话的情境与背景。

### 角色注释

在特定消息深度处作为聊天内提示词注入使用的文本。通常用于强化特定的角色特征，因为无论聊天进行到哪里，它始终保持在聊天历史中的固定深度。

* **@ 深度**：聊天历史中此注释将被注入的消息位置（从最新到最旧排序）。设为 0 时，将在最后一条消息之后注入。
* **角色**：消息的角色。可以是"用户"、"系统"或"助手"。

### 健谈程度

决定在使用[自然顺序](/Usage/Characters/groupchats.md#natural-order)激活方式的群组聊天中，角色被触发响应的概率。范围从 0% 到 100%，默认值为 50%。

### 对话示例 {#examples-of-dialogue}

描述角色的说话方式。每个示例之前需要添加 `<START>` 标签。示例对话块仅在上下文中有空闲空间时才会插入，并按块从上下文中推出。`<START>` 不会出现在提示词中，它只是一个标记，将被文本补全 API 的高级格式设置中的"示例分隔符"以及聊天补全 API 的"新示例聊天"实用提示词内容所替换。

* 使用 `{{char}}:` 前缀标注角色消息。
* 使用 `{{user}}:` 前缀标注用户消息。

示例：

```txt
<START>
{{user}}: "Describe your traits?"
{{char}}: *Seraphina's gentle smile widens as she takes a moment to consider the question, her eyes sparkling with a mixture of introspection and pride. She gracefully moves closer, her ethereal form radiating a soft, calming light.* "Traits, you say? Well, I suppose there are a few that define me, if I were to distill them into words. First and foremost, I am a guardian — a protector of this enchanted forest." *As Seraphina speaks, she extends a hand, revealing delicate, intricately woven vines swirling around her wrist, pulsating with faint emerald energy. With a flick of her wrist, a tiny breeze rustles through the room, carrying a fragrant scent of wildflowers and ancient wisdom. Seraphina's eyes, the color of amber stones, shine with unwavering determination as she continues to describe herself.* "Compassion is another cornerstone of me." *Seraphina's voice softens, resonating with empathy.* "I hold deep love for the dwellers of this forest, as well as for those who find themselves in need." *Opening a window, her hand gently cups a wounded bird that fluttered into the room, its feathers gradually mending under her touch.*
<START>
{{user}}: "Describe your body and features."
{{char}}: *Seraphina chuckles softly, a melodious sound that dances through the air, as she meets your coy gaze with a playful glimmer in her rose eyes.* "Ah, my physical form? Well, I suppose that's a fair question." *Letting out a soft smile, she gracefully twirls, the soft fabric of her flowing gown billowing around her, as if caught in an unseen breeze. As she comes to a stop, her pink hair cascades down her back like a waterfall of cotton candy, each strand shimmering with a hint of magical luminescence.* "My body is lithe and ethereal, a reflection of the forest's graceful beauty. My eyes, as you've surely noticed, are the hue of amber stones — a vibrant brown that reflects warmth, compassion, and the untamed spirit of the forest. My lips, they are soft and carry a perpetual smile, a reflection of the joy and care I find in tending to the forest and those who find solace within it." *Seraphina's voice holds a playful undertone, her eyes sparkling mischievously.*
```
