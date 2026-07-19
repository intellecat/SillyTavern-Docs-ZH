---
order: 100
route: /usage/core-concepts/advancedformatting/
---


# 高级格式设置

本节提供的设置允许对 [提示词构建](prompts.md) 策略进行更多控制，主要针对文本补全 API。

本面板中的大多数设置不适用于聊天补全 API，因为它们由提示词管理器系统管理。

+++ 文本补全 API
* [系统提示词](#system-prompt)
* [上下文模板](#context-template)
* [分词器](#tokenizer)
* [自定义停止字符串](#custom-stopping-strings)
+++ 聊天补全 API
* 系统提示词：不适用，请使用[提示词管理器](prompt-manager.md)
* 上下文模板：不适用，请使用[提示词管理器](prompt-manager.md)
* [分词器](#tokenizer)
* [自定义停止字符串](#custom-stopping-strings)
+++

## 重置模板 {#resetting-templates}

您可以将默认模板恢复到原始状态。这可以通过 UI 或手动删除相关数据文件来完成。

### 通过 UI 重置

1. 打开 **<i class="fa-solid fa-font"></i> 高级格式设置** 菜单。
2. 选择您要重置的模板。
3. 点击 **<i class="fa-solid fa-recycle"></i> 恢复当前模板** 按钮。
4. 在提示时确认操作。

### 手动重置

!!!
请确保 [config.yaml](/Administration/config-yaml.md#data-configuration) 中的 `skipContentCheck` 设置为 `false`，否则内容检查将不会被触发。
!!!

1. 导航到您的用户数据目录（详情请参阅[数据路径](/Installation/index.md#data-paths)）。
2. 从用户数据目录根目录中删除 `content.log` 文件。此文件记录了为您的用户复制的默认文件。
3. 从相关子目录（`context`、`instruct`、`sysprompt` 等）中删除模板 JSON 文件。
4. 重启 SillyTavern 服务器。应用程序将重新填充默认内容，恢复任何已删除的默认模板。

## 后端定义的模板

!!! 适用于：文本补全 API
不适用于聊天补全 API，因为它们使用不同的提示词构建器。
!!!

一些文本补全源提供了自动选择模型作者推荐模板的功能。这通过比较模型的 `tokenizer_config.json` 文件中定义的聊天模板的哈希值与 SillyTavern 默认模板之一来实现。

1. 必须在 **<i class="fa-solid fa-font"></i> 高级格式设置** 菜单中启用 **<i class="fa-solid fa-bolt"></i> 派生模板** 选项。这可以应用于上下文、指令或两者。
2. 必须选择支持的后端作为文本补全源。目前只有 llama.cpp 和 KoboldCpp 支持派生模板。
3. 在建立与 API 的连接时，模型必须正确报告其元数据。如果这不起作用，请尝试将后端更新到最新版本。
4. 报告的聊天模板哈希值必须与 [已知的 SillyTavern 模板](https://github.com/SillyTavern/SillyTavern/blob/release/public/scripts/chat-templates.js) 之一匹配。这仅涵盖默认模板，如 Llama 3、Gemma 2、Mistral V7 等。
5. 如果哈希值匹配，且模板存在于模板列表中（即未重命名或删除），则会自动选择该模板。

## 系统提示词 {#system-prompt}

!!! 适用于：文本补全 API
对于聊天补全 API 中的等效设置，请使用[提示词管理器](prompt-manager.md)。**主提示词**是聊天补全 API 中系统提示词的等效项。
!!!

系统提示词定义了模型需要遵循的一般指令。它为对话设置基调和上下文。例如，它告诉模型充当 AI 助手、写作伙伴或虚构角色。

系统提示词是[故事字符串](context-template.md#story-string)的一部分，通常是模型接收的提示词的第一部分。

查看[提示词指南](prompts.md#主提示词系统提示词)以了解更多关于系统提示词的信息。

## 上下文模板 {#context-template}

!!! 适用于：文本补全 API
对于聊天补全 API 中的等效设置，请使用[提示词管理器](prompt-manager.md)。
!!!

通常，AI 模型需要您以某种特定方式提供角色数据。SillyTavern 包含了一系列针对不同模型的预制转换规则，但您可以根据需要自定义它们。

本节的选项在[上下文模板](context-template.md)中有详细说明。

## 分词器 {#tokenizer}

分词器是一个将文本分解成更小单位（称为 token）的工具。这些 token 可以是单个词，甚至是词的部分，如前缀、后缀或标点符号。一个经验法则是，一个 token 通常对应 3~4 个字符的文本。

本节的选项在[分词器](tokenizer.md)中有详细说明。

## 自定义停止字符串 {#custom-stopping-strings}

接受 JSON 序列化的停止字符串数组。示例：`["\n", "\nUser:", "\nChar:"]`。如果您不确定格式，请使用[在线 JSON 验证器](https://jsonlint.com/)。如果模型输出**以任何停止字符串结尾**，它们将从输出中移除。

支持的 API：

1. KoboldAI Classic（1.2.2 及更高版本）或 KoboldCpp
2. AI Horde
3. 文本补全 API：Text Generation WebUI (ooba)、Tabby、Aphrodite、Mancer、TogetherAI、Ollama 等
4. NovelAI
5. OpenAI（最多 4 个字符串）和兼容的 API
6. OpenRouter（文本和聊天补全）
7. Claude
8. Google AI Studio
9. MistralAI

## 回复起始内容 {#start-reply-with}

!!! 注意
默认情况下，回复起始内容前缀不会显示在生成的消息中。启用"在聊天中显示回复前缀"可以显示它。
!!!

### 文本补全 API

预填充提示词的最后一行，强制模型从该点继续。这对于强制执行特定内容很有用，例如使用定义的前缀引导模型进行[模型推理](/Usage/Prompts/reasoning.md)：

```txt
<think>
Sure!
```

### 聊天补全 API

在提示词末尾添加一条助手角色消息。对于某些后端模型，这等同于预填充模型响应，但有些可能完全不支持此功能，并会返回验证错误。如果不确定，请将此字段留空。
