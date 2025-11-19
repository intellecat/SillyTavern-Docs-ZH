---
order: 130
icon: globe
route: /usage/core-concepts/worldinfo/
templating: false
---

# 世界信息

**World Info(也称为 Lorebooks 或 Memory Books)是 ST 中的一个强大工具,可以动态地将 prompts 插入到你的聊天中,以帮助引导 AI 回复。**

通常,World Info(简称 WI)用于增强 AI 对你的虚拟世界细节的理解,但你也可以使用 World Info 条目来插入任何你想要插入到 prompt 中的内容。

它的功能类似于一个动态字典,只有当与条目相关的关键词出现在消息文本中时,才会插入来自 World Info 条目的相关信息。

SillyTavern 引擎会激活并无缝地将适当的 lore 整合到 prompt 中,为 AI 提供背景信息。

*需要注意的是,虽然 World Info 有助于引导 AI 生成所需的内容,但它不能保证这些内容一定会出现在生成的输出消息中。这取决于你的模型在利用额外信息方面的能力!*

## 专业提示

* World Info 引擎是一个非常强大的 prompt 管理工具。不要仅仅局限于添加角色 lore,可以自由尝试。
* 激活关键词、标题和其他不在 **Content** 字段中的信息不会插入到 context 中,因此每个 World Info 条目都应该有一个全面、独立的描述。
* 要创建丰富详细的世界 lore,条目可以通过使用递归激活相互链接和引用。更多信息请参见下面的 [Recursion](#recursive-scanning)。
* SillyTavern 为插入的背景信息提供灵活的 context 预算。为了节省 prompt tokens,建议保持条目内容简洁。

## 延伸阅读

* [World Info Encyclopedia](https://rentry.co/world-info-encyclopedia): 由 kingbri、Alicat、Trappu 编写的 World Info 和 Lorebooks 深入指南。

## 角色设定

可选地,World Info 文件可以分配给角色,作为该角色在所有聊天(包括群组)中的专用 lore 来源。

一个主要的 World Info 可以绑定到角色。要做到这一点,导航到 Character Management 面板并点击地球按钮,然后从下拉列表中选择 World Info 并点击"Ok"。导出角色时,此文件也会嵌入到角色卡片数据中。

要解除绑定、更改或分配额外的 World Info 文件作为角色 lore,shift-点击地球按钮或点击"More..."然后"Link World Info"。请注意,只有主要的 World Info 文件会随角色导出。

### 角色设定插入策略

在生成 AI 回复时,来自角色 World Info 的条目将与来自全局 World Info 选择器的条目使用以下策略之一进行组合:

#### 均匀排序（默认）

所有条目将根据它们的 Insertion Order 进行排序,就像它们是一个大文件的一部分一样,忽略来源。

#### 角色设定优先

来自 Character World Info 的条目将根据它们的 Insertion Order 首先包含,然后是来自 Global World Info 的条目。

#### 全局设定优先

来自 Global World Info 的条目将根据它们的 Insertion Order 首先包含,然后是来自 Character World Info 的条目。

### World Info 条目

#### 关键词

触发 World Info 条目激活的关键词列表。默认情况下,Keys 不区分大小写(这是[可配置的](#case-sensitive-keys))。

##### 正则表达式（Regex）作为关键词

Keys 通过支持 regex 提供了更灵活的匹配方法。这使得可以匹配更多动态内容,包括可选的词或字符、间距以及 regex 提供的所有其他实用工具。
如果定义的 key 是有效的 regex(Javascript regex 风格,使用 `/` 作为分隔符。允许所有标志),在检查条目是否应该被触发时,它将被视为 regex。多个 regexes 可以作为单独的 keys 输入,并且可以相互配合工作。在 regex 内部,可以使用逗号。Plaintext keys 不支持逗号,因为它们被视为 key 分隔符。

高级 regex 匹配的一个用例示例:
当角色执行与天气相关的动作时应该插入的条目/指令

```js
/(?:{{char}}|he|she) (?:is talking about|is noticing|is checking whether|observes) (?:the )?(rainy weather|heavy wind|it is going to rain|cloudy sky)/i
```

有关 Regex 语法和可能性的更多信息: [Regular expressions - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_expressions)

###### 高级 Regex 每条消息匹配

ST 在 WI 扫描缓冲区中的每条聊天消息前加上 `character name:`,在 v1.12.6 之后,使用字符值 1 (`\x01`) 进行连接。
这意味着你可以使用与该分隔字符相关的 regex 来匹配特定角色的输入或输出。

例如,要仅匹配用户说"hello",你可以使用以下 regex:

```js
/\x01{{user}}:[^\x01]*?hello/
```

##### 关键词输入

有两种输入关键词的模式,每种模式的 UI 略有不同。在 ⌨️ *plaintext mode*(默认)中,keys 可以在单个文本字段中以逗号分隔的列表形式输入。也可以包含 Regexes,但它们没有任何特殊高亮显示。在 ✨ *fancy mode* 中,keys 显示为单独的元素,regexes 将被高亮显示。该控件支持编辑和删除 keys。可以通过输入控件内的内联按钮切换模式。

#### 可选过滤器

与主 key 一起使用的补充关键词的逗号分隔列表。
如果未提供参数,则忽略此标志。
支持 AND ANY、NOT ANY 或 NOT ALL 的逻辑

1. AND ANY = 仅在主 key 和任何一个可选过滤器 keys 都在扫描的 context 中时激活条目。
2. AND ALL = 仅在主 key 和所有可选过滤器 keys 都存在时激活条目。
3. NOT ANY = 仅在主 key 在扫描的 context 中且没有任何可选过滤器 keys 时激活条目。
4. NOT ALL = 即使主 key 触发,如果所有可选过滤器都在扫描的 context 中,也会阻止条目激活。

这些 keys 也支持 [regex](#regular-expression-regex-as-keys)。

#### 条目内容

条目激活时插入到 prompt 中的文本。

#### 插入顺序

数值。定义当多个条目同时激活时的优先级。具有更大 order 数字的条目将被插入到 context 末尾附近,因为它们对输出的影响更大。例如,Order 数字为 100 的条目将出现在 Order 数字为 250 的条目之前。

#### 插入位置

* **Before Char Defs:** World Info 条目插入在角色描述和场景之前。对对话有中等影响。
* **After Char Defs:** World Info 条目插入在角色描述和场景之后。对对话有更大影响。
* **Before Example Messages:** World Info 条目被解析为示例对话块,并插入到角色卡片提供的示例之前。
* **After Example Messages:** World Info 条目被解析为示例对话块,并插入到角色卡片提供的示例之后。
* **Top of AN:** World Info 条目插入在 Author's Note 内容的顶部。影响程度取决于 Author's Note 的位置。
* **Bottom of AN:** World Info 条目插入在 Author's Note 内容的底部。影响程度取决于 Author's Note 的位置。
* **@ D:** World Info 条目插入到聊天中的特定深度(Depth 0 为 prompt 底部)。
  * ⚙️ - 作为 system role 消息
  * 👤 - 作为 user role 消息
  * 🤖 - 作为 assistant role 消息
* **Outlet:** World Info 条目不会自动注入。相反,其内容存储在一个命名的 outlet 下,因此你可以通过使用 [`{{outlet::Name}}` macro](#outlet-name) 调用它来准确决定它在 prompt 中的位置。

Example Message 条目将根据 prompt 构建设置进行格式化:Instruct Mode 或 Chat Completion prompt manager。它们也遵循 Example Messages Behavior 规则:在完整 context 时逐渐被推出、始终保留或完全禁用。

如果你的 Author's Note 被禁用(Insertion Frequency = 0),位于 A/N 位置的 World Info 条目将被忽略!

#### 出口名称

当选择 **Outlet** 插入位置时,条目会显示一个额外的 **Outlet Name** 字段。你在这里提供的名称将条目分组在一起,并定义你将用来手动将它们拉入 prompt 的 token。

在 [Prompt Manager](./Prompts/prompt-manager.md) 或 [Advanced Formatting](./Prompts/advancedformatting.md) prompt 字段中使用 `{{outlet::YourName}}` macro。当构建 prompt 时,macro 会被替换为共享相同 outlet name 的每个 World Info 条目的组合内容,用换行符分隔,按它们的 [Insertion Order](#insertion-order) 值排序。

如果 outlet 条目缺少名称,它将在生成期间被跳过,因此请确保填写该字段。Outlet names 支持基于你已使用的名称的自动完成,以便轻松重用一致的标签。

##### 限制和注意事项

* 不支持在 World Info 条目内放置 outlet macros,也不会工作。这与 World Info 的评估顺序冲突,可能导致无限循环。
* 不支持嵌套 outlets。你不能在另一个 outlet 的内容中放置 outlet macro。与上述相同,这可能导致无限循环。
* Character card 字段(Description、Personality、Scenario 等)不能扩展 outlets。这些字段被提前解析,因此它们可以充当 World Info 触发器的[额外匹配源](#additional-matching-sources),这意味着在处理它们的文本时 outlets 不可用。如果需要将 outlet 内容放在 prompt 主体中,请使用另一个支持 macro 的字段。
* Author's Note 编辑器也不能解析 outlets。要将 outlet 内容放在 Author's Note 周围,请将条目分配到 **Top of AN** 或 **Bottom of AN** 插入位置,而不是依赖 macro。
* Outlet names 区分大小写。`{{outlet::}}` macro 必须使用与条目的 **Outlet Name** 完全相同的大小写,否则不会返回内容。
* Outlet name 中的前导或尾随空格在调用 macro 时会被忽略,因此保存了额外空格的名称将不匹配。避免填充名称以便正确解析。
* 没有分配内容的 Outlet macros 将被替换为空字符串。

#### 条目标题/备注

一个方便你标记条目的文本字段,不会被 AI 或任何触发逻辑使用。

如果为空,可以通过点击"Fill empty memos"按钮使用条目的第一个 key 进行回填。

#### 策略

1. 🔵 (Blue Circle) = 条目将始终存在于 prompt 中。
2. 🟢 (Green Circle) = 条目仅在存在关键词时触发。
3. 🔗 (Chain Link) = 条目允许通过 embedding 相似性插入。

每个 Entry 还有一个切换开关,允许你启用或禁用该条目。

#### 触发概率 (%)

此值充当额外的过滤器,为条目在通过任何方式激活时(constant、primary key、recursion)添加不被插入的机会。

1. Probability = 100 意味着条目将在每次激活时插入。
2. Probability = 50 意味着条目将以 1:1 的机会插入。
3. Probability = 0 意味着条目不会插入(本质上禁用它)。

使用此功能在你的聊天中创建随机事件。例如,如果在消息中提到上古神的名字,每条消息都有 1% 的机会唤醒它。

#### 包含组

Inclusion groups 控制当多个具有相同组标签的条目同时触发时如何选择条目。如果激活了多个具有相同组标签的条目,只有一个会被插入到 prompt 中。

默认情况下,根据它们的 Group Weight(默认为 100 点)随机选择选定的条目——数字越高,被选择的概率越高。这允许在触发的条目之间进行随机选择,为互动添加惊喜和变化元素。

如果将多个组定义为逗号分隔的列表,单个条目可以是多个 inclusion groups 的一部分。上述相同的逻辑将适用。如果该条目被触发,它将*禁用*属于其任何组的所有其他条目。因此,如果激活了任何组,该条目将不会被激活。

#### 优先包含

为了对通过 [包含组](/Usage/worldinfo.md#inclusion-group) 激活的条目提供更多控制,你可以使用'优先包含'设置。此选项允许你确定性地指定要选择的条目,而不是随机掷 Group Weight 机会。

如果激活了多个具有相同组标签且启用此设置的条目,将选择具有最高'Order'值的条目。这对于通过 inclusion groups 创建回退序列很有用。例如,优先考虑低深度条目以获得更多强调,或者在两者都有效时选择特定的场景设置指令而不是另一个。

#### 使用组评分

当此设置全局启用或按条目启用时,激活的条目 keys 数量决定组胜者选择。只有具有最高 key 匹配数的组子集将被留下以通过 Group Weight 或 Inclusion Priority 激活——其余的将被停用并从组中移除。

使用此功能为大组中的单个条目提供更多特异性。例如,它们可以有一个共同的 key 和一个特定的 key。当没有提供特定 key 时,将插入随机条目,反之亦然。

主 keys 的分数计算逻辑是 1 个匹配 = 1 分。

对于次要 keys,交互取决于所选的 Selective Logic:

1. AND ANY: 1 个次要匹配 = 1 分。
2. AND ALL: 如果所有次要 keys 都匹配,则每个次要 key 得 1 分。
3. NOT ANY 和 NOT ALL: 无变化。

示例:

* Entry 1. Keys: song, sing, Black Cat. Group: songs
* Entry 2. Keys: song, sing, Ghosts. Group: songs

输入 `sing me a song` 可以激活任一条目(都激活了 2 个 keys),但 `sing me a song about Ghosts` 将只激活 Entry 2(激活了 3 个 keys)。

#### 自动化 ID

允许将 World Info 条目与 Quick Replies extension 中的 [STscripts](/For_Contributors/st-script.md) 集成。如果 quick reply 命令和 WI 条目具有相同的 自动化 ID,当具有匹配 ID 的条目被激活时,该命令将自动执行。

Automations 按照它们被触发的顺序执行,遵循你指定的排序策略,将 [Character Lore Insertion Strategy](#character-lore-insertion-strategy) 与'Priority'排序相结合。这导致 [Blue Circle](#strategy) 条目首先处理,然后是其他按照指定的'Order'处理。递归触发的条目将按照相同的顺序在之后处理。

如果激活了多个具有相同 自动化 ID 的条目,脚本命令将只运行一次。

#### 角色过滤器

此条目可以激活的角色名称列表。如果此列表不为空,则条目仅对名称在列表中的角色激活。当选择标签时,条目仅对具有该特定标签的角色激活。

"Exclude"模式反转过滤器,这意味着条目将对除了添加到列表或具有所选标签的角色之外的所有角色激活。

#### 触发器

此 World Info 条目可以激活的生成类型。如果未选择任何内容,条目可以对所有生成类型激活。如果选择了一个或多个,条目将仅对这些特定生成类型激活:

* **Normal:** 常规消息生成请求。
* **Continue:** 按下 Continue 按钮时。
* **Impersonate:** 按下 Impersonate 按钮时。
* **Swipe:** 通过滑动触发生成时。
* **Regenerate:** 在单人聊天中按下 Regenerate 按钮时。
* **Quiet:** 后台生成请求,通常由 [extensions](/extensions/index.md) 或 [STscript](/For_Contributors/st-script.md) 命令触发。

!!!
"Regenerate"触发器在群聊中不可用,因为它使用不同的重新生成逻辑:删除最后一次回复的所有消息,并根据所选的 [Group reply strategy](/Usage/Characters/groupchats.md#reply-order-strategies) 使用"Normal"生成类型排队消息。
!!!

#### 额外匹配源

默认情况下,World Info Entries 仅与当前对话的内容匹配。这些选项允许你将条目与聊天中未显示的不同角色信息,甚至 persona 信息进行匹配。当你想要在多个角色之间使用广泛的条目但不想管理大型标签列表,或者每次创建新角色时都不想更新角色过滤器列表时,这很有用。这还允许你基于你激活的 persona 匹配条目。

* **Character Description**: 与角色描述匹配。
* **Character Personality**: 与角色个性摘要匹配,可在 Advanced Definitions 下找到。
* **Scenario**: 与角色指定的场景匹配,可在 Advanced Definitions 下找到。
* **Persona Description**: 与当前选择的 persona 描述匹配。
* **Character's Note**: 与角色的笔记匹配,可在 Advanced Definitions 下找到。
* **Creator's Notes**: 与角色创建者的笔记匹配,可在 Advanced Definitions 下找到。创建者的笔记通常不包含在 prompt 中。

## 向量存储匹配

Vector Storage 扩展通过使用最近聊天消息和 World Info 条目内容之间的相似性,提供了关键词匹配的替代方案。

要启用和使用此功能,需要满足以下先决条件:

1. Vector Storage 扩展已启用,并配置为使用可用的 embedding 源之一。
2. Vector Storage 扩展设置中的"Enable for World Info"复选框已勾选。
3. 要么允许 keyless 匹配的 World Info 条目具有"Vectorized"(🔗)状态,要么在 Vector Storage 设置中勾选了"Enabled for all entries"选项。

这里不会涉及扩展中的 vectorization 模型选择和"embeddings"这个术语背后的理论含义。如果你需要更多关于这个主题的信息,请查看 [Data Bank](/Usage/Characters/data-bank.md#vector-storage) 指南。

Vector Storage 匹配遵循以下规则:

* 可以通过"Max Entries"设置调整允许通过 Vector Storage 匹配的最大条目数。这个数字只设置限制,不影响 World Info 激活设置中设置的 token 预算。所有预算规则仍然适用。
* 此功能仅替换关键词检查。所有额外检查都必须满足才能插入条目:trigger%、character filters、inclusion groups 等。
* 不使用 Activation Settings 或条目覆盖中的"扫描深度"设置。而是使用 Vector Storage 的"Query messages"值来获取要匹配的文本。这允许像"扫描深度"设置为 0 这样的配置,因此不会进行常规关键词匹配,但条目仍然可以通过 vectors 激活。
* "Vectorized"状态只是一个额外的标记。如果设置了 keywords,条目仍然会像正常的、启用的、非 constant 记录一样,通过 keywords 激活。如果你希望它们只通过 vectors 激活,请删除 keywords。

!!!info 注意
由于检索质量完全取决于 embedding 模型的输出,无法准确预测会插入哪些条目。如果你想要确定性和可预测的结果,请坚持使用 keyword 匹配。
!!!

## 定时效果

通常,World Info 评估是无状态的,这意味着评估的结果是相同的,只取决于当前的聊天 context。但是,通过引入 Timed Effects,你可以创建具有激活延迟、在触发后保持活动或在激活后无法触发的条目。

### 定时效果规则

1. 效果的时间框架以消息为单位测量(不是消息对/交换),0 表示没有效果。
2. 效果仅适用于激活条目的聊天。Branches 继承父聊天的状态。
3. 如果聊天没有前进,例如如果最后一条消息被滑动或删除,则会删除活动的 timed effects。
4. 对当前处于 timed effect 的条目进行任何更改都会导致效果被强制删除。
5. 如果效果已经处于活动状态,后续触发 keywords 不会刷新效果持续时间。

### 定时效果类型

1. Sticky - 条目在激活后保持活动 N 条消息。Stickied 条目在到期之前的后续扫描中忽略概率检查。
2. Cooldown - 条目在激活后的 N 条消息内无法激活。可以与 sticky 一起使用:当 sticky 持续时间结束时,条目进入 cooldown。
3. Delay - 除非在评估时聊天中至少有 N 条消息,否则条目无法激活。
    * Delay = 0 -> 条目可以在任何时候激活。
    * Delay = 1 -> 条目不能在聊天为空时激活(无问候语)。
    * Delay = 2 -> 条目不能在聊天中有零条或只有一条消息时激活,以此类推。

### 定时效果示例

条目配置: sticky = 3, cooldown = 2, delay = 2。

```txt
Message 0: delay
Message 1: entry activated
Message 2: sticky
Message 3: sticky
Message 4: sticky
Message 5: cooldown
Message 6: cooldown
Message 7: entry can be activated again
```

## 激活设置

位于 World Info 屏幕顶部的可折叠菜单。

### 扫描深度

> 可以在条目级别上覆盖。

定义应该扫描聊天历史中的多少条消息以查找 World Info keys。

* 如果设置为 0,则只评估递归条目和 Author's Note。
* 如果设置为 1,则 SillyTavern 只扫描最后一条消息。
* 2 = 最后两条消息,以此类推。

### 包含名称

定义聊天参与者的名称是否应该作为消息前缀包含在扫描的文本缓冲区中。这允许激活使用名称作为 keywords 的条目,而无需在消息中直接提到名称。

假设聊天参与者名为 Alice 和 Bob,请参见下面要扫描的文本示例。

启用(默认):

```txt
Alice: Hello! Good to see you.
Bob: How is the weather today?
```

禁用:

```txt
Hello! Good to see you.
How is the weather today?
```

### 上下文 % / 预算

**定义 World Info 条目一次可以使用多少 tokens。**
你可以相对于 API 的 max-context 设置定义阈值(Context %)或定义一个客观的 token 阈值(Budget)。

如果预算用尽,即使 prompt 中存在 keys,也不会激活更多条目。

Constant 条目将首先插入。然后是具有更大 order 数字的条目。

通过直接提到 keys 插入的条目比那些在其他条目内容中提到的条目具有更高的优先级。

### 最小激活数

**此设置与 最大递归步骤 互斥。**

Minimum Activations: 如果设置为非零值,这将忽略"scan-depth"的限制,从最新消息向后搜索整个聊天记录中的 keywords,直到触发了指定数量的条目。这仍然会受到 Max Depth 设置或总 Budget 上限的限制。

*由 最小激活数 触发的额外扫描不会检查在前面步骤中由 recursion 添加的条目。只有聊天消息和扩展 prompts 可以触发这些额外激活。但是,由 最小激活数 激活的条目可以像往常一样触发其他条目。*

### 最大深度

使用 最小激活数 设置时要扫描的最大深度。

### 递归扫描

递归扫描 允许条目激活其他条目或被其他条目激活,从而实现不同 World Info 条目之间的复杂交互和依赖关系。此功能可以显著增强你创意场景的动态性。
是否启用 recursive scanning 可以通过全局设置 **Recursive Scan** 来控制。
每个条目有三个选项可以控制 recursion:

* **Non-recursable**: 选中此复选框时,该条目不会被其他条目激活。这对于不应改变或不受其他 world info 条目影响的静态信息很有用。

* **Prevent further recursion**: 选择此选项可确保一旦此条目被激活,它将不会触发任何其他条目。这有助于避免意外的激活链。

* **Delay until recursion**: 此条目将仅在 recursive 检查期间激活,这意味着它不会在初始传递中触发,但可以被启用了 recursion 的其他条目激活。现在,通过为这些延迟添加的 **Recursion Level**,条目按级别分组。最初,只有第一级(最小数字)会匹配。一旦找不到匹配项,下一级就有资格匹配,重复此过程直到检查完所有级别。这允许更好地控制在 recursion 过程中如何以及何时揭示更深层的信息,特别是在与 NOT ANY 或 NOT ALL key 匹配组合等条件结合使用时。

**条目可以通过在内容文本中提到其他条目的 keywords 来激活它们。**

例如,如果你的 World Info 包含两个条目:

```txt
Entry #1
Keyword: Bessie
Content: Bessie is a cow and is friends with Rufus.
```

```txt
Entry #2
Keyword: Rufus
Content: Rufus is a dog.
```

如果消息文本仅提到 **Bessie**,**两个**条目都会被拉入 context 中。

### 最大递归步骤

**此设置与 最小激活数 互斥。**

当设置为零时,recursion 嵌套仅受你的 prompt 预算限制。当设置为非零值时,将总扫描次数限制为所需的最大"嵌套级别"。

示例值:

* 1 实际上禁用了 recursion,因为检查在第一步后停止。
* 2 只能激活一次 recursive 条目。
* 3 可以触发两次 recursion...

### 区分大小写关键词

> 可以在条目级别上覆盖。

**要被拉入 context 中,entry keys 需要与它们在 World Info 条目中定义的大小写匹配。**

当你的 keys 是常见词或常见词的一部分时,这很有用。

例如,当此设置激活时,keys 'rose' 和 'Rose' 将根据输入被区别对待。

### 匹配整词

> 可以在条目级别上覆盖。

只包含一个词的条目 keys 只有在搜索文本中出现完整词时才会匹配。默认启用。

例如,如果启用此设置且条目 key 是 "king",则文本 "long live the king" 会匹配,但 "it's not to my liking" 不会。

**重要:** 此设置在用于不使用空格分隔词的语言(如日语或中文)时可能会产生不利影响。如果你用这些语言编写条目,建议保持关闭。

### 溢出警告

如果激活的 World Info 超过分配的 token 预算,则显示警告。
