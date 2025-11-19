---
route: /usage/api-connections/novelai/
---

# NovelAI

NovelAI 是一个付费订阅服务，允许每月无限制访问他们高质量的内部文本生成、图像生成和文本转语音模型。在此注册账户以开始使用：<https://novelai.net/>

你只能获得 *50 次生成* 的免费试用来评估模型。当出现 **"Not eligible for this model"** 错误时，这意味着你已经用完了试用期，需要订阅付费计划。

## API Key

要获取你的 NovelAI API key，请按照以下步骤操作：

1. 选择左侧边栏顶部的齿轮图标。
![Left Sidebar](/static/novel-side.png)

2. 在 "User Settings" 下选择 "Account"。
![User Settings](/static/novel-user.png)

3. 选择 "Get Persistent API Token"。
![Account](/static/novel-account.png)

4. 选择复制图标将你的 NovelAI API token 复制到剪贴板。
![Persistent API Token](/static/novel-token.png)

## 模型

如果你有 Opus，那么 Erato 是要使用的模型。如果你没有 Opus，那么 Kayra 是最好的可用模型。

Clio 在 Tablet/scroll tiers 上有更大的 context 大小，但 Kayra 的强度通常可以弥补这种差异。

## 设置

设置文件在这里（`SillyTavern/data/<user-handle>/NovelAI Settings`）。
你也可以手动添加自己的设置文件。

### Response Length

你希望每条消息生成多少文本。请注意，NovelAI 每次响应的限制为 150 tokens。

### Context Size

在任何给定时间，聊天中保留在 context 中的 tokens 数量。你可以使用的最大 context 大小取决于模型和你的订阅 tier：

- Kayra (Tablet) - 3072 tokens
- Kayra (Scroll) - 6144 tokens
- Erato (仅限 Opus)、Kayra (Opus) 和 Clio (所有 tiers) - 8192 tokens

### Preamble

插入在聊天上方的文本，用于修改写作风格。推荐的格式是一个简短标签列表，如 "[ Style: chat, detailed, sensory ]"。

## Preset 描述
根据 Novel AI 的说法，这是默认 presets 的适用场景。

### Erato

* Golden Arrow - 一个很好的全能选择。
* Wilder - 更高的词汇选择多样性，重新生成之间的差异更大，更容易出错。
* Zany Scribe - 避免错误和重复。优先使用更复杂的词汇。
* Dragonfruit - 多样化和复杂的语言，重复很少。更频繁的错误和矛盾。
* Shosetsu - 专为日语写作设计。也适用于英语。

### Kayra

* Asper - 用于创意写作。期待意想不到的转折。
* Carefree - 一个很好的全能选择
* Fresh-Coffee - 保持正轨。很好地处理 instruct。
* Pro_Writer - 模仿畅销小说的节奏和感觉
* Stelenes - 更有可能选择合理的替代方案。重试时有多样性。
* Tea_Time - 开始后会变得更好。
* Writers-Daemon - 极富想象力，有时过于丰富。

### Clio

* Edgewise - 很好地处理各种生成风格
* Fresh Coffee - 保持正轨。
* Long-Press - 用于创意散文。
* Talker Chat - 专为聊天风格生成设计。
* Vingt-Un - 一个很好的全能默认选择，倾向于散文。

## 使用 NovelAI 与 SillyTavern 的提示和常见问题

当从另一个 ST 后端 API 切换到 NovelAI 时，会出现很多常见问题和疑问。差异归结于模型的训练目的。很可能，你使用过 OpenAI 或 Anthropic 模型（或模仿这些模型的本地模型），这些模型是围绕遵循用户指令构建的。NovelAI 的模型纯粹围绕文本补全构建：NovelAI 的模型不是将你的输入作为消息并制定响应，而是尝试继续传入的提示。由于这种差异，许多适用于其他 API 的提示和常识对 NAI 不起作用。

### 为 NovelAI 调整设置

在 Advanced Formatting（A 图标）下：
- 将 "Context Template" 设置为 "NovelAI"
- 将 "Tokenizer" 设置为 "Best match"
- 勾选 "Always add character's name to prompt"
- 勾选 "Collapse Consecutive Newlines"
- 在 "Instruct Mode" 下取消勾选 "Enabled" 框

在 User Settings（带齿轮的人）下
- 打开 "Swipes"（不是 NAI 特定的，但它非常有用，你应该这样做）

### 为 NovelAI 构建/调整角色卡

要为 NovelAI 优化你的角色卡，有几种推荐的方法来编写角色描述：散文和属性。

散文非常简单，感觉不应该有效："Sylpheed 是一个看起来很年轻但实际上已经 900 岁的仙女。她身材矮小娇小，长长的白发在编织的侧马尾辫中渐变为绿色渐变，祖母绿般的眼睛呈十字形。[...]" 不，真的，就是这样。只需用正常的句子写出角色的外观、行为等，AI 就会理解。

如果你不相信自己的写作能力或想要更结构化的方法，可以使用属性方法，这在 NovelAI 训练数据中存在。这作为一个简单的不同类型的角色特征列表工作。以下是经过测试对 NovelAI 模型有效的可能属性列表：

```
Name:
AKA:
Type: character
Setting:
Nationality:
Species:
Gender:
Age:
Height:
Weight:
Appearance:
Clothing:
Attire:
Personality:
Mind:
Mental:
Likes:
Dislikes:
Sexuality:
Speech:
Voice:
Abilities:
Skills:
Quote:
Affiliation:
Occupation:
Reputation:
Secret:
Family:
Allies:
Enemies:
Background:
Description:
Attributes:
```

"Type: character" 是为了告诉 AI 这是在描述一个角色（而不是位置、物体或其他类型的东西）。其余属性是可选的，有些是冗余的（例如，Personality、Mind 和 Mental 基本上意味着同样的事情），但这些已经过测试，并且与 NovelAI 的模型配合得很好。填写与你的角色相关的任何属性。属性应该用小写字母书写，并用逗号分隔，不需要在单词周围加引号。例如：

```
Skills: lockpicking, stealth, running away very fast
```

推荐这些方法是因为它们存在于 NovelAI 的训练数据中，因此它们特别适合该模型。

#### 示例卡片

以下是几个为 NovelAI 制作的示例卡片，展示了专门为 NovelAI 创建卡片的不同方式。第一张卡片 Valka 使用属性方法进行角色描述，而第二张卡片 Eris 使用散文描述，以及大量示例对话。

<div style="display:flex;gap:2em;justify-content:center">

[![Valka](/static/Valka.png)](/static/Valka.png)

[![Eris](/static/Eris.png)](/static/Eris.png)

</div>

#### 不应该做什么

大多数现有的角色卡格式不适合 NovelAI。它们会给你一些结果，甚至一些好的结果，但它们有很多问题。W++ 是最大的违规者之一，它不像 NovelAI 模型训练的任何东西，而且它不断使用括号/大括号/引号会吃掉大量 tokens，使卡片的大小膨胀而没有真正的好处。

在不是内置于 NovelAI 的现有格式中，AliChat 是最有可能工作的格式，因为它依赖于使用示例消息来同时传达有关角色及其声音的信息，采用你希望 AI 输出的消息类型的格式。

对于大多数其他格式，由于它们通常是列出特定角色的不同特征的方式，因此可以相当直接地转换为属性方法。

### 我应该使用哪个 module？

可能是 No Module。如果你希望角色以更华丽的方式说话，Prose Augmenter 很有用，但要小心不要过度使用。Text Adventure 可能对文本冒险风格的卡片/故事有用。

### 不是 instruct module 吗？

你可以在需要时调用 Instruct module。在你的消息中创建一个新行，并将你的指令放在花括号中，如下所示：`{ CharName is offended by that seemingly innocuous statement }`（文本和括号之间的空格是 _必需的_）。这样做会自动将 AI 切换到 Instruct module 一小段时间。你不想一直使用 Instruct module，因为它往往会产生比其他 modules 更少创意的输出，只有当你需要强烈引导 AI 朝特定方向时才使用。

### 为什么我的响应总是被截断？

NovelAI 将响应长度限制为总共约 150 tokens，即使你将滑块设置得更高。当它达到滑块中的 tokens 数量或 150 个（以较低者为准）时，它将生成最多 20 个 tokens，寻找停止序列或句子的结尾，因此有效限制为 170 tokens 的响应，此时它会停止，导致它被截断。

如果它被截断，你可以选择继续选项（在文本框左侧的三行菜单中）让角色继续他们的响应。

如果你经常想要超过 170 tokens 的响应，可以这样解决限制：

- 将响应长度保持在 150 tokens。
- 在 Advanced Formatting 下，启用 Auto-continue。
- 将 "Target length" 设置为所需长度。

这将链接多个生成以给你更长的消息，但不保证如果模型决定停止，回复将 100% 达到所需长度。

### 如何让机器人写更长的响应？

阅读上面关于响应被截断的内容。这将有助于确保响应不会因达到生成长度限制而过早被截断。

如果你的响应没有被截断但仍然太短，很可能你正在处理"垃圾进，垃圾出" - 如果你给模型不好的示例，它会产生不好的输出。如果角色卡没有示例对话或示例对话很短，并且你发送给机器人的消息很短，模型会注意到这一点，将其视为可接受的做事方式，响应会很短。因此，写更长的示例对话和更长的消息给机器人。（你总是可以使用 NovelAI 为你写一些示例对话，而不是自己做。）

### 如何让机器人停止为我说话？

- 检查角色卡的第一条消息和示例对话是否包括角色为你采取行动 - 如果包括，则重写它们以消除它为你行动
- 确保勾选了 "Always add character's name to prompt"
- 确保你当前使用的用户角色与聊天的其余部分相同。如果你更改了用户角色并且没有改回（或没有锁定到该聊天的角色），通常停止为你生成的规则将失败
- 将 ["\n\{\{user\}\}:"] 添加到 Custom Stopping Strings（不应该是必需的，但有时有帮助）

### 为什么我的角色没有响应？

很多事情都可能导致这种情况，所以我们需要查看几个地方：

- 确保在 Advanced Formatting 中勾选了 "Always add character's name to prompt"
- 检查以确保 API 没有任何错误。虽然你可以在 NAI 免费试用期间使用 SillyTavern，但一旦试用期结束，你只会收到错误
- 检查你在 "Custom Stopping Strings" 中有什么 - 如果这些在响应开始时生成，可能会过早被截断

### 我应该如何使用 Author's Note？

一般来说，你可能不应该使用。它被插入到 context 末尾附近，对于 NAI 的模型，它经常压倒 context 中的其他所有内容。它主要是旧版本、较弱模型的遗留物，当时更有必要。

### 如何进行场景转换/时间跳跃？

将以下内容作为系统消息或在下一条消息开头的新行上：
```
***
[ 2 days later ]
```

然后在下一行放置消息的其余部分。括号中的文本可以是时间跳跃、新位置或其他任何内容。"***"（滑稽地命名为"dinkus"）告诉 AI 场景已经改变，括号中的文本提供了更多的上下文。

### AI 不断重复特定的单词/短语，我该怎么办？

如上所述，你可以稍微提高重复惩罚滑块，尽管推得太远会使输出不连贯。
要更彻底地解决问题，请返回 context，尤其是最近的消息，并删除重复的单词/短语。从 context 中删除它可以减少 AI 首先开始说它的理由。
