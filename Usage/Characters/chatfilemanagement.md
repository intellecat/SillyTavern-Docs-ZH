---
order: 80
route: /usage/core-concepts/chatfilemanagement/
---

# 聊天文件管理

本页介绍管理 AI 聊天文件的各种方式。

!!!info 注意
其中部分选项可在左下角选项菜单打开的"管理聊天文件"对话框中找到。
!!!

## 单人聊天与群组聊天

使用角色卡的最简单方式是单人聊天：直接点击角色卡，开始聊天即可。

拥有多个角色卡后，您也可以使用"创建新聊天群组"按钮来创建包含多个角色的[群组聊天](/Usage/Characters/groupchats.md)，这些角色将相互交流并与您互动。

## 聊天导入

**将 Character.AI 的聊天导入到 SillyTavern。**

如需导入 Character.AI 的聊天记录和机器人，请使用 CAI Tools 浏览器扩展：[https://github.com/irsat000/CAI-Tools](https://github.com/irsat000/CAI-Tools)。

其他支持导入聊天记录的程序和工具包括：

* TavernAI（原版）：<https://github.com/TavernAI/TavernAI>
* Text Generation WebUI (oobabooga)：<https://github.com/oobabooga/text-generation-webui>
* Agnai：<https://github.com/agnaistic/agnai>
* KoboldAI Lite：<https://github.com/LostRuins/lite.koboldai.net>
* RisuAI：<https://github.com/kwaroran/RisuAI>

## 导出为 .jsonl

点击"管理聊天文件"后，聊天文件列表中的每个条目都有一个导出按钮，可将其导出为可原样重新导入的格式。使用此功能可共享或迁移聊天记录，包含所有元数据（但不含图片和文件附件）。

如果您注重隐私，请务必检查导出的 JSONL 文件，并删除不希望共享的内容。

## 导出为 .txt

您也可以通过"将聊天下载为纯文本文档"按钮导出简化的纯文本版本。由于会丢失重要元数据，该格式无法重新导入！

## 检查点

"检查点"是当前聊天的克隆，它会复制指定聊天中直至某一时间点的所有消息，并存储与源聊天的链接（通过聊天文件名）。

点击每条聊天消息右侧的三点按钮，您有两种方式创建检查点：

* "创建分支"——克隆当前聊天直至该消息，并切换到该分支。
* "创建检查点"——克隆当前聊天直至该消息，询问名称后创建，但不切换到该检查点。

您可以将它们大致理解为浏览器中的"在新标签页中打开链接"和"在后台新标签页中打开链接"。

您可以从检查点返回父聊天：点击消息文本框左侧的汉堡菜单按钮，然后点击"返回父聊天"。

## 重命名聊天

默认情况下，聊天文件以创建的日期和时间命名。

您可以通过点击铅笔图标并输入新名称来更改聊天名称。

请注意，重命名后将断开来自检查点的链接（因为检查点通过聊天文件名进行链接）。
