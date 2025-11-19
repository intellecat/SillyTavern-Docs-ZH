---
icon: paperclip
route: /usage/core-concepts/connection-profiles/
order: 100
---

# Connection Profiles

保存 Connection Profiles 以快速切换不同的 API、模型和格式化模板。当您主动使用多个 API 连接或需要在不同配置之间切换而不必浏览菜单时,这非常有用。

## 访问 Connection Profiles

从 SillyTavern 1.12.6 或更高版本开始,此功能作为内置扩展默认启用,并在 API Connections 菜单中可用。如果您希望*禁用*它,请打开 Extensions 面板,点击 "Manager extensions",在列表中找到 Connection Profiles,取消选中 "Enabled" 复选框,然后点击 "Close"。

## 保存的内容

Connection Profiles 存储以下选择。

### 通用

* [API type、model 和 server URL](/Usage/API_Connections/index.md)
* [Secret Key](/Usage/faq.md#where-are-my-api-keys-stored-why-cant-i-see-them)
* [Settings preset](/Usage/Common-Settings.md)
* [Start Reply With](/Usage/Prompts/advancedformatting.md#start-reply-with)(可以明确为空)
* [Custom Stopping Strings](/Usage/Prompts/advancedformatting.md#custom-stopping-strings)(可以明确为空)
* [Reasoning Formatting](/Usage/Prompts/reasoning.md#configuration)

### Text Completion APIs

* [System Prompt 及其状态](/Usage/Prompts/advancedformatting.md#system-prompt)
* [Instruct Mode 状态和 template](/Usage/Prompts/instructmode.md)
* [Context Template](/Usage/Prompts/advancedformatting.md#context-template)
* [Tokenizer](/Usage/Prompts/advancedformatting.md#tokenizer)

### Chat Completion APIs

* [Prompt Post-Processing](/Usage/API_Connections/openai.md#prompt-post-processing)
* Proxy preset

## 管理 Connection Profiles

!!!info 注意
Profiles 仅保存下拉字段中的选择,而不了解底层设置。这意味着切换到不同的 profile 会丢失未保存的更改。为防止这种情况,如果您不想丢失临时更改,请确保更新所有 presets 和 templates。
!!!

* 要保存 profile,请设置所有必需的设置并点击 "Create" 按钮。然后查看设置并为 profile 提供名称。**名称应该是唯一的。**
* 要查看所选 profile 的详细信息,请点击 "Information" 按钮。再次点击以隐藏详细信息。
* Connection Profile 设置会保存到 `settings.json` 中,而不会更改关联的 profile 保存文件,直到您按下 "Update" 按钮。这意味着如果您设置了一个 profile,但随后切换到另一个而未更新,您将丢失所有先前的更改。
* 要从保存的 profile 恢复更改的选择,请点击 "Reload" 按钮。
* 要删除 profile,请点击 "Delete" 按钮并确认删除。**此操作不可逆。**

## Slash Commands

可以使用以下 slash commands 管理 Connection profiles。

1. `/profile [name]` - 如果提供了参数则切换到 profile,如果未提供则获取当前 profile 的名称。
2. `/profile-create [name]` - 将当前设置保存为具有提供名称的新 profile。
3. `/profile-list` - 返回可用 profile 名称的 JSON 序列化数组。
4. `/profile-get [name]` - 以 JSON 序列化对象的形式获取具有提供名称的 profile 的详细信息。
5. `/profile-update` - 使用当前设置更新所选 profile。
