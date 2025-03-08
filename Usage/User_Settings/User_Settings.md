---
order: 120
icon: gear
route: /usage/user-settings
---

# 用户设置


:::callout
**[UI 自定义](uicustomization.md)**

根据您的偏好更改聊天界面的主题、外观和感觉。
:::



:::callout
**[视觉小说模式](Visual-Novel.md)**

像在《心跳文学部》等著名视觉小说游戏中那样，与带有精灵的角色聊天。
:::


## 常规设置

这些是影响您整体 SillyTavern 体验的核心设置。

### UI 语言

SillyTavern 的用户界面提供多种语言。语言选择器提供以下选项：
* **默认**：如果可用，使用您的系统语言
* **英语**：无论系统设置如何，都强制使用英语界面
* 通过下拉菜单提供其他语言

注意：此设置仅影响用户界面文本。对于 AI 对话翻译，请使用 [聊天翻译](../../extensions/Translation.md) 扩展。

### 软件版本

您当前的 SillyTavern 版本显示在右上角。此信息对以下方面至关重要：
* 故障排除
* 确保与扩展的兼容性
* 确定是否有可用更新

要更新 SillyTavern 到最新版本，请参阅 [更新](/Installation/Updating) 文档。

### 账户管理

控制您的 SillyTavern 用户账户，备份您的设置和用户数据，并在[多用户模式](/Administration/multi-user.md)下管理用户角色和权限。

#### <i class="fa-fw fa-solid fa-user-shield"></i> 账户

在账户对话框中，您可以查看和编辑您的个人资料信息，更改密码，并管理账户设置。

**个人资料信息**

* 显示名称（通过铅笔图标编辑）
* 用户头像（也可以使用[角色](/Usage/personas.md)更改）
* 账户标识
* 用户角色
* 账户创建日期
* 密码状态（锁定/解锁图标表示保护状态）

**账户操作**

* **设置快照**：创建、管理和恢复用户设置备份
* **下载备份**：导出所有用户数据的完整备份
* **更改密码**：更新您的账户安全凭证

**危险区域**

需谨慎使用的关键账户操作：
* **重置设置**：将所有设置恢复为出厂默认值
* **重置所有**：完全清除账户并恢复出厂设置

#### <i class="fa-fw fa-solid fa-user-tie"></i> 管理面板

!!! 适用于：[多用户模式](/Administration/multi-user.md)

多账户功能需要在 config.yaml 中将 `enableUserAccounts` 设置为 true。
!!!

选择**管理用户**以查看和管理现有用户账户。

##### 用户资料

- 自定义头像管理（上传/删除）
- 显示名称和标识
- 角色和状态信息
- 账户创建日期
- 密码保护状态

##### 账户控制

- <i class="fa-fw fa-solid fa-pencil"></i> 编辑显示名称
- <i class="fa-fw fa-solid fa-check"></i> 启用账户
- <i class="fa-fw fa-solid fa-ban"></i> 禁用账户
- <i class="fa-fw fa-solid fa-arrow-up"></i> 提升为管理员
- <i class="fa-fw fa-solid fa-arrow-down"></i> 降级为普通用户

##### 管理操作

- <i class="fa-fw fa-solid fa-download"></i> 下载用户数据备份
- <i class="fa-fw fa-solid fa-key"></i> 更改用户密码
- <i class="fa-fw fa-solid fa-trash"></i> 删除账户

##### 新用户

选择**新用户**以创建新用户账户。

* 显示名称*（例如："John Snow"）
* 用户标识*（仅限小写字母、数字和破折号）
* 密码（可选）
* 密码确认

创建新用户时会自动在 /data/ 目录中生成一个以用户标识为名的子文件夹。

#### <i class="fa-fw fa-solid fa-right-from-bracket"></i> 登出

!!! 适用于：[多用户模式](/Administration/multi-user.md)
!!!

退出当前会话。

### 设置搜索

一个方便的搜索栏，帮助您快速找到特定设置：
* 输入任何关键词以过滤和突出显示用户设置中的任何位置的设置
* 搜索设置名称和描述
* 帮助更高效地导航复杂设置

## UI 主题

根据您的偏好更改聊天界面的外观。

有关 <i class="fa-fw fa-solid fa-user-gear" title="用户设置图标"></i> **用户设置**中此部分设置的更多信息，请参阅 [UI 自定义](uicustomization.md#UI-主题)。

## 角色处理

* **角色列表子标题**：选择在 [<i class="fa-fw fa-solid fa-address-card" title="角色图标"></i> 角色](/Usage/Characters/characterdesign.md) 列表中角色名称下显示的其他信息：
    - 角色版本
    - 创建者
* **导入卡片标签**：控制导入角色卡片时如何处理标签：
    - 询问 - 每次导入时显示对话框
    - 无 - 不导入任何标签
    - 全部 - 导入所有标签
    - 现有 - 仅导入已存在的标签
* **高级角色搜索**：启用时，使用模糊匹配并搜索所有角色数据字段，而不仅仅是名称。
* **优先使用角色提示词**：如果启用，在可用时使用角色卡片的系统提示词覆盖。
* **优先使用角色指令**：如果启用，在可用时使用角色卡片的历史后指令覆盖。
* **永不调整头像大小**：防止裁剪/调整导入的角色图像大小。禁用时，图像会调整为 512x768。
* **显示头像文件名**：在角色列表中显示角色头像的实际文件名。
* **防剧透模式**：在编辑器面板中将角色定义隐藏在剧透按钮后面。

## 杂项

* **重新加载聊天**：重新加载并重绘当前聊天。
* **[调试菜单](#debug-menu)**：访问调试选项。
* **平滑流式传输**：实验性功能，用于更平滑的文本生成。包括速度控制滑块。
* **[消息声音](uicustomization.md#消息声音)**：在消息生成完成时播放声音。
    - **仅背景声音**：仅在浏览器标签未聚焦时播放声音。
* **宽松 API URL**：减少 API URL 的格式要求。
* **知识库导入对话框**：导入带有嵌入知识的角色时显示世界信息/知识库导入对话框。
* **自动选择输入文本**：点击某些输入字段时自动选择文本。
* **Markdown 快捷键**：启用 markdown 格式化的键盘快捷键。
* **恢复用户输入**：在页面刷新时保留未保存的用户输入。
* **可移动 UI**：允许通过拖动重新定位 UI 元素（仅限 PC）。
    - <i class="fa-solid fa-recycle" title="重置图标"></i> **重置**按钮以恢复默认位置
    - 用于保存/加载 UI 布局的预设系统

## 聊天/消息处理

### 消息显示设置

控制如何在聊天界面中加载和显示消息。这些设置影响整体聊天体验和性能。
* **加载消息数量**：分页前加载的聊天历史消息数量（0 = 全部）
* **流式传输 FPS**：流式文本的更新速度（5-100 FPS）
* **示例消息行为**：
    - 逐步推出
    - 始终包含示例
    - 永不包含示例

### 输入和响应控制

决定如何发送消息以及 AI 如何继续其响应的设置。
* **回车发送**：选择禁用、自动（PC）或启用
* **"发送"继续**：使用发送按钮继续 AI 响应
* **快速"继续"按钮**：显示扩展 AI 最后消息的按钮
* **快速"扮演"按钮**：显示单消息角色扮演的按钮
* **滑动**：显示替代 AI 响应的箭头按钮（PC 和移动设备）
* **手势**：启用生成的滑动手势（仅限移动设备）

### 自动管理

帮助管理聊天流程和内容的自动化功能。
* **自动加载最后聊天**：启动时自动加载最近的聊天
* **自动滚动聊天**：自动滚动到最新消息
* **自动保存消息编辑**：无需确认即可保存消息编辑
* **确认删除消息**：删除消息前提示
* **自动修复 Markdown**：自动纠正 markdown 格式

#### 自动滑动

根据可配置的标准自动拒绝和重新生成 AI 消息。
* **启用自动滑动**：自动滑动功能的主开关
* **最小生成消息长度**：如果消息短于此值则触发自动滑动
* **黑名单词**：可触发自动滑动的词列表，用逗号分隔
* **触发滑动的黑名单词数**：必须检测到的最小黑名单词数以触发自动滑动

#### 自动继续

如果模型在达到特定长度之前停止，则自动继续响应。

这让您的 AI 可以分多个部分写长响应，因此您可以使用较短的[响应长度设置](/Usage/Common-Settings.md#回应长度tokens)同时仍获得长回复。

它不会让 AI 写出超出其原本会写的内容。要求 AI 继续一个它认为"已完成"的消息通常不会奏效。有关其他想法，请参阅[如何让 AI 写更多？](/Usage/faq.md#如何让-AI-写得更多)。

* **启用自动继续**：自动继续功能的主开关
* **允许聊天完成 API**：为聊天完成 API 端点启用自动继续功能
* **目标长度（tokens）**：所需的消息长度（以 tokens 为单位）- 如果消息短于此值将触发继续（0-1024）

### 消息格式和显示

控制如何格式化消息以及显示什么内容。
* **禁止外部媒体**：阻止来自外部域的嵌入媒体
* **在响应中显示 {\{char}}:**：如果生成则保留响应中的角色名称前缀
* **在响应中显示 {\{user}}:**：如果生成则保留响应中的用户名称前缀
* **在响应中显示标签**：允许在响应中将（某些）HTML 标签显示为 HTML
* **在群组中放宽消息修剪**：允许 AI 在群组聊天中为其他角色发言，而不是停止响应生成
* **显示群组聊天队列**：在群组聊天的角色列表中显示响应顺序

### 提示词检查和调试

* **将提示词记录到控制台**：将提示词输出到浏览器控制台
* **请求 token 概率**：从 API 请求 AI 响应的 token 概率。在可用的情况下，这些可以在 <i class="fa-solid fa-bars" title="菜单图标"></i> [Token 概率](../../Usage/Chatting/index.md#Token-概率面板)中查看。

### 自动完成

- 自动隐藏详情
- 匹配样式（开头匹配/包含/模糊）
- 视觉样式（主题/深色/浅色）
- 键盘选择选项
- 字体缩放
- 宽度控制

## STscript 设置

[STscript 解析器](/For_Contributors/st-script.md#parser-flags)的配置选项。

### STRICT_ESCAPING

* Pipes don't need to be escaped in quoted values.
* A backslash in front of a symbol can be escaped to provide the literal backslash followed by the functional symbol.

See [Strict Escaping](/For_Contributors/st-script.md#strict-escaping) for more information.

### REPLACE_GETVAR

Helps to avoid double-substitutions when the variable values contain text that could be interpreted as macros.

See [Replace Variable Macros](/For_Contributors/st-script.md#replace-variable-macros) for more information.

## Debug menu

!!! warning These functions are intended for advanced users only.

Do not use them unless you fully understand their consequences.
!!!

The Debug Menu provides functionality for troubleshooting, maintenance, and development purposes. These functions should be used with caution as they can significantly impact your SillyTavern installation.

Because extensions can add debug functions, the available options will vary depending on the extensions you have installed.

### Translation & Locale Functions
* **Get missing translations**: Analyzes the current locale (or all locales if English is selected) for missing translations and outputs results to browser console
* **Apply locale**: Forces a refresh of the current language settings by reapplying the selected locale
### Cache & Storage Management
* **Clear WebSearch cache**: Removes all stored search results from local cache
* **Purge all vector indices**: Completely removes all stored vectors across all sources
* **Reset token cache**: Clears stored token counts, forcing complete re-tokenization of all chats
* **Delete itemized prompts**: Removes all itemized prompts from local storage
### Data & Statistics
* **Refresh Stat File**: Rebuilds the statistics file using existing chat data
* **Backfill token counters**: Recalculates token counts for all messages in current chat
    - Useful when switching between models with different tokenizers
    - Triggers chat reload after completion
    - Visual changes only, does not modify chat content
### API & Extension Testing
* **Change Mancer base URL**: Modify the base URL for Mancer API server
* **Test WebSearch extension**: Performs a test search using current settings
* **Send a generation request**: Tests text generation using the currently selected API
### System & Debug Tools
* **Force onboarding**: Restarts the onboarding process
* **Toggle event tracing**: Enables/disables event tracking for debugging
* **Copy ST setup**: [Work in Progress] Copies system configuration data to clipboard for bug reports

Each function can be executed using the "Execute" button beneath its description. Consider backing up your data before using these tools, as some operations cannot be undone.
