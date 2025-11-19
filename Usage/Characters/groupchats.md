---
order: 70
route: /usage/core-concepts/groupchats/
---

# Group Chats

## 回复顺序策略

决定如何为 group chats 中的 characters 起草回复。

### Manual

您可以手动从菜单中选择要回复的 character，或使用 `/trigger` 命令。选定的 group 成员将是唯一回复的成员。User 消息不会自动触发任何回复。使用空 user 输入触发生成将触发随机未静音的 group 成员回复。

### Natural Order

尝试模拟真实人类对话的流程。算法如下：

1. 从聊天中的最后一条消息中提取 group 成员名称的提及。

    只有完整的单词才会被识别为提及！如果您的 character 名字是"Misaka Mikoto"，他们只会对"Misaka"或"Mikoto"做出回应，但永远不会对"Misa"、"Railgun"等做出回应。

    除非启用了"Allow Self Responses"设置，否则 characters 不会回复其自己消息中对其名字的提及！

2. Characters 由"Talkativeness"因子激活。

    Talkativeness 定义了如果 character 未被提及，他们说话的频率。在 character 编辑器的"Advanced Definitions"屏幕上调整此值。滑块值从 **0% / Shy**（character 除非被提及，否则永远不会说话）到 **100% / Chatty**（character 总是回复）呈线性刻度。新 characters 的默认值为 50% 概率。

3. 选择随机 character。

    如果在前面的步骤中没有激活任何 characters，则随机选择一个发言者，忽略所有其他条件。

### List Order

Characters 根据他们在 group 成员列表中的顺序被起草。不适用其他规则。

### Pooled Order

激活自上一条 user 消息以来尚未发言的一个随机 character。如果所有 characters 都已发言，则随机选择一个，直到下一条 user 消息。

## Group 生成处理模式

此设置决定如何处理 group chat 成员的 character 信息。无论选择什么，group chat 历史始终在所有成员之间共享。

### Swap character cards

默认模式。每次生成消息时，只有活动发言者的 character card 信息被包含在 context 中。

### Join character cards

所有 group 成员的信息按其列表顺序组合成一个联合 prompt。这可以在改变大块 context 不可取的情况下有所帮助，例如使用 llama.cpp prompt 缓存时。

此模式有两个子模式（您必须选择一个）：

* Include muted - 静音的 characters 将始终包含在联合 prompt 中。
* Exclude muted - 如果静音的 characters 不是当前发言者，则不会被包含。

以下字段正在组合：

1. Description
2. Scenario，如果未为聊天覆盖
3. Personality
4. Message examples
5. Character notes / Depth prompts

**重要！** 请注意，由于典型 character card 的结构方式，使用此模式可能导致意外行为，包括但不限于：characters 对自己感到困惑，具有合并的性格，不确定的特征等。

### Join Prefix 和 Suffix

当选择'Join character cards'时，characters 的所有相应字段都会连接在一起。这意味着在生成的 prompt 中，所有 character descriptions 将连接成一大块文本。如果您希望这些字段被分隔，可以定义 prefix 和/或 suffix。

这些选项支持普通 macros，还会将 \{\{char\}\} 替换为相关 character 的名称，并将 \<FIELDNAME\> 替换为部分的名称（例如：description、personality、scenario 等）。

## 其他 Group Chat 菜单选项

### Mute Character

Group chat 菜单中 character 头像旁边的划掉的语音气泡图标可以禁用或启用特定 character 在聊天中的回复。

### Force Talk

Group chat 菜单中 character 头像旁边的语音气泡图标将仅触发特定 character 的回复，绕过回复顺序策略。即使 group 成员被静音，它也会工作。

### Auto-mode

启用 auto-mode 时，group chat 将遵循回复顺序并在没有 user 交互的情况下触发消息生成。当最后起草的 character 发送其消息时，下一个 auto-mode 轮次将在 5 秒延迟后触发。当 user 开始在发送消息文本区域中键入时，auto-mode 将被禁用，但已排队的生成不会自动停止。

### Allow Self Responses

当选择 Natural Order 时，如果 character 恰好由于在自我提及时被触发，将允许发送最新消息的 character 连续回复。对 List order 无效。

### Group Chat Scenario Override

所有 group 成员将使用输入的 scenario 文本，而不是其 character cards 中指定的内容。分支聊天从其父级继承 scenario 覆盖，之后可以单独更改。

### Peek Character Definitions

点击 group chat 菜单中头像旁边的 character card 图标将快速导航到常规 character 定义屏幕。此处所做的任何更改都将保存到 card 本身。

要返回 group chat，请点击 Group Name 标题链接。

### Member Management

您的任何现有 characters 都可以在 group chat 中添加、删除、静音或重新排序。默认情况下，新成员被添加到 group 成员列表的顶部，然后可以使用箭头图标重新排序。

### Group Chat pop-out

可以通过点击"Current Members"字段旁边的图标来激活 group chat 菜单弹出窗口。这会创建 group chat 菜单的弹出窗口。通过从 user settings 启用 MovingUI，此菜单可以调整大小并拖动到界面内的任何位置，并且功能与常规 group chat 菜单一样。
