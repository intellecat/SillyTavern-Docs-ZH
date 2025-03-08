---
order: character-14
route: /usage/core-concepts/macros
---

# 宏（替换标签）

!!! 注意
此列表可能不完整或已过时。在任何 SillyTavern 聊天中使用 `/help macros` 斜杠命令来获取您的实例中可用的宏列表。
!!!

宏可以在角色描述、作者注释、世界信息和许多其他地方使用，并在生成回应时替换为相应的值。它们可以用于在提示词中插入动态内容，如用户名称、角色描述或当前时间。宏用双大括号括起来，例如 `{{user}}`，通常不区分大小写。**请注意，目前不支持宏嵌套。**

注意：某些扩展可能还会添加特定于上下文的特殊宏，这些宏只在某些区域有效（即扩展提示词的特殊占位符）。除非该宏不绑定到特定功能，否则这里不会记录这些宏。

## 通用宏

| 宏 | 描述 |
|-------|-------------|
| `{{pipe}}` | 仅用于斜杠命令批处理。替换为前一个命令的返回结果。 |
| `{{newline}}` | 插入换行符。 |
| `{{trim}}` | 修剪此宏周围的换行符。 |
| `{{noop}}` | 无操作，只是一个空字符串。 |
| `{{user}}` 或 `<USER>` | 用户名称。 |
| `{{charPrompt}}` | 角色的主提示词覆盖。 |
| `{{charJailbreak}}` | 角色的历史后指令提示词覆盖。 |
| `{{group}}` 或 `{{charIfNotGroup}}` | 群组成员名称的逗号分隔列表，或单人聊天中的角色名称。 |
| `{{groupNotMuted}}` | 与 `{{group}}` 相同但不包括已静音的成员。 |
| `{{char}}` 或 `<BOT>` | 角色名称。 |
| `{{description}}` | 角色描述。 |
| `{{scenario}}` | 角色场景或聊天场景覆盖（如果已设置）。 |
| `{{personality}}` | 角色个性。 |
| `{{persona}}` | 用户的个性描述。 |
| `{{mesExamples}}` | 角色的对话示例（未更改和未分割）。 |
| `{{char_version}}` | 角色的版本号。 |
| `{{model}}` | 当前选择的 API 的文本生成模型名称。**可能不准确！** |
| `{{lastMessageId}}` | 最后一条聊天消息的 ID。 |
| `{{lastMessage}}` | 最后一条聊天消息的文本。 |
| `{{firstIncludedMessageId}}` | 上下文中包含的第一条消息的 ID。需要在当前会话中至少运行一次生成。 |
| `{{lastCharMessage}}` | 角色发送的最后一条聊天消息。 |
| `{{lastUserMessage}}` | 用户发送的最后一条聊天消息。 |
| `{{currentSwipeId}}` | 当前显示的最后一条消息滑动的基于 1 的 ID。 |
| `{{lastSwipeId}}` | 最后一条聊天消息的滑动次数。 |
| `{{lastGenerationType}}` | 最后一个排队的生成请求的类型。值：normal（正常）、impersonate（扮演）、regenerate（重新生成）、quiet（静默）、swipe（滑动）、continue（继续）。 |
| `{{original}}` | 可以在提示词覆盖字段中使用，以包含系统设置中的默认提示词。仅适用于聊天补全 API 和指令模式。 |
| `{{time}}` | 当前系统时间。 |
| `{{time_UTC±X}}` | 指定 UTC 偏移（时区）的当前时间，例如对于 UTC+02:00 使用 `{{time_UTC+2}}`。 |
| `{{timeDiff::(time1)::(time2)}}` | time1 和 time2 之间的时间差。接受时间和日期宏。 |
| `{{date}}` | 当前系统日期。 |
| `{{input}}` | 用户输入栏的内容。 |
| `{{weekday}}` | 当前星期几。 |
| `{{isotime}}` | 当前 ISO 时间（24 小时制）。 |
| `{{isodate}}` | 当前 ISO 日期（YYYY-MM-DD）。 |
| `{{datetimeformat ...}}` | 指定格式的当前日期/时间（例如 `{{datetimeformat DD.MM.YYYY HH:mm}}`）。 |
| `{{idle_duration}}` | 插入自上次发送用户消息以来的时间范围的人性化字符串（示例：4 小时，1 天）。 |
| `{{random:(args)}}` | 从列表中返回一个随机项（例如 `{{random:1,2,3,4}}` 将随机返回 4 个数字中的 1 个）。 |
| `{{random::arg1::arg2}}` | random 的替代语法，支持其参数中的逗号。 |
| `{{pick::(args)}}` | random 的替代方案，但如果源字符串保持不变，则在当前聊天的后续评估中选择的参数是稳定的。 |
| `{{roll:(formula)}}` | 使用 D&D 骰子语法生成随机值：XdY+Z（例如 `{{roll:d6}}` 生成 1-6 的值）。 |
| `{{bias "text here"}}` | 为 AI 设置行为偏差，直到下一个用户输入。文本周围需要引号。 |
| `{{// (note)}}` | 允许留下将被替换为空内容的注释。AI 不可见。 |
| `{{banned "text here"}}` | 动态将引号中的文本添加到 Text Generation WebUI 后端的禁用词序列中。对其他后端无效。需要引号。 |
| `{{reverse:(content)}}` | 反转宏的内容。 |

## 指令模式和上下文模板宏

（在高级格式设置中启用）

| 宏 | 描述 |
|-------|-------------|
| `{{exampleSeparator}}` | 上下文模板示例对话分隔符。 |
| `{{chatStart}}` | 上下文模板聊天开始行。 |
| `{{instructSystemPrompt}}` | 指令系统提示词。 |
| `{{instructSystemPromptPrefix}}` | 系统提示词前缀序列。 |
| `{{instructSystemPromptSuffix}}` | 系统提示词后缀序列。 |
| `{{instructUserPrefix}}` | 用户消息前缀序列。 |
| `{{instructAssistantPrefix}}` | 助手消息前缀序列。 |
| `{{instructSystemPrefix}}` | 系统消息前缀序列。 |
| `{{instructUserSuffix}}` | 用户消息后缀序列。 |
| `{{instructAssistantSuffix}}` | 助手消息后缀序列。 |
| `{{instructSystemSuffix}}` | 系统消息后缀序列。 |
| `{{instructFirstAssistantPrefix}}` | 助手第一个输出序列。 |
| `{{instructLastAssistantPrefix}}` | 助手最后一个输出序列。 |
| `{{instructFirstUserPrefix}}` | 指令用户第一个输入序列。 |
| `{{instructLastUserPrefix}}` | 指令用户最后一个输入序列。 |
| `{{instructSystemInstructionPrefix}}` | 系统指令前缀序列。 |
| `{{instructUserFiller}}` | 用户填充消息文本。 |
| `{{instructStop}}` | 指令停止序列。 |
| `{{maxPrompt}}` | 提示词的最大 token 数（上下文长度减去回应长度）。 |
| `{{systemPrompt}}` | 系统提示词内容，如果允许且可用，包括角色提示词覆盖。 |
| `{{defaultSystemPrompt}}` | 系统提示词内容（不包括角色提示词覆盖）。 |

## 聊天变量宏

- 本地变量 = 仅在当前聊天中唯一
- 全局变量 = 在任何角色的任何聊天中都有效

| 宏 | 描述 |
|-------|-------------|
| `{{getvar::name}}` | 替换为本地变量"name"的值。 |
| `{{setvar::name::value}}` | 替换为空字符串，将本地变量"name"设置为"value"。 |
| `{{addvar::name::increment}}` | 替换为空字符串，将数值"increment"添加到本地变量"name"。 |
| `{{incvar::name}}` | 替换为变量"name"的值加 1 的结果。 |
| `{{decvar::name}}` | 替换为变量"name"的值减 1 的结果。 |
| `{{getglobalvar::name}}` | 替换为全局变量"name"的值。 |
| `{{setglobalvar::name::value}}` | 替换为空字符串，将全局变量"name"设置为"value"。 |
| `{{addglobalvar::name::value}}` | 替换为空字符串，将数值"increment"添加到全局变量"name"。 |
| `{{incglobalvar::name}}` | 替换为全局变量"name"的值加 1 的结果。 |
| `{{decglobalvar::name}}` | 替换为全局变量"name"的值减 1 的结果。 |
| `{{var::name}}` | 替换为作用域变量"name"的值（仅限 STscript）。 |
| `{{var::name::index}}` | 替换为作用域变量"name"中索引处的值（用于 STscript 中的数组/对象）。 |

## 扩展特定宏

由扩展添加，仅在特定条件下工作。

| 宏 | 描述 |
|-------|-------------|
| `{{summary}}` | 替换为当前聊天会话的摘要（如果可用）。 |
