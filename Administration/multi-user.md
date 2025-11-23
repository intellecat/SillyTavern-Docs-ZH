---
title: 多用户模式
icon: people
order: -10
route: /administration/multi-user/
---

多用户模式允许多人使用一个 SillyTavern 服务器。每个用户都有自己的设置、扩展和数据。用户账户也可以设置密码保护。

!!!warning
用户密码在多用户设置的用户之间提供基本隐私。它们不是安全功能，不应被视为安全功能。所有用户数据（包括聊天历史、API 密钥和其他敏感信息）都以明文形式存储在服务器上。任何有权访问服务器文件系统的人都可以查看和修改它。**不要在公共服务器上或与不受信任的用户一起使用 SillyTavern。**
!!!

## 配置

要启用和使用多用户模式，请编辑 `config.yaml` 文件：

```yaml
# 启用多用户模式
enableUserAccounts: true
# 启用隐私登录模式：在登录屏幕上隐藏用户列表
enableDiscreetLogin: true
```

1. 当用户账户设置被禁用时，使用 `default-user` 后备管理员账户来存储用户数据。
2. 当隐私登录设置被禁用时，在登录屏幕上显示活动用户列表。如果启用，用户必须手动输入其句柄。

!!!info
您无法从用户列表中_删除_ `default-user` 账户，因为它用于在 `enableUserAccounts` 设置为 `false` 时提供用户数据。但您可以_禁用_它以将其从列表中隐藏并禁止登录。
!!!

## 用户句柄

句柄是用户的唯一标识符。它只能由小写字母、数字和破折号组成。

用户数据目录的路径假定使用以下模式：`%DATA_ROOT%/%USER_HANDLE%`。

有效用户句柄的示例：

- default-user
- juan555
- flux-the-cat
- cool-guy1337

## 角色

- **Admin** - 可以管理（创建、删除、修改）其他用户。可以为所有用户安装扩展。
- **User** - 无法管理其他用户。只能为自己安装扩展。

除了拥有管理面板访问权限外，两种用户角色在功能上是相同的，可以使用 SillyTavern 的全部功能而没有任何限制。用户权限的实现尚待确定。

所有用户账户首先被创建为普通用户，然后如果需要可以提升为管理员。

### 登录屏幕

在那里您可以选择要使用的用户账户。根据 `enableDiscreetLogin` 配置值有两种样式。

当您只有一个活动用户且未设置密码保护时，将绕过登录屏幕且不显示。

### 用户个人资料

您可以使用顶部菜单栏中"用户设置"面板下的"账户"按钮访问账户自我管理菜单。

1. 显示名称 - 在登录屏幕中使用，可以更改。与人格不相关，对 AI API 不可见 - 您仍然可以使用任意数量的人格。
2. 个人资料图片 - 在登录屏幕中使用。您可以使用自定义图片、默认人格图片（如果已设置）或最后使用的人格。
3. 密码 - 锁图标反映账户保护状态（开锁 = 无密码）。可以使用"更改密码"按钮设置、更改或删除密码。
4. 设置快照 - 访问和查看 `settings.json` 文件的备份，并能够创建或恢复快照。
5. 下载备份 - 下载用户数据文件夹的存档。
6. 重置设置 - 重置工厂默认设置，同时保留其他数据（角色、聊天）不变。

## 密码恢复

1. 可以从登录屏幕恢复密码。您需要访问服务器控制台以获取一次性恢复代码（由 4 位数字组成）。
2. 或者，您可以使用 SillyTavern 服务器中的实用程序脚本通过提供用户句柄来重置密码。

```txt
Usage: node recover.js [account] (password)
Example: node recover.js admin SecurePassword
```

## 内容脚手架

要为用户添加自定义内容，您可以使用内容脚手架功能。此功能允许您定义一组文件，这些文件将在服务器启动时复制到每个用户的数据目录。

要使此功能正常工作，您必须在 `/default/scaffold` 目录中创建一个 `index.json` 文件。语法与默认内容相同。所有文件路径应相对于 `/default/scaffold` 目录，您可以使用子目录组织文件。

脚手架文件在默认文件之前复制，这意味着它们将覆盖具有相同文件名的任何默认文件（预设/设置等）。

!!!tip
每个用户数据目录都有一个 `content.log` 文件，列出从脚手架和默认目录复制的所有文件。删除此文件以强制服务器在下次重启时再次同步内容。
!!!

### 识别的内容类型

| 类型                          | 值                |
|-------------------------------|----------------------|
| settings.json                 | `'settings'`         |
| 角色卡                | `'character'`        |
| 角色精灵             | `'sprites'`          |
| 背景图像              | `'background'`       |
| World Info 文件               | `'world'`            |
| 人格头像                | `'avatar'`           |
| UI 主题                      | `'theme'`            |
| ComfyUI workflow              | `'workflow'`         |
| KoboldAI Classic 预设       | `'kobold_preset'`    |
| Chat Completion 预设        | `'openai_preset'`    |
| NovelAI 预设                | `'novel_preset'`     |
| Text Completion 预设        | `'textgen_preset'`   |
| Instruct Mode 模板        | `'instruct'`         |
| Context Formatting 模板   | `'context'`          |
| MovingUI 预设               | `'moving_ui'`        |
| Quick Replies 集             | `'quick_replies'`    |
| System Prompt 模板        | `'sysprompt'`        |
| Reasoning Formatting 模板 | `'reasoning'`        |

### 示例 (`/default/scaffold/index.json`)

```json
[
    {
        "filename": "themes/Midnight.json",
        "type": "theme"
    },
    {
        "filename": "backgrounds/city.png",
        "type": "background"
    },
    {
        "filename": "characters/Charlie.png",
        "type": "character"
    }
]
```
