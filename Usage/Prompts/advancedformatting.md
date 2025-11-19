---
order: 100
route: /usage/core-concepts/advancedformatting/
---

# 高级格式化

本节提供的设置允许对 [prompt-building](index.md) 策略进行更精细的控制,主要用于 Text Completion API。

此面板中的大多数设置不适用于 Chat Completions API,因为它们由 prompt manager 系统管理。

+++ Text Completion APIs
* [System Prompt](#system-prompt)
* [Context Template](#context-template)
* [Tokenizer](#tokenizer)
* [Custom Stopping Strings](#custom-stopping-strings)
+++ Chat Completion APIs
* System Prompt: 不适用,请使用 [Prompt Manager](prompt-manager.md)
* Context Template: 不适用,请使用 [Prompt Manager](prompt-manager.md)
* [Tokenizer](#tokenizer)
* [Custom Stopping Strings](#custom-stopping-strings)
+++

## Resetting Templates

你可以将默认模板恢复到原始状态。这可以通过 UI 或手动删除相关数据文件来完成。

### UI Reset

1. 打开 **<i class="fa-solid fa-font"></i> Advanced Formatting** 菜单。
2. 选择你想要重置的模板。
3. 点击 **<i class="fa-solid fa-recycle"></i> Restore current template** 按钮。
4. 在提示时确认操作。

### Manual Reset

!!!
确保在 [config.yaml](/Administration/config-yaml.md#data-configuration) 中将 `skipContentCheck` 设置为 `false`,否则内容检查不会被触发。
!!!

1. 导航到你的用户数据目录(详见 [Data paths](/Installation/index.md#data-paths))。
2. 从用户数据目录的根目录删除 `content.log` 文件。此文件跟踪为你的用户复制的默认文件。
3. 从相关子目录(`context`、`instruct`、`sysprompt` 等)删除模板 JSON 文件。
4. 重启 SillyTavern 服务器。应用程序将重新填充默认内容,恢复任何已删除的默认模板。

## Backend-defined templates

!!! Applies to: Text Completion APIs
不适用于 Chat Completion API,因为它们使用不同的 prompt builder。
!!!

一些 Text Completion 源提供了自动选择模型作者推荐的模板的功能。这通过将模型的 `tokenizer_config.json` 文件中定义的 chat template 的哈希值与默认 SillyTavern 模板之一进行比较来实现。

1. 必须在 **<i class="fa-solid fa-font"></i> Advanced Formatting** 菜单中启用 **<i class="fa-solid fa-bolt"></i> Derive templates** 选项。这可以应用于 Context、Instruct 或两者。
2. 必须选择支持的 backend 作为 Text Completion 源。目前只有 llama.cpp 和 KoboldCpp 支持派生模板。
3. 当建立与 API 的连接时,模型必须正确报告其元数据。如果不起作用,请尝试将 backend 更新到最新版本。
4. 报告的 chat template 哈希值必须与 [已知的 SillyTavern 模板](https://github.com/SillyTavern/SillyTavern/blob/release/public/scripts/chat-templates.js) 之一匹配。这仅涵盖默认模板,如 Llama 3、Gemma 2、Mistral V7 等。
5. 如果哈希值匹配,模板将在模板列表中存在时自动选择(即未重命名或删除)。

## System Prompt

!!! Applies to: Text Completion APIs
对于 Chat Completion API 中的等效设置,请使用 [Prompt Manager](prompt-manager.md)。**Main Prompt** 是 Chat Completion API 中 System Prompt 的等效项。
!!!

System Prompt 定义了模型要遵循的一般指令。它设置了对话的语气和上下文。例如,它告诉模型扮演 AI 助手、写作伙伴或虚构角色。

System Prompt 是 [Story String](context-template.md#story-string) 的一部分,通常是模型接收的 prompt 的第一部分。

请参阅 [prompting guide](index.md#main-prompt-system-prompt) 以了解有关 System Prompt 的更多信息。

## Context Template

!!! Applies to: Text Completion APIs
对于 Chat Completion API 中的等效设置,请使用 [Prompt Manager](prompt-manager.md)。
!!!

通常,AI 模型要求你以某种特定方式向它们提供角色数据。SillyTavern 包含了针对不同模型的预制转换规则列表,但你可以随意自定义它们。

本节的选项在 [Context Template](context-template.md) 中有详细说明。

## Tokenizer

Tokenizer 是将一段文本分解为称为 token 的较小单元的工具。这些 token 可以是单个单词,甚至是单词的一部分,如前缀、后缀或标点符号。一个经验法则是,一个 token 通常对应于 3~4 个文本字符。

本节的选项在 [Tokenizer](tokenizer.md) 中有详细说明。

## Custom Stopping Strings

接受 JSON 序列化的停止字符串数组。例如:`["\n", "\nUser:", "\nChar:"]`。如果你不确定格式,请使用 [在线 JSON 验证器](https://jsonlint.com/)。如果模型输出 **以** 任何停止字符串 **结尾**,它们将从输出中删除。

支持的 API:

1. KoboldAI Classic(版本 1.2.2 及更高版本)或 KoboldCpp
2. AI Horde
3. Text Completion APIs: Text Generation WebUI (ooba)、Tabby、Aphrodite、Mancer、TogetherAI、Ollama 等。
4. NovelAI
5. OpenAI(最多 4 个字符串)和兼容的 API
6. OpenRouter(Text 和 Chat Completion 都支持)
7. Claude
8. Google AI Studio
9. MistralAI

## Start Reply With

!!! Note
默认情况下,Start Reply With 前缀不会在生成的消息中显示。启用 "Show reply prefix in chat" 以显示它。
!!!

### Text Completion APIs

预填充 prompt 的最后一行,强制模型从该点继续。这对于强制执行内容很有用,例如使用定义的前缀推动 [Model Reasoning](/Usage/Prompts/reasoning.md):

```txt
<think>
Sure!
```

### Chat Completion APIs

在 prompt 末尾添加一个 assistant 角色消息。对于某些 backend 模型,这等同于预填充模型响应,但有些可能根本不支持这一点,并会因验证错误而失败。如果你不确定,请将此字段留空。
