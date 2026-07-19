# 作者注释

## 什么是作者注释？

作者注释是一个强大的自定义 AI 响应的工具，它可以在提示词的任意位置、以任意频率插入一段文本。

## 使用方法

作者注释可以在聊天输入栏左侧的选项菜单中找到。

| 选项菜单                              | 作者注释面板                           |
|---------------------------------------|----------------------------------------|
| ![](/static/extensions/note-menu.png) | ![](/static/extensions/note-panel.png) |

## 配置作者注释

### 特定聊天的作者注释

作者注释面板顶部的文本框包含当前聊天的作者注释。

**此文本框的内容不会自动转移到任何新聊天中。**

### 插入位置选项

#### 场景之后

将作者注释插入到上下文中角色定义"场景"部分之后的位置。如果未指定场景，它将被插入到角色定义最后一部分之后、示例消息之前。

#### 聊天内

将作者注释以指定深度插入到聊天历史记录中。

深度 0 = 插入到聊天历史记录的最末尾。

深度 4 = 插入到最近 3 条聊天历史消息之前，成为聊天历史记录中的第 4 个元素。

_作者注释在提示词中越靠近底部，对下一次 AI 响应的影响越大。_

### 插入频率

设置作者注释被包含到聊天中的频率。

频率 0 = 永远不插入作者注释。

频率 1 = 每次用户输入提示时都插入作者注释。

频率 4 = 每隔 4 次用户输入提示插入一次作者注释。

### 默认作者注释

面板底部的文本框包含默认作者注释，该注释将应用于每个新聊天。

## 常见使用场景

### 提醒 AI 响应格式

作者注释可用于指定 AI 应如何撰写其回复。

- [Your next response must be 300 tokens in length.]
- [Write your next reply in the style of Edgar Allan Poe]
- [Use markdown italics to signify unspoken actions, and quotation marks to specify spoken word.]

### 强化指令

- [Remember the instructions you were given at the beginning of this chat.]

### 作为临时世界信息、角色偏向，或用于非指令模式模型的指令

- [\{\{char\}\} is in the library]
- [\{\{user\}\} has a fresh wound to his leg, so won't be able to run away.]
- [\{\{char\}\} cannot speak and must communicate using hand signals.]
