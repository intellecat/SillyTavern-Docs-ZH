---
order: 110
icon: smiley
route: /usage/core-concepts/personas/
templating: false
---

# Personas

## 什么是 Persona?

SillyTavern 中的 persona 是你在聊天中使用的身份——本质上是你的显示名称、头像和可选描述文本的组合。Personas 允许你轻松切换你所扮演的角色或"characters",而无需每次都手动更新你的用户名/头像。

!!!
**注意:** 未绑定到 persona 的旧用户头像/名称已被移除。现有数据将迁移到 personas。如果未指定名称,persona 将被命名为"[Unnamed Persona]"。
!!!

## 如何创建 Persona?

1. 打开 **Persona Management** 面板(顶部菜单中的 <i class="fa-solid fa-face-smile"></i> 按钮)。
2. 使用 **Create** 按钮创建一个空白 persona 并为其命名。
3. 在 persona 列表中,选择新创建的 persona。
4. 在右侧,你可以填写描述并通过"Change Persona Image"按钮设置头像。两者都是可选的。
5. 现在你的 persona 已准备好在聊天中使用。

### 将 Character 转换为 Persona

Personas 也可以通过转换任何现有角色来创建。只需打开角色,选择"More..."并点击"Convert to Persona"。将创建一个具有相同名称和描述的 persona。不会使用其他角色卡片字段,如 Scenario 或 Personality。角色不会被删除。

!!! 注意
由于 `{{user}}` 和 `{{char}}` 宏在 Persona 和 Character 描述中使用时具有相反的含义,如果转换的描述包含其中任何一个,系统会提示你交换它们。
!!!

## Persona 描述

每个 persona 可以存储自定义文本描述——心理和身体特征、年龄、职业或任何个人详细信息。这些也可以包括模板宏,如 `{{char}}` 或 `{{user}}`(参见 [Macros](/Usage/Characters/macros.md))。

你的 persona 描述注入到 AI prompt 中的位置取决于 Persona Management 面板中的 **Position** 设置:

- **None (disabled)**
- **In Story String / Prompt Manager** (默认)
- **Top of Author's Note** / **Bottom of Author's Note** (仅在存在 Author's Note 时添加)
- **In Chat @ Depth** (这将打开配置选项以设置深度和角色)

位置是**按 persona** 保存的。

## Persona 标题

标题是一个可选的文本字段,可用于存储关于 persona 的附加信息,不用于 prompt 中,但显示在 Persona Management 面板中。

要设置标题,点击 Persona Management 面板中的 **<i class="fa-solid fa-pencil"></i> Rename Persona** 按钮,并在"Persona Title"字段中输入标题,或在创建 persona 时指定。当标题已存在时设置空值将删除它。

## Persona 连接 / 锁定

Persona connections 确保在特定情况下自动选择给定的 persona。如果没有连接 persona,当前选择的 persona 将保持选中状态。

有三种类型的锁定:

1. **<i class="fa-solid fa-unlock"></i> Chat lock** – Persona 锁定到当前聊天。
2. **<i class="fa-solid fa-unlock"></i> Character lock** – Persona 锁定到特定角色。
3. **<i class="fa-solid fa-crown"></i> Default persona** – 一个在没有其他锁定适用时使用的 persona。

### 1. 锁定到聊天

如果 persona 锁定到聊天,将来打开该聊天时将自动切换到锁定的 persona。

- **要锁定**: 选择所需的 persona,然后点击"Connections"部分下的 **<i class="fa-solid fa-unlock"></i> Chat** 按钮(或使用 `/persona-lock type=chat on`)。
- **要解锁**: 再次点击该按钮(或使用 `/persona-lock type=chat off`)。

### 2. 锁定到角色

你还可以将 persona 链接到特定角色。打开与该角色的任何聊天时会自动选择你锁定的 persona。

- **要锁定**: 选择所需的 persona,然后点击"Connections"部分下的 **<i class="fa-solid fa-unlock"></i> Character** 按钮(或使用 `/persona-lock type=character on`)。
- **要解锁**: 再次点击该按钮(或使用 `/persona-lock type=character off`)。

Persona Management 面板还显示哪些角色链接到该 persona(显示为小头像)。点击它们可直接导航到该角色的聊天。

#### 将多个 Personas 锁定到同一角色

如果另一个 persona 已经链接到该角色,默认情况下它将自动取消链接。

要同时链接多个 personas,可以使用全局设置 **Allow multiple persona connections per character**。
如果多个 personas 链接到同一角色,每次打开或开始与该角色的新聊天时,你都会看到一个弹窗,询问要使用哪个 persona(除非 persona 绑定到聊天)。

### 3. Default Persona

你的 **default persona** 在没有其他相关锁定时使用。default persona 通过其头像周围的黄色边框可识别。

- **要设置/取消设置 default**: 选择所需的 persona,然后点击"Connections"部分下的 **<i class="fa-solid fa-crown"></i> Default** 按钮(或使用 `/persona-lock type=default`)。

只能选择一个 persona 作为 default persona。

### Temporary Persona

如果三个连接选项中的任何一个将 persona 连接到当前角色/聊天,你仍然可以选择使用不同的 persona。此 persona 将在 persona 面板中标记为"Temporary Persona"。任何浏览器窗口重新加载或切换到不同聊天再返回都会将其重置为链接的 persona。

你可以通过将 Temporary Persona 链接到聊天来手动*转换*它以持久连接。

## 全局 Persona 设置

**Current Persona** 下的所有设置都是按 persona 保存的。也存在一些全局设置,可以在 Persona Management 面板的 **Global Persona Settings** 下找到。

1. **Show notifications on switching personas**
   - 启用与 persona 相关的 toast 消息(例如,"Persona Auto Selected"、"Temporary Persona")。

2. **Allow multiple persona connections per character**
   - **启用**时,你可以将多个 personas 链接到单个角色。打开该角色的聊天时会提示你使用哪个 persona。如果禁用,一次只能将一个 persona 连接到一个角色。

3. **Auto-lock a chosen persona to the chat**
   - **启用**时,每当你选择 persona(手动或通过自动选择)或创建新聊天时,它都会将该 persona 锁定到聊天。
   这与"Allow multiple"结合使用,提供了为每个角色选择 persona 的选项,但一旦为聊天选择就保持绑定。

## Personas 的 Slash Commands

### `/persona-lock type=<type?>`

- `chat` 将当前 persona 锁定到你的活动聊天。
- `character` 将当前 persona 锁定到正在使用的角色。
- `none`(或无参数)解锁/清除当前上下文的 persona 锁定。
- 如果不带参数使用,它将返回当前锁定状态(如果未设置则返回错误)。
- 锁定状态可以通过 `on`、`off` 或 `toggle` 选择。默认是 toggle。

### `/persona <name>`

- 通过名称快速切换你的活动 persona,而无需打开 Persona Management 面板。
- 示例: `/persona Blaze`。
- 使用 `mode=temp` 允许临时设置**当前** persona 的名称,即使可能已经存在同名的 persona(保留你当前的头像和描述)。

### `/persona-sync`

- 将活动聊天中的所有用户消息重新归属于**当前** persona 及其名称。

> **注意:** 较旧的 `/lock` 和 `/unlock` 命令为了向后兼容而保留,但可能会在将来被移除。请改用 `/persona-lock`。

## 专业提示

1. **聊天中切换 personas** 不会将你过去的用户消息重新归属于新 persona;这些消息仍归属于你当时使用的 persona。
2. **批量重新归属**: 如果你需要所有先前的消息与新 persona 匹配,点击 **sync** 按钮或使用 `/persona-sync`。
3. **替换 persona 图像**,而不丢失描述或锁定,方法是选择你的 persona 并点击 **<i class="fa-solid fa-images"></i> Change Persona Image** 按钮。
4. **Character link popups**: 如果多个 personas 链接到同一角色,每次打开聊天时你都会看到一个弹窗来选择使用哪个 persona。这是为特定角色提供小范围 personas 选择的便捷方式。
5. **备份**: 你可以使用 Persona Management 中的 **Backup** 按钮备份整个 persona 列表(名称、角色连接、描述),并在需要时稍后恢复。

!!!tip 备份备注

- 图像和聊天连接不会与 personas 一起保存,也不会通过此方式备份。
- 这些备份不是为共享而设计的,因为它们包含内部链接。

!!!
