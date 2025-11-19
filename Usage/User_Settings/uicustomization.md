---
order: 20
route: /usage/core-concepts/uicustomization/
---

# UI 自定义

## UI 主题

### 主题管理

主题文件允许你保存、分享和重用你的 UI 自定义设置。你可以为不同的心情或用途维护多个主题,并在它们之间即时切换。

* 导入/导出主题文件
* 删除现有主题
* 保存更改到当前主题
* 保存为新主题

本节中的所有设置都保存到当前主题。如果你切换主题,这些设置将被新主题的设置替换。

### 显示设置

这些显示选项影响角色和消息在聊天界面中的呈现方式。

#### Avatar Style

在 Circle、Square、Rectangle 或 Rounded Square 之间选择。此设置适用于用户和 AI 头像。

#### Chat Style

| Style        | 描述                                                                                                                                                    | [Slash command](/For_Contributors/st-script.md#ui-styling) |
|--------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------|
| **Flat**     | 简洁连续的"聊天记录"风格,为你的 AI 互动提供平坦的画布。                                                                 | `/flat`<br>`/default`                                      |
| **Bubbles**  | "即时通讯"风格,每条消息都有独特的气泡、令人愉悦的圆角和细微的 3D 效果。                                          | `/bubble`<br>`/bubbles`                                    |
| **Document** | 紧凑的、类似文档的外观,采用以文本为中心的布局。隐藏历史消息的头像、时间戳和消息控制按钮。 | `/single`<br>`/story`                                      |

### Notifications

设置通知弹窗(toast messages)在屏幕上显示的位置。

* Top Left
* Top Center (默认)
* Top Right
* Bottom Left
* Bottom Center
* Bottom Right

### Theme Colors

自定义每个 UI 元素的配色方案以创建你的完美主题。可以使用颜色选择器选择颜色,并在适用的地方包括透明度选项。

* Main Text
* Italics Text
* Underlined Text
* Quote Text
* Text Shadow
* Chat Background
* UI Background
* UI Border
* User Message
* AI Message

### Layout & Visual Settings

使用这些滑块微调界面的视觉呈现。

* **Chat Width**: 调整聊天窗口宽度(屏幕的 25-100%)
* **Font Scale**: 自定义文本大小(0.5-1.5x)
* **Blur Strength**: 控制 UI 面板模糊度(0-30)
* **Shadow Width**: 调整文本阴影强度(0-5)

### Theme Toggles

这些开关控制各种 UI 功能和行为。某些选项可以提高低端设备的性能,而其他选项则为聊天界面添加有用的信息或功能。

* **Reduced Motion**: 禁用动画和过渡效果
* **No Blur Effect**: 移除背景模糊以提高性能
* **No Text Shadows**: 禁用文本阴影效果
* **[Visual Novel mode](Visual-Novel.md)**: 带有背景精灵的紧凑聊天
* **Expand Message Actions**: 始终显示完整的消息上下文菜单
* **Zen Sliders**: 简化的参数控制
* **Mad Lab Mode**: 不受限制的参数范围
* **Message Timer**: 显示 AI 响应生成时间
* **Chat Timestamps**: 显示消息时间戳
* **Model Icons**: 为消息显示 AI 模型图标
* **Message IDs**: 显示顺序消息编号
* **Hide Chat Avatars**: 从聊天中移除头像
* **Message Token Count**: 显示每条消息的 token 数量
* **Compact Input Area**: 单行输入(仅限移动端)
* **Swipe # for All Messages**: 在所有消息上显示滑动编号
* **Characters Hotswap**: 收藏角色的快速选择按钮
* **Avatar Hover Magnification**: 头像悬停时的缩放效果
* **Tags as Folders**: 使用标签作为文件夹组织角色
* **Click to Edit**: 点击消息快速打开消息编辑器

### Custom CSS

允许你应用自定义 CSS 样式以进一步自定义聊天界面的外观。

使用 <i class="fa-fw fa-solid fa-maximize" title="Expand icon"></i> **Expand** 展开编辑器窗口以获得更好的可见性和编辑体验。

如果你切换主题,你的自定义 CSS 将被新主题的自定义 CSS 替换。如果你想在切换主题时保留自定义 CSS,请确保将其保存到主题中。

如果你使用大量自定义 CSS,或者想在多个主题中使用相同的自定义 CSS,非官方的 [CSS Snippets extension](https://github.com/LenAnderson/SillyTavern-CssSnippets) 可以帮助你管理和组织自定义 CSS。

---

## Message Sound

要在收到机器人的新消息时播放你自己的自定义声音,请替换 SillyTavern 文件夹中的以下 MP3 文件:

`public/sounds/message.mp3`

以 80% 音量播放。

如果启用了"[Background Sound Only](index.md#miscellaneous)"选项,则仅在 SillyTavern 窗口**未聚焦**时播放声音。

## Formulas Rendering

要启用数学公式渲染,请使用 [LaTeX extension](https://github.com/SillyTavern/Extension-LaTeX)。要获取该扩展,你需要通过 SillyTavern 中的"Download Extensions & Assets"菜单安装它。

在代码块中输入你的公式,对于 LaTeX 和 AsciiMath 分别使用 `latex` 或 `asciimath` 语言标识符。该扩展使用 [KaTeX](https://katex.org/) 进行渲染。

<pre><code>```latex
\int_{-\infty}^{\infty} e^{-x^2} dx = \sqrt{\pi}
```

```asciimath
int_{-oo}^{oo} e^{-x^2} dx = sqrt{pi}
```</code></pre>

!!!info 弃用通知
不再支持旧的 `$` 和 `$$` 包装语法。请使用以下 regex 脚本来兼容旧语法:

* [$$ - LaTeX](https://github.com/SillyTavern/Extension-LaTeX/raw/refs/heads/main/assets/$$_-_latex.json)
* [$ - AsciiMath](https://github.com/SillyTavern/Extension-LaTeX/raw/refs/heads/main/assets/$_-_asciimath.json)
!!!
