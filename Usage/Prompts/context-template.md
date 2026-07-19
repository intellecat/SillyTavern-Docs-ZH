---
order: 90
templating: false
route: /usage/prompts/context-template/
---

# 上下文模板

!!! 适用于：文本补全 API
聊天补全 API 中的等效设置，请使用[提示词管理器](prompt-manager.md)。
!!!

AI 模型通常要求以特定方式提供角色数据。SillyTavern 内置了针对多种模型的预制转换规则，你也可以根据需要自由定制。

在「[高级格式设置](advancedformatting.md)」面板中编辑这些设置。

## 故事字符串 {#story-string}

此字段是提示词前言的模板（内部称为故事字符串），是为文本补全和 instruct 模型添加[角色卡](/Usage/Characters/index.md)信息的主要方式。

该模板支持 Handlebars 语法、自定义文本注入或格式化，以及任何其他[宏](/usage/macros.md)。语言参考：<https://handlebarsjs.com/guide/>

以下参数可用于 Handlebars 求值器（用双花括号包裹）：

1. `{{anchorBefore}}`：位置设置为「Before Story String」的提示词。
2. `{{anchorAfter}}`：位置设置为「After Story String」的提示词。
3. `{{description}}`：角色的[描述](/Usage/Characters/characterdesign.md#character-description)。
4. `{{scenario}}`：角色的[场景](/Usage/Characters/characterdesign.md#scenario)。
5. `{{personality}}`：角色的[性格](/Usage/Characters/characterdesign.md#personality-summary)。
6. `{{system}}`：[系统提示词](advancedformatting.md#system-prompt)，或角色的[主提示词](/Usage/Characters/characterdesign.md#prompt-overrides)覆盖（若存在且在用户设置中启用了「优先使用角色提示词」）。
7. `{{persona}}`：所选[人设的描述](/Usage/personas.md#persona-description)。
8. `{{char}}`：角色名称。
9. `{{user}}`：所选人设的名称。
10. `{{wiBefore}}` 或 `{{loreBefore}}`：位置设置为「Before Char Defs」的已激活[世界信息](/Usage/worldinfo.md)条目合并内容。
11. `{{wiAfter}}` 或 `{{loreAfter}}`：位置设置为「After Char Defs」的已激活[世界信息](/Usage/worldinfo.md)条目合并内容。
12. `{{mesExamples}}`：（可选）角色的[示例对话](/Usage/Characters/characterdesign.md#examples-of-dialogue)，以带分隔符的 instruct 格式呈现。
13. `{{mesExamplesRaw}}`：角色的[示例对话](/Usage/Characters/characterdesign.md#examples-of-dialogue)的原始格式，不含任何格式化。

!!!tip **重要**
在故事字符串中使用 `{{mesExamples}}` 时，请将 **<i class="fa-solid fa-user-cog"></i> 用户设置**面板中的**「示例消息行为」**设置为**「从不包含示例」**，以避免在提示词中重复出现示例消息。
!!!

支持特殊的 `{{trim}}` 宏，可移除其周围的换行符。如果希望某段文本紧接前一行而不换行，请使用此宏（_空格**不会**被修剪_）。

**警告**：如果故事字符串模板中缺少上述任何参数，该参数的内容将不会出现在提示词中。

### 提示词锚点

`{{anchorBefore}}` 和 `{{anchorAfter}}` 是通用占位符，用于放置由各种扩展和杂项功能在固定位置添加的提示词，例如：

* [Author's Note](/Usage/Characters/Author's-Note.md)
* [摘要](/extensions/Summarize.md)
* [聊天向量化](/extensions/Chat-vectorization.md) / [数据库](/Usage/Characters/data-bank.md)
* [STscript 注入](/For_Contributors/st-script.md#prompt-injections)
* [网页搜索](/extensions/WebSearch.md)

### 故事字符串位置

默认情况下，渲染后的故事字符串（所有占位符均已替换）位于提示词最开头，后接示例消息和可见的聊天历史。

此外，可通过选择「In-chat @ Depth」选项将其移至动态位置，使故事字符串出现在聊天上下文的特定深度处。

!!!warning **注意**
如果模板包含用于包裹故事字符串的静态提示词元素（特定于模型的前缀或后缀），使用「In-Chat @ Depth」位置会导致它被错误地双重包裹，产生重复序列，可能引发意外结果。

此时可通过以下方式之一解决问题：

1. **内置模板**：按照[高级格式设置](/Usage/Prompts/advancedformatting.md#resetting-templates)中的步骤将模板重置为默认值。
2. **自定义模板**：将故事字符串模板中的静态元素移至[故事字符串序列](/Usage/Prompts/instructmode.md#sequences-story-string-wrapping)。
!!!

### 故事字符串包裹

!!!
以下内容仅在 **Instruct 模式开启**时适用。
!!!

* **Default（默认）**位置：渲染后的故事字符串将使用[故事字符串序列](/Usage/Prompts/instructmode.md#sequences-story-string-wrapping)中定义的序列进行包裹。
* **In-chat @ Depth** 位置：渲染后的故事字符串将使用[聊天消息序列](/Usage/Prompts/instructmode.md#sequences-chat-messages-wrapping)中为所选角色（默认为系统）定义的序列进行包裹。

## 示例分隔符

用作示例对话块的块头和块间分隔符。示例对话中所有的 `<START>` 标签将被替换为此字段的内容。

## 聊天开始

在渲染后的故事字符串之后、示例对话块之后，但在上下文中第一条消息之前插入的分隔符。

## 将分隔符作为停止字符串

将「示例分隔符」和「聊天开始」添加到停止字符串列表中。

在模型倾向于产生幻觉或「泄露」以分隔符开头的完整示例对话块时很有帮助。

## 将名称作为停止字符串

将角色名称和用户人设名称添加到停止字符串列表中。

建议保持开启，以防止模型冒充用户或角色。

## 始终在提示词中添加角色名称

!!!info
当 Instruct 模式开启时，此设置无效。名称行为由所选的[包含名称](/Usage/Prompts/instructmode.md#include-names)选项决定。
!!!

在提示词末尾追加角色名称，强制模型以角色身份续写消息：

```txt
** 其他上下文 **
角色:
```
