---
order: 80
route: /usage/core-concepts/chatfilemanagement/
---

# 聊天文件管理

本页描述了管理 AI 聊天文件的方法。

!!!info 注意
其中一些选项可在从左下角 options 菜单打开的 "Manage chat files" 对话框中找到。
!!!

## 单人聊天与群聊

使用 character card 的最简单方法是 Solo chat；只需点击他们的 card 即可开始聊天。

拥有几个 character cards 后，您还可以使用 "Create New Chat Group" 按钮创建包含多个角色的 [group chat](/Usage/Characters/groupchats.md)，然后这些角色将相互交互并与您交互。

## 聊天导入

**将 Character.AI 的聊天导入 SillyTavern。**

要导入 Character.AI 聊天和 bots，请使用 CAI Tools browser extension：[https://github.com/irsat000/CAI-Tools](https://github.com/irsat000/CAI-Tools)。

您可以从中导入聊天的其他程序和工具包括：

* TavernAI (original): <https://github.com/TavernAI/TavernAI>
* Text Generation WebUI (oobabooga): <https://github.com/oobabooga/text-generation-webui>
* Agnai: <https://github.com/agnaistic/agnai>
* KoboldAI Lite: <https://github.com/LostRuins/lite.koboldai.net>
* RisuAI: <https://github.com/kwaroran/RisuAI>

## Export as .jsonl

点击 "Manage chat files" 时，聊天文件列表中的每个条目都会有一个按钮，可以将其导出为可以按原样重新导入的格式。使用此功能共享或迁移聊天，包括所有元数据（但不包括图像和文件附件）。

如果您关注隐私，请务必检查导出的 JSONL 文件并清除任何您不想共享的内容。

## Export as .txt

您还可以使用 "Download chat as plain text document" 按钮导出简化的纯文本版本。它不能重新导入，因为它会丢失重要的元数据！

## Checkpoints

"Checkpoints" 是当前聊天的克隆，它们将给定聊天中的所有消息复制到某个点，并存储指向源的链接（通过聊天文件名）。

从每条聊天消息右侧的三个点按钮，您有两种方法创建 checkpoints：

* "Create Branch" 将克隆当前聊天直到该消息并切换到它
* "Create Checkpoint" 将克隆当前聊天直到该消息，要求输入名称并创建它，但不会切换到它

您可以将它们大致理解为浏览器中的"在新标签页中打开链接"和"在后台的新标签页中打开链接"。

您可以通过进入消息文本框左侧的 burger menu 按钮，然后点击 "Back to parent chat"，从 checkpoint 返回到父聊天。

## Rename Chat

默认情况下，聊天文件的名称是它们开始的日期和时间。

您可以通过点击铅笔图标并输入新名称来更改此设置。

请注意，这将破坏从 checkpoints 到该聊天的链接（因为它们通过聊天文件名链接）。