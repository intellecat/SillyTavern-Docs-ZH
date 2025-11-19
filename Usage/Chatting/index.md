---
icon: report
order: 170
expanded: false
route: /usage/chatting/
---

# 聊天

当你[连接到 API](/Usage/API_Connections/index.md) 后,在屏幕底部的聊天栏中输入消息即可向 AI 发送消息。然后点击 <i class="fa-solid fa-paper-plane"></i> **发送** 或按 Enter 键。
![聊天栏](/static/chatbox.png)

AI 将回复一条消息以继续对话。

![聊天消息](/static/chatmessage.png)

你现在可以:

* **发送另一条消息**
* **滑动回复**: 点击消息上的 <i class="fa-solid fa-chevron-right"></i> **滑动** 按钮以生成不同的回复。
* **编辑消息**: 点击任意消息上的 <i class="fa-solid fa-pencil"></i> **编辑** 按钮以[编辑消息内容](#edit-message-content)。
* **消息操作**: 点击消息上的 <i class="fa-solid fa-ellipsis"></i> **消息操作** 按钮以获取更多[消息选项](#message-actions-panel),如[翻译](../../extensions/Translation.md)、图像生成和故事分支。
* **聊天选项**: 点击聊天栏旁边的 <i class="fa-solid fa-bars"></i> **选项** 按钮以获取更多[聊天选项](#chat-options-panel),如作者注释和聊天文件管理。

!!! 编辑和滑动
如果你希望说些不同的话,可以编辑你的消息,然后滑动 AI 的回复以获得新的回复。
!!!

!!! 键盘快捷键
你也可以使用**右**方向键来滑动,使用**上**方向键来编辑聊天中的最后一条消息。有关更多 hotkeys,请在聊天中使用 `/help hotkeys` [slash command](/Usage/Chatting/slashcommands.md) 或查看 [HotKeys](/Usage/Chatting/hotkeys.md) 页面。
!!!

## Message actions panel

通过消息上的省略号(•••)按钮管理单个聊天消息。

要在所有聊天消息中显示这些选项,请在用户设置中启用 [Expand Message Actions](/Usage/User_Settings/uicustomization.md#theme-toggles) 设置。

### 核心功能

* <i class="fa-solid fa-language"></i> **Translate**: 将消息转换为不同语言
* <i class="fa-solid fa-paintbrush"></i> **Generate Image**: 从消息内容[创建图像](/extensions/Stable-Diffusion.md)
* <i class="fa-solid fa-bullhorn"></i> **Narrate**: [文本转语音](/extensions/TTS.md)转换
* <i class="fa-solid fa-square-poll-horizontal"></i> **Prompt**: 查看生成 prompt 和 token 使用情况

### 消息可见性

* <i class="fa-solid fa-eye"></i> **Included**: AI 可以看到此消息;点击以排除它
* <i class="fa-solid fa-eye-slash"></i> **Excluded**: AI 看不到此消息;点击以包含它

### 内容管理

* <i class="fa-solid fa-paperclip"></i> **Embed**: [附加文件或图像](/Usage/Characters/data-bank.md#about-documents)
* <i class="fa-solid fa-flag-checkered"></i> **Checkpoint**: 创建故事检查点
* <i class="fa-solid fa-flag"></i> **Checkpoint Navigation**: 点击打开检查点聊天,Shift+点击更新
  现有检查点
* <i class="fa-solid fa-code-branch"></i> **Branch**: 开始替代故事路径
* <i class="fa-solid fa-copy"></i> **Copy**: 复制消息文本
* <i class="fa-solid fa-pencil"></i> **Edit**: 编辑消息内容

## Edit message content

当你 <i class="fa-solid fa-pencil"></i> **编辑** 聊天消息时出现的紧凑型消息操作工具面板。

### 核心操作

* <i class="fa-solid fa-check"></i> **Confirm**: 保存消息更改
* <i class="fa-solid fa-xmark"></i> **Cancel**: 放弃消息更改

### 消息操作

* <i class="fa-solid fa-copy"></i> **Copy**: 复制消息内容
* <i class="fa-solid fa-trash-can"></i> **Delete**: 删除消息

### 消息位置

* <i class="fa-solid fa-chevron-up"></i> **Move Up**: 将消息在聊天中向上移动
* <i class="fa-solid fa-chevron-down"></i> **Move Down**: 将消息在聊天中向下移动

注意:根据消息在聊天历史中的位置,移动控制可能会被禁用。

## Chat options panel

通过聊天界面左下角的 <i class="fa-solid fa-bars"></i> **选项** 按钮管理聊天设置和操作。

### 显示控制

* <i class="fa-lg fa-solid fa-times"></i> **Close chat**: 退出当前聊天会话
* <i class="fa-lg fa-solid fa-cog"></i> **Toggle Panels**: 显示/隐藏[界面面板](/Usage/index.md#control-panels)

### 生成设置

* <i class="fa-lg fa-solid fa-note-sticky"></i> **[Author's Note](/Usage/Characters/Author's-Note.md)**: 自定义上下文指令
* <i class="fa-lg fa-solid fa-scale-balanced"></i> **[CFG Scale](/Usage/Prompts/CFG.md)**: 调整回复创造性
* <i class="fa-lg fa-solid fa-pie-chart"></i> **[Token Probabilities](#token-probabilities-panel)**: 查看 token 生成统计信息

### 聊天导航

* <i class="fa-lg fa-solid fa-left-long"></i> **Back to parent chat**: 返回主对话
* <i class="fa-lg fa-solid fa-flag"></i> **Save checkpoint**: 创建故事检查点
* <i class="fa-lg fa-solid fa-people-arrows"></i> **Convert to group**: 转换为[群组聊天](/Usage/Characters/groupchats.md)

### 聊天管理

* <i class="fa-lg fa-solid fa-comments"></i> **Start new chat**: 开始新对话
* <i class="fa-lg fa-solid fa-address-book"></i> **Manage chat files**: [聊天文件操作](/Usage/Characters/chatfilemanagement.md),如导入、导出和重命名

### 消息控制

* <i class="fa-lg fa-solid fa-trash-can"></i> **Delete messages**: 选择并删除多条消息
* <i class="fa-lg fa-solid fa-repeat"></i> **Regenerate**: 创建新回复
* <i class="fa-lg fa-solid fa-user-secret"></i> **Impersonate**: AI 以用户身份写消息
* <i class="fa-lg fa-solid fa-arrow-right"></i> **Continue**: 延续最后一条消息

注意:某些选项可能会根据上下文和聊天状态而隐藏。

## Token Probabilities Panel

Token Probabilities 面板让你深入了解 AI 文本生成的采样过程。它不仅显示 AI 写了什么,还显示它在文本的每个点考虑了哪些其他选项。

要打开它,请点击 <i class="fa-solid fa-bars" title="Burger Menu icon"></i> **Chat Options** 面板中的 <i class="fa-solid fa-pie-chart"></i> **Token Probabilities** 按钮。

![示例消息](/static/token-probs/fling-msg.png){ width=500}

![示例消息的 token probabilities 显示](/static/token-probs/fling-probs.png){ width=500}

当你点击生成文本中的任何 token(单词、标点符号或格式化字符)时,面板会显示 AI 在该位置考虑的备选 tokens,以及它们的概率分数。这让你深入了解 AI 的"思考过程",并显示回复可能采取的其他方向。查看这些备选项可以帮助你了解是否有几个可能的选项或一个明确的选择。

![备选 tokens 和概率](/static/token-probs/fling-probs-logprob.png){ width=500}

如果你看到一个你认为 AI 应该选择不同的 token,选择一个备选项,消息将从该点向前重新生成,可能会给你一个不同的回复。

### 重新生成

如果你更改特定 token 并重新生成回复,新回复中更改的 token 之前的部分将与原始回复相同。这部分以灰色显示。由于它没有被生成,因此这部分没有概率信息。

你可能想看看基于你的备选 token 可能生成的其他回复。

你可以点击灰色部分来"重新生成",为你提供文本的新变体。点击灰色部分的任何地方都会保留整个灰色部分并重新生成整个白色/着色部分。

在点击灰色部分中的 token 时按住 Ctrl 键将保留到被点击 token 之前的灰色部分,并重新生成其余文本。在这种情况下,你选择的备选 token 无法保留。

### 控制

**Token 显示**:

* 生成的文本被分割成单个 tokens
* 每个 token 都是交互式的,点击 token 可查看 AI 考虑的备选项
* Tokens 会被着色作为视觉辅助,但这不表示概率
* 特殊字符(空格、换行符)会被明显标记

**Token 选择**:

* 点击 token 查看备选项
* 点击备选项以替换 token 并重新生成回复
* 将鼠标悬停在 token 上可查看其原始 log-probability 分数

**窗口控制**:

* <i class="fa-solid fa-grip"></i> 拖动手柄用于面板重新定位(仅限 MovingUI)
* <i class="fa-solid fa-window-maximize"></i> 最大化/恢复面板大小
* <i class="fa-solid fa-circle-chevron-up"></i> 展开/折叠面板内容
* <i class="fa-solid fa-circle-xmark"></i> 关闭面板

### 可用性

你必须在[用户设置](/Usage/User_Settings/index.md#chatmessage-handling)中选择 **Request token probabilities** 才能启用此功能。

Token probabilities 仅适用于最新消息,并且不会保存到聊天中。如果消息的 token probability 信息不再可用,面板将显示一条消息指示这一点。

使用 Smooth Streaming 时不可用 Token probabilities。

并非所有 API 都提供 token probabilities。如果你使用的 API 不支持 token probabilities,面板将打开但不会显示任何信息。

#### Text Completion
* **LlamaCPP**: 可用
* **Text Generation WebUI** (oobabooga): 可用
* **TabbyAPI**: 可用
* **NovelAI**: 可用
* **KoboldCPP**: 可用
* **Ollama**: 似乎不可用
* **OpenRouter Text**: 似乎不可用

#### Chat Completion
* **OpenAI** 或 **Custom**: 可用,但不支持重新生成
* **Anthropic**: 似乎不可用
* **Google AI Studio**: 似乎不可用
* **OpenRouter Chat**: 似乎不可用
