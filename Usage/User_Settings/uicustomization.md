---
order: 20
route: /usage/core-concepts/uicustomization/
---
---

## Message Sound

To play your own custom sound on receiving a new message from bot, replace the following MP3 file in your SillyTavern folder:

`public/sounds/message.mp3`

Plays at 80% volume.

If the "[Background Sound Only](index.md#miscellaneous)" option is enabled, the sound plays only if SillyTavern window is **unfocused**.

## Formulas Rendering

To enable math formulas rendering, use the [LaTeX extension](https://github.com/SillyTavern/Extension-LaTeX). To get the extension, you need to install it via the "Download Extensions & Assets" menu in SillyTavern.

Type your formulas in code blocks with `latex` or `asciimath` language identifiers for LaTeX and AsciiMath respectively. The extension uses [KaTeX](https://katex.org/) for rendering.

<pre><code>```latex
\int_{-\infty}^{\infty} e^{-x^2} dx = \sqrt{\pi}
```

```asciimath
int_{-oo}^{oo} e^{-x^2} dx = sqrt{pi}
```</code></pre>

!!!info Deprecation notice
The legacy `$` and `$$` wrapper syntax is no longer supported. Please use the following regex scripts to polyfill the old syntax:

* [$$ - LaTeX](https://github.com/SillyTavern/Extension-LaTeX/raw/refs/heads/main/assets/$$_-_latex.json)
* [$ - AsciiMath](https://github.com/SillyTavern/Extension-LaTeX/raw/refs/heads/main/assets/$_-_asciimath.json)
---


# UI 自定义

## UI 主题

### 主题管理
主题文件允许您保存、共享和重复使用您的 UI 自定义设置。您可以维护多个主题以适应不同的心情或用途，并在它们之间即时切换。

* 导入/导出主题文件
* 删除现有主题
* 保存对当前主题的更改
* 另存为新主题

本节中的所有设置都保存到当前主题。如果您切换主题，这些设置将被新主题的设置替换。

### 显示设置
这些显示选项影响角色和消息在聊天界面中的呈现方式。

#### 头像样式
在圆形、方形或矩形之间选择。

#### 聊天样式
| 样式        | 描述                                                                                                                                                    | [斜杠命令](/For_Contributors/st-script.md#ui-styling) |
|--------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------|
| **平面**     | 干净连续的"聊天日志"样式，一个让您的 AI 互动栩栩如生的平面画布。                                                                 | `/flat`<br>`/default`                                      |
| **气泡**  | "即时通讯"样式，每条消息都有独特的气泡，令人愉悦的圆角和微妙的 3D 效果。                                          | `/bubble`<br>`/bubbles`                                    |
| **文档** | 紧凑的文档式外观，带有点击编辑模式和以文本为中心的布局。隐藏过去消息的头像、时间戳和消息控制按钮。 | `/single`<br>`/story`                                      |

### 主题颜色
自定义每个 UI 元素的配色方案，创建您完美的主题。可以使用颜色选择器选择颜色，并在适用的情况下包括透明度选项。

* 主要文本
* 斜体文本
* 下划线文本
* 引用文本
* 文本阴影
* 聊天背景
* UI 背景
* UI 边框
* 用户消息
* AI 消息

### 布局和视觉设置
使用这些滑块微调界面的视觉呈现。

* **聊天宽度**：调整聊天窗口宽度（屏幕的 25-100%）
* **字体缩放**：自定义文本大小（0.5-1.5 倍）
* **模糊强度**：控制 UI 面板模糊度（0-30）
* **阴影宽度**：调整文本阴影强度（0-5）

### 主题开关
这些开关控制各种 UI 功能和行为。一些选项可以提高低端设备的性能，而其他选项则为聊天界面添加有用的信息或功能。
* **减少动画**：禁用动画和过渡效果
* **无模糊效果**：移除背景模糊以提高性能
* **无文本阴影**：禁用文本阴影效果
* **[视觉小说模式](Visual-Novel.md)**：带背景精灵的紧凑聊天
* **展开消息操作**：始终显示完整的消息上下文菜单
* **禅意滑块**：简化的参数控制
* **疯狂实验室模式**：无限制的参数范围
* **消息计时器**：显示 AI 回应生成时间
* **聊天时间戳**：显示消息时间戳
* **模型图标**：显示消息的 AI 模型图标
* **消息 ID**：显示顺序消息编号
* **隐藏聊天头像**：从聊天中移除头像
* **消息 Token 计数**：显示每条消息的 token 数
* **紧凑输入区域**：单行输入（仅限移动设备）
* **所有消息显示滑动编号**：在所有消息上显示滑动编号（移动设备）
* **角色快速切换**：收藏角色的快速选择按钮
* **头像悬停放大**：悬停时的头像缩放效果
* **标签作为文件夹**：使用标签作为文件夹组织角色


### 自定义 CSS

允许您应用自定义 CSS 样式以进一步自定义聊天界面的外观。

使用 <i class="fa-fw fa-solid fa-maximize" title="Expand icon"></i> **展开** 来扩展编辑器窗口以获得更好的可见性和编辑体验。

如果您切换主题，您的自定义 CSS 将被新主题的自定义 CSS 替换。如果您想在切换主题时保留您的自定义 CSS，请确保将其保存到主题中。

如果您使用大量自定义 CSS，或想在多个主题中使用相同的自定义 CSS，非官方的 [CSS Snippets 扩展](https://github.com/LenAnderson/SillyTavern-CssSnippets) 可以帮助您管理和组织您的自定义 CSS。

## 消息声音

要在收到机器人的新消息时播放您自己的自定义声音，请替换 SillyTavern 文件夹中的以下 MP3 文件：

`public/sounds/message.mp3`

以 80% 音量播放。

如果启用了"[仅背景声音](User_Settings.md#杂项)"选项，则仅在 SillyTavern 窗口**未聚焦**时播放声音。

有关更多信息，请参阅[杂项](User_Settings.md#杂项)。


## 公式渲染

要启用数学公式渲染，请使用 [LaTeX 扩展](https://github.com/SillyTavern/Extension-LaTeX)。要获取该扩展，您需要通过 SillyTavern 中的"下载扩展和资源"菜单安装它。

在代码块中使用 `latex` 或 `asciimath` 语言标识符分别输入 LaTeX 和 AsciiMath 公式。该扩展使用 [KaTeX](https://katex.org/) 进行渲染。

<pre><code>```latex
\int_{-\infty}^{\infty} e^{-x^2} dx = \sqrt{\pi}
```

```asciimath
int_{-oo}^{oo} e^{-x^2} dx = sqrt{pi}
```</code></pre>

!!! info 弃用通知
不再支持传统的 `$` 和 `$$` 包装语法。请使用以下正则表达式脚本来兼容旧语法：

* [$$ - LaTeX](https://github.com/SillyTavern/Extension-LaTeX/raw/refs/heads/main/assets/$$_-_latex.json)
* [$ - AsciiMath](https://github.com/SillyTavern/Extension-LaTeX/raw/refs/heads/main/assets/$_-_asciimath.json)
!!!
