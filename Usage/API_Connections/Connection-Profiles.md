---
icon: paperclip
route: /usage/core-concepts/connection-profiles/
order: 100
---

# 连接配置

保存连接配置，以便快速在不同的 API、模型和格式化模板之间切换。当你需要同时使用多个 API 连接，或者需要在不同配置之间切换而不想反复翻找菜单时，此功能非常实用。

## 访问连接配置

该功能从 SillyTavern 1.12.6 起作为内置扩展默认启用，可在 API 连接菜单中使用。如需*禁用*，请打开扩展面板，点击「管理扩展」，在列表中找到「连接配置」，取消勾选「已启用」复选框，然后点击「关闭」。

## 保存的内容

连接配置会保存以下选项。

### 通用

* [API 类型、模型和服务器 URL](/Usage/API_Connections/index.md)
* [密钥](/Usage/faq.md#where-are-my-api-keys-stored-why-cant-i-see-them)
* [设置预设](/Usage/Common-Settings.md)
* [回复起始内容](/Usage/Prompts/advancedformatting.md#start-reply-with)（可以明确留空）
* [自定义停止字符串](/Usage/Prompts/advancedformatting.md#custom-stopping-strings)（可以明确留空）
* [推理格式化](/Usage/Prompts/reasoning.md#configuration)

### 文本补全 API

* [系统提示词及其状态](/Usage/Prompts/advancedformatting.md#system-prompt)
* [Instruct 模式状态与模板](/Usage/Prompts/instructmode.md)
* [上下文模板](/Usage/Prompts/advancedformatting.md#context-template)
* [分词器](/Usage/Prompts/advancedformatting.md#tokenizer)

### 聊天补全 API

* [提示词后处理](/Usage/API_Connections/openai.md#prompt-post-processing)
* 代理预设

## 管理连接配置

!!!info 注意
配置仅保存下拉字段中的选项，而不了解底层设置的具体内容。这意味着切换到其他配置时，未保存的更改将会丢失。为避免这种情况，请确保在切换前更新所有预设和模板。
!!!

* 要保存配置，先完成所有必要设置，然后点击「创建」按钮。随后检查设置并为配置命名。**名称必须唯一。**
* 要查看所选配置的详细信息，点击「信息」按钮。再次点击可隐藏详情。
* 连接配置的设置会保存到 `settings.json`，但不会修改关联的配置文件，直到你按下「更新」按钮为止。这意味着如果你设置了某个配置，却在更新前切换到了其他配置，之前的所有更改都会丢失。
* 要从已保存的配置中恢复更改的选项，点击「重载」按钮。
* 要删除配置，点击「删除」按钮并确认删除操作。**此操作不可撤销。**

## 斜杠命令

可以使用以下斜杠命令管理连接配置。

1. `/profile [名称]` - 若提供参数则切换到指定配置，否则返回当前配置的名称。
2. `/profile-create [名称]` - 以提供的名称将当前设置保存为新配置。
3. `/profile-list` - 返回可用配置名称的 JSON 序列化数组。
4. `/profile-get [名称]` - 以 JSON 序列化对象的形式获取指定名称配置的详细信息。
5. `/profile-update` - 用当前设置更新所选配置。
