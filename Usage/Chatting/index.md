---
icon: report
order: 170
expanded: false
---

# 聊天

当您[连接到 API](/Usage/API_Connections/index.md) 后，可以通过在屏幕底部的聊天栏中输入消息来发送给 AI。然后点击 <i class="fa-solid fa-paper-plane"></i> **发送** 或按回车键。
![聊天栏](/static/chatbox.png)

AI 将回复一条继续对话的消息。

![聊天消息](/static/chatmessage.png)

现在您可以：

* **发送另一条消息**
* **滑动回复**：点击消息上的 <i class="fa-solid fa-chevron-right"></i> **滑动** 按钮以生成不同的回复。
* **编辑消息**：点击任何消息上的 <i class="fa-solid fa-pencil"></i> **编辑** 按钮来[编辑消息内容](#编辑消息内容)。
* **消息操作**：点击消息上的 <i class="fa-solid fa-ellipsis"></i> **消息操作** 按钮，获取更多[消息选项](#消息操作面板)，如[翻译](../../extensions/Translation.md)、图像生成和故事分支。
* **聊天选项**：点击聊天栏旁边的 <i class="fa-solid fa-bars"></i> **选项** 按钮，获取更多[聊天选项](#聊天选项面板)，如作者注释和聊天文件管理。

!!! 编辑和滑动
如果您希望说些不同的话，您可以编辑您的消息，然后滑动 AI 的回复以获得新的回复。
!!!

!!! 键盘快捷键
您也可以使用**右**箭头键来滑动，使用**上**箭头键来编辑聊天中的最后一条消息。要了解更多快捷键，请在聊天中使用 `/help hotkeys` [斜杠命令](/Usage/Chatting/slashcommands.md)或查看[快捷键](/Usage/Chatting/hotkeys.md)页面。
!!!

## 消息操作面板

通过消息上的省略号（•••）按钮管理单个聊天消息。

要在您的聊天中显示所有消息的这些选项，请在用户设置中启用[展开消息操作](/Usage/User_Settings/uicustomization.md#主题开关)设置。

### 核心功能

* <i class="fa-solid fa-language"></i> **翻译**：将消息转换为不同语言
* <i class="fa-solid fa-paintbrush"></i> **生成图像**：从消息内容[创建图像](/extensions/Stable-Diffusion.md)
* <i class="fa-solid fa-bullhorn"></i> **朗读**：[文本转语音](/extensions/TTS.md)转换
* <i class="fa-solid fa-square-poll-horizontal"></i> **提示词**：查看生成提示词和 token 使用情况

### 消息可见性

* <i class="fa-solid fa-eye"></i> **包含**：AI 可以看到此消息；点击以排除
* <i class="fa-solid fa-eye-slash"></i> **排除**：AI 看不到此消息；点击以包含

### 内容管理

* <i class="fa-solid fa-paperclip"></i> **嵌入**：[附加文件或图像](/Usage/Characters/data-bank.md#关于文档)
* <i class="fa-solid fa-flag-checkered"></i> **检查点**：创建故事检查点
* <i class="fa-solid fa-flag"></i> **检查点导航**：点击以打开检查点聊天，Shift+点击以更新现有检查点
* <i class="fa-solid fa-code-branch"></i> **分支**：开始替代故事路径
* <i class="fa-solid fa-copy"></i> **复制**：复制消息文本
* <i class="fa-solid fa-pencil"></i> **编辑**：编辑消息内容

## 编辑消息内容

当您 <i class="fa-solid fa-pencil"></i> **编辑**聊天消息时出现的紧凑消息操作工具面板。

### 核心操作

* <i class="fa-solid fa-check"></i> **确认**：保存消息更改
* <i class="fa-solid fa-xmark"></i> **取消**：放弃消息更改

### 消息操作

* <i class="fa-solid fa-copy"></i> **复制**：复制消息内容
* <i class="fa-solid fa-trash-can"></i> **删除**：移除消息

### 消息位置

* <i class="fa-solid fa-chevron-up"></i> **上移**：将消息在聊天中向上移动
* <i class="fa-solid fa-chevron-down"></i> **下移**：将消息在聊天中向下移动

注意：根据消息在聊天历史中的位置，移动控件可能被禁用。

## 聊天选项面板

通过聊天界面左下角的 <i class="fa-solid fa-bars"></i> **选项** 按钮管理聊天设置和操作。

### 显示控件

* <i class="fa-lg fa-solid fa-times"></i> **关闭聊天**：退出当前聊天会话
* <i class="fa-lg fa-solid fa-cog"></i> **切换面板**：显示/隐藏[界面面板](/Usage/index.md#控制面板)

### 生成设置

* <i class="fa-lg fa-solid fa-note-sticky"></i> **[作者注释](/Usage/Characters/Author's-Note.md)**：自定义上下文指令
* <i class="fa-lg fa-solid fa-scale-balanced"></i> **[CFG 比例](/Usage/Prompts/CFG.md)**：调整回复创造性
* <i class="fa-lg fa-solid fa-pie-chart"></i> **[Token 概率](#Token-概率面板)**：查看 token 生成统计

### 聊天导航

* <i class="fa-lg fa-solid fa-left-long"></i> **返回父聊天**：返回主对话
* <i class="fa-lg fa-solid fa-flag"></i> **保存检查点**：创建故事检查点
* <i class="fa-lg fa-solid fa-people-arrows"></i> **转换为群组**：转换为[群组聊天](/Usage/Characters/groupchats.md)

### 聊天管理

* <i class="fa-lg fa-solid fa-comments"></i> **开始新聊天**：开始新的对话
* <i class="fa-lg fa-solid fa-address-book"></i> **管理聊天文件**：[聊天文件操作](/Usage/Characters/chatfilemanagement.md)，如导入、导出和重命名

### 消息控件

* <i class="fa-lg fa-solid fa-trash-can"></i> **删除消息**：选择并移除多条消息
* <i class="fa-lg fa-solid fa-repeat"></i> **重新生成**：创建新的回复
* <i class="fa-lg fa-solid fa-user-secret"></i> **扮演**：AI 以用户身份写消息
* <i class="fa-lg fa-solid fa-arrow-right"></i> **继续**：延伸最后一条消息

注意：根据上下文和聊天状态，某些选项可能被隐藏。

## Token 概率面板

Token 概率面板让您深入了解 AI 的文本生成采样过程。它不仅显示 AI 写了什么，还显示它在文本的每个点上考虑的其他选项。

要打开它，请点击 <i class="fa-solid fa-bars" title="Burger Menu icon"></i> **聊天选项**面板中的 <i class="fa-solid fa-pie-chart"></i> **Token 概率**按钮。

![示例消息](/static/token-probs/fling-msg.png){ width=500}

![示例消息的 Token 概率显示](/static/token-probs/fling-probs.png){ width=500}

当您点击生成文本中的任何 token（单词、标点符号或格式字符）时，面板会显示 AI 在该位置考虑的替代 token，以及它们的概率分数。这让您深入了解 AI 的"思考过程"，并显示回复可能采取的其他方向。查看这些替代选项可以帮助您理解是有多个可能的选项还是只有一个明确的选择。

![替代 token 和概率](/static/token-probs/fling-probs-logprob.png){ width=500}

如果您看到一个您认为 AI 应该选择不同的 token，选择一个替代选项，消息将从该点开始重新生成，可能会给您一个不同的回复。

### 重新生成

如果您更改特定 token 并重新生成回复，新回复中更改 token 之前的部分将与原始回复相同。这部分显示为灰色。由于它没有被生成，这部分没有概率信息。

您可能想看到基于您的替代 token 可能生成的其他回复。

您可以点击灰色部分来"重新生成"，给您文本的新变体。点击灰色部分的任何部分都会保留整个灰色部分并重新生成整个白色/着色部分。

在点击灰色部分中的 token 时按住 Ctrl 键将保留到被点击 token 的灰色部分并重新生成其余文本。在这种情况下，您选择的替代 token 不能被保留。

### 控件

**Token 显示**：

* 生成的文本被分割成单个 token
* 每个 token 都是可交互的，点击 token 可以查看 AI 考虑的替代选项
* Token 被着色作为视觉辅助，但这并不表示概率
* 特殊字符（空格、换行）被明显标记

**Token 选择**：

* 点击 token 查看替代选项
* 点击替代选项以替换 token 并重新生成回复
* 将鼠标悬停在 token 上可以查看其原始对数概率分数

**窗口控件**：

* <i class="fa-solid fa-grip"></i> 用于面板重新定位的拖动手柄（仅限 MovingUI）
* <i class="fa-solid fa-window-maximize"></i> 最大化/还原面板大小
* <i class="fa-solid fa-circle-chevron-up"></i> 展开/折叠面板内容
* <i class="fa-solid fa-circle-xmark"></i> 关闭面板

### 可用性

您必须在[用户设置](/Usage/User_Settings/User_Settings.md#聊天消息处理)中选择**请求 token 概率**以启用此功能。

Token 概率仅适用于最新消息，且不会保存到聊天中。如果消息的 token 概率信息不再可用，面板将显示一条指示此情况的消息。

使用平滑流式传输时，Token 概率不可用。

并非所有 API 都提供 Token 概率。如果您使用的 API 不支持 token 概率，面板将打开但不会显示任何信息。

#### 文本补全
* **LlamaCPP**：可用
* **Text Generation WebUI**（oobabooga）：可用
* **TabbyAPI**：可用
* **NovelAI**：可用
* **KoboldCPP**：可用
* **Ollama**：似乎不可用
* **OpenRouter Text**：似乎不可用

#### 聊天补全
* **OpenAI** 或 **Custom**：可用，但不支持重新生成
* **Anthropic**：似乎不可用
* **Google AI Studio**：似乎不可用
* **OpenRouter Chat**：似乎不可用
