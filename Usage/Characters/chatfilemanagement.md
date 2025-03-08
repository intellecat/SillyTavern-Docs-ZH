---
order: character-15
route: /usage/core-concepts/chatfilemanagement
---

# 聊天文件管理

本页描述了管理 AI 聊天文件的各种方法。

!!! info 注意
这些选项中的一些可以在底部左侧选项菜单中的"管理聊天文件"对话框中找到。
!!!

## 单人聊天与群组聊天

使用角色卡片最简单的方式是单人聊天；只需点击他们的卡片并开始聊天。

一旦您有了几个角色卡片，您也可以使用"创建新聊天群组"按钮来创建包含多个角色的[群组聊天](/Usage/Characters/groupchats.md)，这些角色将相互交流并与您互动。

## 聊天导入

**将 Character.AI 的聊天导入到 SillyTavern。**

要导入 Character.AI 的聊天和机器人，请使用 CAI Tools 浏览器扩展：[https://github.com/irsat000/CAI-Tools](https://github.com/irsat000/CAI-Tools)。

其他可以导入聊天的程序和工具包括：

* TavernAI（原版）：<https://github.com/TavernAI/TavernAI>
* Text Generation WebUI (oobabooga)：<https://github.com/oobabooga/text-generation-webui>
* Agnai：<https://github.com/agnaistic/agnai>
* KoboldAI Lite：<https://github.com/LostRuins/lite.koboldai.net>
* RisuAI：<https://github.com/kwaroran/RisuAI>

## 导出为 .jsonl

点击"管理聊天文件"时，聊天文件列表中的每个条目都有一个按钮，可以将其导出为可以原样重新导入的格式。使用此功能来共享或迁移聊天，包括所有元数据（但不包括图像和文件附件）。

如果您注重隐私，请务必检查导出的 JSONL 文件并删除任何您不想共享的内容。

## 导出为 .txt

您也可以使用"将聊天下载为纯文本文档"按钮导出简化的纯文本版本。由于失去了重要的元数据，它不能再次导入！

## 检查点

"检查点"是当前聊天的克隆，它们会复制给定聊天中直到某个点的所有消息，并存储到源的链接（通过聊天文件名）。

从每条聊天消息右侧的三个点按钮，您有两种创建检查点的方式：

* "创建分支"将克隆当前聊天直到该消息并切换到它
* "创建检查点"将克隆当前聊天直到该消息，询问名称并创建它，但不会切换到它

您可以将它们大致理解为浏览器中的"在新标签页中打开链接"和"在后台新标签页中打开链接"。

您可以通过点击消息文本框左侧的汉堡菜单按钮，然后点击"返回父聊天"从检查点返回到父聊天。

## 重命名聊天

默认情况下，聊天文件会以它们开始的日期和时间命名。

您可以通过点击铅笔图标并输入新名称来更改这个。

请注意，这将破坏检查点到该聊天的链接（因为它们是通过聊天文件名链接的）。
