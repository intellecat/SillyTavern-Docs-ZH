---
order: 90
route: /usage/core-concepts/macros/
---

# Macros（替换标签）

!!! Note
此列表可能不完整或已过时。在任何 SillyTavern 聊天中使用 `/help macros` slash 命令以获取在您的实例中有效的 macros 列表。
!!!

Macros 可以在 character description、author's notes、world info 和许多其他地方使用，并在生成响应时替换为相应的值。它们可用于将动态内容插入 prompt，例如 user 的名字、character 的描述或当前时间。Macros 用双花括号括起来，例如 `{{user}}`，通常不区分大小写。**请记住，目前不支持 macro 嵌套。**

注意：某些 extensions 可能还会添加仅在某些区域有效的特殊上下文特定 macros（即 extension prompts 的特殊占位符）。除非 macro 不绑定到特定功能，否则这些不会在此处记录。

## 通用 Macros

| Macro | 描述 |
|-------|------|
| `{{pipe}}` | 仅用于 slash command 批处理。替换为上一个命令的返回结果。 |
| `{{newline}}` | 插入换行符。 |
| `{{trim}}` | 修剪此 macro 周围的换行符。 |
| `{{noop}}` | 无操作，只是一个空字符串。 |
| `{{user}}` 或 `<USER>` | User 的名字。 |
| `{{charPrompt}}` | Character 的 Main Prompt 覆盖。 |
| `{{charJailbreak}}` | Character 的 Post-History Instructions Prompt 覆盖。 |
| `{{group}}` 或 `{{charIfNotGroup}}` | 逗号分隔的 group 成员名称列表或单独聊天中的 character 名称。 |
| `{{groupNotMuted}}` | 与 `{{group}}` 相同，但排除静音的成员。 |
| `{{notChar}}` | 除当前发言者（`{{char}}`）之外的所有聊天参与者的逗号分隔列表。在 group chats 中，这仍然包括静音的 characters，并且在没有生成消息时列出名单中的每个 character。 |
| `{{char}}` 或 `<BOT>` | Character 的名字。 |
| `{{description}}` | Character 的 description。 |
| `{{scenario}}` | Character 的 scenario 或 chat scenario 覆盖（如果已设置）。 |
| `{{personality}}` | Character 的 personality。 |
| `{{persona}}` | User 的 persona description。 |
| `{{mesExamples}}` | Character 的对话示例（instruct 格式化）。 |
| `{{mesExamplesRaw}}`  | Character 的对话示例（未更改且未拆分）。 |
| `{{charVersion}}` | Character 的版本号。 |
| `{{charDepthPrompt}}` | Character 的 at-depth prompt。 |
| `{{model}}` | 当前选定 API 的文本生成模型名称。**可能不准确！** |
| `{{lastMessageId}}` | 最后一条聊天消息 ID。 |
| `{{lastMessage}}` | 最后一条聊天消息文本。 |
| `{{firstIncludedMessageId}}` | 包含在 context 中的第一条消息的 ID。需要在当前会话中至少运行一次生成。 |
| `{{lastCharMessage}}` | character 发送的最后一条聊天消息。 |
| `{{lastUserMessage}}` | user 发送的最后一条聊天消息。 |
| `{{currentSwipeId}}` | 当前显示的最后一条消息滑动的基于 1 的 ID。 |
| `{{lastSwipeId}}` | 最后一条聊天消息中的滑动数量。 |
| `{{lastGenerationType}}` | 最后排队的生成请求的类型。值："normal"、"impersonate"、"regenerate"、"quiet"、"swipe"、"continue"。 |
| `{{original}}` | 可用于 Prompt Overrides 字段以包含系统设置中的默认 prompt。仅适用于 Chat Completion APIs 和 Instruct mode。 |
| `{{time}}` | 当前系统时间。 |
| `{{time_UTC±X}}` | 指定 UTC 偏移量（时区）中的当前时间，例如对于 UTC+02:00 使用 `{{time_UTC+2}}`。 |
| `{{timeDiff::(time1)::(time2)}}` | time1 和 time2 之间的时间差。接受时间和日期 macros。 |
| `{{date}}` | 当前系统日期。 |
| `{{input}}` | user 输入栏的内容。 |
| `{{weekday}}` | 当前星期几。 |
| `{{isotime}}` | 当前 ISO 时间（24 小时制）。 |
| `{{isodate}}` | 当前 ISO 日期（YYYY-MM-DD）。 |
| `{{datetimeformat ...}}` | 指定格式的当前日期/时间（例如 `{{datetimeformat DD.MM.YYYY HH:mm}}`）。 |
| `{{idle_duration}}` | 插入自上次发送 user 消息以来的时间范围的人性化字符串（示例：4 hours、1 day）。 |
| `{{random:(args)}}` | 从列表中返回随机项（例如 `{{random:1,2,3,4}}` 将随机返回 4 个数字中的 1 个）。 |
| `{{random::arg1::arg2}}` | random 的替代语法，在其参数中支持逗号。 |
| `{{pick::(args)}}` | random 的替代方案，但如果源字符串保持不变，则在当前聊天中后续评估时选定的参数是稳定的。 |
| `{{roll:(formula)}}` | 使用 D&D 骰子语法生成随机值：XdY+Z（例如 `{{roll:d6}}` 生成 1-6 的值）。 |
| `{{bias "text here"}}` | 为 AI 设置行为偏差，直到下一次 user 输入。需要文本周围的引号。 |
| `{{// (note)}}` | 允许留下将被空白内容替换的笔记。对 AI 不可见。 |
| `{{banned "text here"}}` | 为 Text Generation WebUI backend 动态添加引用文本到禁用词序列。对其他 backends 不执行任何操作。需要引号。 |
| `{{reverse:(content)}}` | 反转 macro 的内容。 |
| `{{outlet::(name)}}` | 替换为命名的 [World Info outlet](/Usage/worldinfo.md#outlet-name) 的内容，将包含由换行符分隔的激活条目。 |

## Instruct Mode 和 Context Template Macros

（在 Advanced Formatting settings 中启用）

| Macro | 描述 |
|-------|------|
| `{{exampleSeparator}}` | Context template 示例对话分隔符。 |
| `{{chatStart}}` | Context template 聊天开始行。 |
| `{{instructSystemPrompt}}` | Instruct system prompt。 |
| `{{instructSystemPromptPrefix}}` | System prompt 前缀序列。 |
| `{{instructSystemPromptSuffix}}` | System prompt 后缀序列。 |
| `{{instructUserPrefix}}` | User 消息前缀序列。 |
| `{{instructAssistantPrefix}}` | Assistant 消息前缀序列。 |
| `{{instructSystemPrefix}}` | System 消息前缀序列。 |
| `{{instructUserSuffix}}` | User 消息后缀序列。 |
| `{{instructAssistantSuffix}}` | Assistant 消息后缀序列。 |
| `{{instructSystemSuffix}}` | System 消息后缀序列。 |
| `{{instructFirstAssistantPrefix}}` | Assistant 第一个输出序列。 |
| `{{instructLastAssistantPrefix}}` | Assistant 最后一个输出序列。 |
| `{{instructFirstUserPrefix}}` | Instruct user 第一个输入序列。 |
| `{{instructLastUserPrefix}}` | Instruct user 最后一个输入序列。 |
| `{{instructSystemInstructionPrefix}}` | System instruction 前缀序列。 |
| `{{instructUserFiller}}` | User filler 消息文本。 |
| `{{instructStop}}` | Instruct stop 序列。 |
| `{{maxPrompt}}` | Prompt 的最大大小（以 tokens 为单位）（context 长度减去响应长度）。 |
| `{{systemPrompt}}` | System prompt 内容，包括 character prompt 覆盖（如果允许且可用）。 |
| `{{defaultSystemPrompt}}` | System prompt 内容（不包括 character prompt 覆盖）。 |

## Chat variables Macros

- Local variables = 当前聊天独有
- Global variables = 适用于任何 character 的任何聊天

| Macro | 描述 |
|-------|------|
| `{{getvar::name}}` | 替换为 local variable "name" 的值。 |
| `{{setvar::name::value}}` | 替换为空字符串，将 local variable "name" 设置为 "value"。允许空值。 |
| `{{addvar::name::increment}}` | 替换为空字符串，将 "increment" 的数值添加到 local variable "name"。 |
| `{{incvar::name}}` | 替换为将 variable "name" 的值递增 1 的结果。 |
| `{{decvar::name}}` | 替换为将 variable "name" 的值递减 1 的结果。 |
| `{{getglobalvar::name}}` | 替换为 global variable "name" 的值。 |
| `{{setglobalvar::name::value}}` | 替换为空字符串，将 global variable "name" 设置为 "value"。允许空值。 |
| `{{addglobalvar::name::value}}` | 替换为空字符串，将 "increment" 的数值添加到 global variable "name"。 |
| `{{incglobalvar::name}}` | 替换为将 global variable "name" 的值递增 1 的结果。 |
| `{{decglobalvar::name}}` | 替换为将 global variable "name" 的值递减 1 的结果。 |
| `{{var::name}}` | 替换为 scoped variable "name" 的值（仅限 STscript）。 |
| `{{var::name::index}}` | 替换为 scoped variable "name" 的索引处的值（用于 STscript 中的数组/对象）。 |

## Extension 特定 Macros

由 extensions 添加，仅在特定条件下有效。

| Macro | 描述 |
|-------|------|
| `{{summary}}` | 替换为当前聊天会话的摘要（如果可用）。 |
| `{{authorsNote}}` | 替换为 Author's Note 的内容。 |
| `{{charAuthorsNote}}` | 替换为 Character's Author's Note 的内容。 |
| `{{defaultAuthorsNote}}` | 替换为默认 Author's Note 的内容。 |
| `{{charPrefix}}` | 替换为 character 特定的 Image Generation positive prompt 前缀（如果可用）。 |
| `{{charNegativePrefix}}` | 替换为 character 特定的 Image Generation negative prompt 前缀（如果可用）。 |
