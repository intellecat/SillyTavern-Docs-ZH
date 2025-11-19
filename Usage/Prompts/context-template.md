---
order: 90
templating: false
route: /usage/prompts/context-template/
---

# Context 模板

!!! Applies to: Text Completion APIs
对于 Chat Completion API 中的等效设置,请使用 [Prompt Manager](prompt-manager.md)。
!!!

通常,AI 模型要求你以某种特定方式向它们提供角色数据。SillyTavern 包含了针对不同模型的预制转换规则列表,但你可以随意自定义它们。

在 "[Advanced Formatting](advancedformatting.md)" 面板中编辑这些设置。

## Story String

此字段是 prompt 前导(内部称为 story string)的模板。这是为 text completion 和 instruct 模型添加 [Character Cards](/Usage/Characters/index.md) 中定义的信息的主要方式。

该模板支持 Handlebars 语法、自定义文本注入或格式化,以及任何其他 [宏](/Usage/Characters/macros.md)。请参阅此处的语言参考:<https://handlebarsjs.com/guide/>

我们向 Handlebars 评估器提供以下参数(用双花括号包裹):

1. `{{anchorBefore}}`: 设置为使用 "Before Story String" 位置的 Prompt。
2. `{{anchorAfter}}`: 设置为使用 "After Story String" 位置的 Prompt。
3. `{{description}}`: 角色的 [Description](/Usage/Characters/characterdesign.md#character-description)。
4. `{{scenario}}`: 角色的 [Scenario](/Usage/Characters/characterdesign.md#scenario)。
5. `{{personality}}`: 角色的 [Personality](/Usage/Characters/characterdesign.md#personality-summary)。
6. `{{system}}`: [system prompt](advancedformatting.md#system-prompt) 或角色的 [main prompt](/Usage/Characters/characterdesign.md#prompt-overrides) 覆盖(如果存在且在 User Settings 中启用了 "Prefer Char. Prompt")。
7. `{{persona}}`: 选定的 [persona 描述](/Usage/personas.md#persona-description)。
8. `{{char}}`: 角色的名称。
9. `{{user}}`: 选定的 persona 的名称。
10. `{{wiBefore}}` 或 `{{loreBefore}}`: 合并的已激活 [World Info](/Usage/worldinfo.md) 条目,Position 设置为 "Before Char Defs"。
11. `{{wiAfter}}` 或 `{{loreAfter}}`: 合并的已激活 [World Info](/Usage/worldinfo.md) 条目,Position 设置为 "After Char Defs"。
12. `{{mesExamples}}`: (可选)角色的 [Example Dialogues](/Usage/Characters/characterdesign.md#examples-of-dialogue),经过 instruct 格式化并带有分隔符。
13. `{{mesExamplesRaw}}`: 角色的 [Example Dialogues](/Usage/Characters/characterdesign.md#examples-of-dialogue) 原始格式,不带任何格式化。

!!!tip **重要**
在 Story String 中使用 `{{mesExamples}}` 时,将 **<i class="fa-solid fa-user-cog"></i> User Settings** 面板中的 **"Example Messages Behavior"** 设置为 **"Never include examples"**,以避免在 prompt 中重复示例消息。
!!!

支持特殊的 `{{trim}}` 宏来删除围绕它的任何换行符。如果你希望文本的某个部分不与前一行用换行符分隔,请使用它(_空格 **不会** 被修剪_)。

**警告**: 如果 story string 模板中缺少上述任何参数,它们将根本不会在 prompt 中发送。

### Prompt Anchors

`{{anchorBefore}}` 和 `{{anchorAfter}}` 是各种扩展和其他功能在选定的静态位置添加的 prompt 的通用占位符,例如:

* [Author's Note](/Usage/Characters/Author's-Note.md)
* [Summaries](/extensions/Summarize.md)
* [Chat Vectorization](/extensions/Chat-vectorization.md) / [Data Bank](/Usage/Characters/data-bank.md)
* [STscript injections](/For_Contributors/st-script.md#prompt-injections)
* [Web Search](/extensions/WebSearch.md)

### Story String position

默认情况下,渲染的 story string(所有占位符已替换)放置在 prompt 的最开头,后跟示例消息和可见的聊天历史记录。

或者,你可以通过选择 "In-chat @ Depth" 选项将其移动到动态位置,该选项将 story string 放置在聊天上下文中的特定深度。

!!!warning **注意**
如果模板包含用于包裹 story string 的静态 prompt 元素(特定于模型的前缀或后缀),使用 "In-Chat @ Depth" 位置将导致它被错误地双重包裹,带有重复的序列,这可能导致意外结果。

在这种情况下,你可以通过以下方式之一修复问题:

1. **内置模板**: 使用 [Advanced Formatting](/Usage/Prompts/advancedformatting.md#resetting-templates) 中描述的步骤将模板重置为默认值。
2. **自定义模板**: 将静态元素从 story string 模板移动到 [Story String Sequences](/Usage/Prompts/instructmode.md#sequences-story-string-wrapping)。
!!!

### Story String wrapping

!!!
以下部分仅在 **Instruct Mode** 开启时适用。
!!!

* **Default** 位置: 渲染的 Story String 将使用 [Story String Sequences](/Usage/Prompts/instructmode.md#sequences-story-string-wrapping) 中定义的序列进行包裹。
* **In-chat @ Depth** 位置: 渲染的 Story String 将使用为选定角色(默认:System)在 [Chat Messages Sequences](/Usage/Prompts/instructmode.md#sequences-chat-messages-wrapping) 中定义的序列进行包裹。

## Example Separator

用作块标题和示例对话块之间的分隔符。示例对话中 `<START>` 标签的任何实例都将替换为此字段的内容。

## Chat Start

作为分隔符插入在渲染的 story string 之后和示例对话块之后,但在上下文中的第一条消息之前。

## Separators as Stop Strings

将 "Example Separator" 和 "Chat Start" 添加到停止字符串列表中。

如果模型倾向于幻觉或泄漏整个示例对话块(前面有分隔符),这将很有帮助。

## Names as Stop Strings

将 Character 和 User Persona 名称添加到停止字符串列表中。

建议保持启用以防止模型冒充。

## Always add character's name to prompt

!!!info
当 Instruct Mode 开启时,此设置无效。名称行为由选定的 [Include Names](/Usage/Prompts/instructmode.md#include-names) 选项定义。
!!!

将角色的名称附加到 prompt 以强制模型以角色身份完成消息:

```txt
** OTHER CONTEXT HERE **
Character:
```
