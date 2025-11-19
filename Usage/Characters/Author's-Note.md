---
order: 50
route: /usage/core-concepts/authors-note/
---

# Author's Note

## 它是什么？

Author's Note 是一个强大的工具，用于自定义 AI 响应，它可以将一段文本插入到 prompt 的任何位置，并以您期望的任何频率插入。

## 使用方法

Author's Note 可以在聊天输入栏左侧的 Options 菜单中找到。

| Options 菜单                           | Author's Note 面板                      |
|---------------------------------------|----------------------------------------|
| ![](/static/extensions/note-menu.png) | ![](/static/extensions/note-panel.png) |

## 配置 Author's Notes

### 特定聊天的 Author's Note

Author's Note 面板顶部的文本框包含当前聊天的 Author's Note。

**此文本框的内容不会自动转移到任何新聊天中。**

### 放置选项

#### After Scenario

这会将 Author's Note 放置在 context 顶部附近，位于 Character Definition 的 'Scenario' 部分之后。如果未指定 scenario，它将放置在 Character Definition 的最后部分之后，Example messages 之前。

#### In-chat

这会将 Author's Note 放置到指定深度的聊天历史中。

Depth 0 = 放置在聊天历史的最末端。

Depth 4 = 放置在最近 3 条聊天历史消息之前，使其成为聊天历史中的第 4 个实体。

_Author's Note 越接近 prompt 底部，对下一次 AI 响应的影响就越大。_

### Insertion Frequency

这是您希望 Author's Note 包含在聊天中的频率。

Frequency 0 = Author's Note 永远不会被插入。

Frequency 1 = Author's Note 将在每次用户输入 prompt 时插入。

Frequency 4 = Author's Note 将在每第 4 次用户输入 prompt 时插入。

### Default Author's Note

面板底部的文本框包含 Default Author's Note，它将应用于每个新聊天。

## 常见用例

### 提醒 AI 响应格式

Author's Note 可用于指定 AI 应如何编写响应。

- [Your next response must be 300 tokens in length.]
- [Write your next reply in the style of Edgar Allan Poe]
- [Use markdown italics to signify unspoken actions, and quotation marks to specify spoken word.]

### 强化指令

- [Remember the instructions you were given at the beginning of this chat.]

### 作为临时 World Info、Character Bias 或非 Instruct 模型的 Instruct

- [\{\{char\}\} is in the library]
- [\{\{user\}\} has a fresh wound to his leg, so won't be able to run away.]
- [\{\{char\}\} cannot speak and must communicate using hand signals.]