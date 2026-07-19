# 宏

!!!tip 实验性宏引擎
实验性宏引擎支持嵌套、稳定的替换顺序以及其他改进。新安装的 SillyTavern 默认启用此功能。已有安装可在**用户设置** > **聊天/消息处理** > **实验性宏引擎**中启用。
!!!

宏是动态占位符，在处理文本时会被替换为实际值。它们广泛用于 SillyTavern 的提示词、角色卡、世界信息、快速回复等功能中。

## 查找可用宏

SillyTavern 为所有可用宏提供了内置文档：

- **斜杠命令**：在聊天输入框中输入 `/? macros`，可显示所有已注册宏及其描述的列表。
- **自动补全**：详情请参阅下方的[宏自动补全](#宏自动补全)。

### 宏自动补全

宏自动补全会在你输入时提供可用宏的建议。它适用于 SillyTavern 中所有支持宏的文本字段。

输入 `{{` 即可开始宏的自动补全，显示可用宏及其参数、可能的[宏标志](#宏标志)、[变量简写](#变量简写)等内容。

**默认情况下自动补全出现的位置：**

- 聊天用户输入框
- 扩展编辑器（全屏文本编辑，通过文本字段旁的"展开"按钮打开）
- 提示词管理器编辑器

**在其他字段中触发自动补全：**

- 在任何支持宏的文本字段中按 **Ctrl+Space** 打开自动补全弹出窗口
- 启用**设置 → 自动补全设置 → 在所有宏字段中显示**，使自动补全在所有宏字段中自动出现

## 基本语法

宏用双花括号括起来：

```txt
{{macroName}}
```

宏名称**不区分大小写**。`{{User}}`、`{{USER}}` 和 `{{user}}` 均解析为同一个宏。

示例：

```txt
{{user}}        // 返回当前用户/人设名称
{{char}}        // 返回当前角色名称
{{time}}        // 返回当前时间
{{date}}        // 返回当前日期
```

## 参数

许多宏接受参数以自定义其行为。

### 空格分隔符

对于只有一个参数的宏，可以用空格将宏名与其参数分开：

```txt
{{macroName argument}}
```

示例：

```txt
{{getvar myVariable}}
{{roll 1d20}}
{{reverse Hello World}}
```

### 双冒号分隔符

使用 `::` 分隔多个参数：

```txt
{{macroName::arg1::arg2::arg3}}
```

示例：

```txt
{{setvar::myVariable::Hello World}}
{{random::red::green::blue}}
{{roll::2d6+3}}
```

空格和 `::` 都是推荐的宏参数语法。

### 单冒号分隔符（旧版）

单个 `:` 也可以引入参数，但此语法被视为旧版语法，不推荐用于新内容：

```txt
{{macroName:argument}}
```

示例：

```txt
{{roll:1d20}}
```

## 宏定义中的空白字符

宏名称、分隔符和参数之间的空白字符会被忽略。这使得格式更易读：

```txt
{{ macroName :: arg1 :: arg2 }}
{{ setvar :: myVariable :: some value }}
{{ if :: condition }}
```

以上所有写法均等同于不含额外空格的紧凑形式。

## 嵌套宏

宏可以嵌套在其他宏内部。内层宏先被解析：

```txt
{{getvar::{{char}}_mood}}
```

这会先解析 `{{char}}`（例如解析为"Alice"），然后解析 `{{getvar::Alice_mood}}`。

更多示例：

```txt
{{setvar::greeting::Hello, {{user}}!}}
```

设置一个包含用户名称的变量内容。

```txt
{{if {{getvar::showDetails}}}}Details here{{/if}}
```

条件本身是一个检索变量值的宏。

## 作用域宏

任何接受至少一个参数的宏都支持作用域语法。开闭标签之间的内容将成为该宏的**最后一个参数**。

### 作用域语法

可以将最后一个参数放置在开闭标签之间，而不是内联写出：

```txt
{{macroName argument}}
  scoped content here
{{/macroName}}
```

闭合标签在宏名前使用 `/` 标志：`{{/macroName}}`。

这等同于：

```txt
{{macroName::argument::scoped content here}}
```

### 示例

设置包含多行内容的变量：

```txt
{{ setvar backstory }}
  This character was born in a small village
  and grew up to become a renowned scholar.
{{ /setvar }}
```

对作用域内容使用 `reverse`：

```txt
{{ reverse }}
  Hello World
{{ /reverse }}
```

返回结果为"dlroW olleH"。

### 内容修剪

默认情况下，作用域内容会自动修剪：

- 移除首尾空白字符
- 统一的缩进会被去除（第一个非空行的缩进从所有行中移除）

这允许干净的格式：

```txt
{{ if condition }}
    # Heading
    Some content here
{{ /if }}
```

结果为 `# Heading\nSome content here`（不含前导空格）。

若要保留所有空白字符（包括首尾换行符），请使用 `#` 标志。详情请参阅[宏标志](#宏标志)。

## 条件宏

`{{if}}` 宏根据值是 truthy 还是 falsy 来有条件地渲染内容。

### 简单条件

```txt
{{ if description }}
  # Character Description
  {{ description }}
{{ /if }}
```

仅当 `description` 返回非空值时才显示标题和描述。

条件可以是：

- 宏名称（如不需要参数则自动解析）
- 来自嵌套宏的任何值，如 `{{getvar::flag}}`
- 变量简写，如 `.myFlag` 或 `$globalFlag`（参见[变量简写](#变量简写)）
- 任何文本（将根据其内容隐式解析为 truthy 或 falsy）

Falsy 值：空字符串、`false`、`0`、`off`、`no`。

### 在条件中使用变量简写

变量简写提供了一种在条件中检查变量值的简洁方式：

```txt
{{ if .isEnabled }}
  Feature is enabled.
{{ /if }}

{{ if !$globalDisabled }}
  Not globally disabled.
{{ /if }}
```

有关简写符号的更多详情，请参阅[变量简写](#变量简写)。

### 反转条件

在条件前加 `!` 以反转它：

```txt
{{ if !personality }}
  No personality defined for this character.
{{ /if }}
```

### If/Else 分支

在 `{{if}}` 块内使用 `{{else}}` 定义替代分支：

```txt
{{ if personality }}
  {{ personality }}
{{ else }}
  No personality defined.
{{ /if }}
```

另一个示例：

```txt
{{ if {{getvar::details-block}} }}
  # Details Block
  {{ getvar::details-block }}
{{ else }}
  No details currently exist.
{{ /if }}
```

## 宏标志

标志是放置在开花括号与宏名之间的特殊符号字符，用于修改宏的行为。

### 语法

```txt
{{!macroName}}
{{#macroName}}
```

标志可以组合使用：

```txt
{{!?macroName}}
```

标志与宏名之间允许有空白字符：

```txt
{{ / macroName }}
{{ # macroName }}
```

### 已实现的标志

| 标志 | 名称 | 描述 |
|------|------|-------------|
| `/` | 闭合块 | 标记作用域宏的闭合标签。示例：`{{/if}}` |
| `#` | 保留空白字符 | 阻止自动修剪作用域内容。 |

### 计划中的标志（尚未实现）

| 标志 | 名称 | 描述 |
|------|------|-------------|
| `!` | 立即执行 | 在同一文本中的其他宏之前解析此宏。 |
| `?` | 延迟执行 | 在同一文本中的其他宏之后解析此宏。 |
| `~` | 重新求值 | 将此宏标记为需要重新求值。 |
| `>` | 过滤器 | 为此宏启用基于管道的输出过滤器。 |

### 类标志的前缀运算符

变量简写语法使用前缀运算符（`.` 和 `$`），其行为类似于标志，但本身并非标志。
详情请参阅[变量简写](#变量简写)部分。

### 保留空白字符标志

当你需要保留作用域内容中的所有空白字符（包括首尾换行符和缩进）时，使用 `#` 标志：

```txt
{{ # setvar code }}
    function hello() {
        return "world";
    }
{{ /setvar }}
```

不使用 `#` 时，内容会被修剪和去除缩进。使用 `#` 时，所有空白字符都会完全按照书写内容保留——包括开标签后和闭标签前的换行符。

## 注释

使用注释宏添加不会出现在输出中的注解：

```txt
{{// This is a comment and will be removed}}
```

对于多行注释，使用作用域语法：

```txt
{{ // }}
  This entire block is a comment.
  It can span multiple lines.
{{ /// }}
```

## 转义宏

若要显示字面花括号而不进行宏解析，请用反斜杠转义：

```txt
\{\{notAMacro\}\}
```

输出结果为纯文本 `{{notAMacro}}`。

## 变量简写

变量简写为常见变量操作提供了简洁语法。使用 `.` 表示局部变量，使用 `$` 表示全局变量。

### 变量简写前缀

| 前缀 | 名称            | 描述                                                     |
| ------ | --------------- | --------------------------------------------------------------- |
| `.`    | 局部变量  | 局部变量操作的简写。示例：`{{.myvar}}`  |
| `$`    | 全局变量 | 全局变量操作的简写。示例：`{{$myvar}}` |

这些前缀运算符必须紧接在变量名**之前**，位于任何可选出现的[宏标志](#宏标志)之后。它们不被视为宏标志，而是表示正在插入变量简写而非按名称查找宏的标识符。前缀运算符不是变量名的一部分，而是改变变量访问方式的修饰符。

### 变量名

变量名遵循与宏标识符相同的规则：以字母开头，后跟字母、数字、下划线或连字符。最后一个字符不允许是下划线或连字符。

```txt
{{.my-var}}       // 有效
{{.my_var}}       // 有效
{{.myVar123}}     // 有效
```

如果你的变量标识符不符合标准规则，必须使用完整的变量宏语法（例如 `{{getvar::my§var----}}`），或重命名/移动你的变量值。

### 值中的嵌套宏

变量值可以包含嵌套宏：

```txt
{{.greeting = Hello, {{user}}!}}
```

解析后保存 `Hello, User!` 到变量中。（假设 `{{user}}` 的名称为"User"）

### 空白字符处理

运算符周围允许有空白字符：

```txt
{{ .myvar = spaced value }}
{{ .counter ++ }}
```

### 变量简写运算符

以下运算符可与变量简写配合使用。每个运算符遵循 `{{.varName operator value}}` 或 `{{$varName operator value}}` 的模式。

| 运算符 | 名称                      | 示例                | 描述                                            |
| -------- | ------------------------- | ---------------------- | ------------------------------------------------------ |
| *（无）* | [获取](#获取变量)      | `{{.myvar}}`           | 返回变量值                             |
| `=`      | [设置](#设置变量)      | `{{.myvar = value}}`   | 将变量设置为某值，不返回内容         |
| `++`     | [递增](#递增)   | `{{.counter++}}`       | 递增 1，返回新值                     |
| `--`     | [递减](#递减)   | `{{.counter--}}`       | 递减 1，返回新值                     |
| `+=`     | [加法](#加法)               | `{{.score += 10}}`     | 向变量加值（数值或字符串连接），不返回内容 |
| `-=`     | [减法](#减法)     | `{{.health -= 5}}`     | 从变量中减值（仅限数值），不返回内容 |
| `\|\|`   | [逻辑或](#逻辑或) | `{{.name \|\| Guest}}` | 变量为 falsy 时返回回退值                  |
| `??`     | [空值合并](#空值合并) | `{{.name ?? Guest}}` | 仅当变量未定义时返回回退值 |
| `\|\|=`  | [逻辑或赋值](#逻辑或赋值) | `{{.name \|\|= Guest}}` | 变量为 falsy 时设置值，返回新值 |
| `??=`    | [空值合并赋值](#空值合并赋值) | `{{.name ??= Guest}}` | 仅当变量未定义时设置值，返回新值 |
| `==`     | [等于](#等于)         | `{{.status == active}}`| 比较值，返回 `"true"` 或 `"false"`         |
| `!=`     | [不等于](#不等于) | `{{.status != active}}`| 比较值，不相等时返回 `"true"`         |
| `>`      | [大于](#大于) | `{{.score > 50}}` | 变量大于值时返回 `"true"`     |
| `>=`     | [大于或等于](#大于或等于) | `{{.level >= 10}}` | 变量大于或等于值时返回 `"true"` |
| `<`      | [小于](#小于)   | `{{.health < 20}}`     | 变量小于值时返回 `"true"`        |
| `<=`     | [小于或等于](#小于或等于) | `{{.health <= 0}}` | 变量小于或等于值时返回 `"true"` |

#### 获取变量

使用简单前缀检索变量值：

```txt
{{.myvar}}       // 获取局部变量"myvar"
{{$myvar}}       // 获取全局变量"myvar"
```

等同于 `{{getvar::myvar}}` 和 `{{getglobalvar::myvar}}`。

#### 设置变量

使用 `=` 运算符设置变量值：

```txt
{{ .myvar = Hello World }}     // 设置局部变量
{{ $myvar = Some value }}      // 设置全局变量
```

等同于 `{{setvar::myvar::Hello World}}` 和 `{{setglobalvar::myvar::Hello World}}`。返回空字符串。

#### 递增

使用 `++` 将数值变量递增 1：

```txt
{{.counter++}}    // 递增局部变量，返回新值
{{$counter++}}    // 递增全局变量，返回新值
```

等同于 `{{incvar counter}}` 和 `{{incglobalvar counter}}`。返回递增后的新值。

#### 递减

使用 `--` 将数值变量递减 1：

```txt
{{.counter--}}    // 递减局部变量，返回新值
{{$counter--}}    // 递减全局变量，返回新值
```

等同于 `{{decvar counter}}` 和 `{{decglobalvar counter}}`。返回递减后的新值。

#### 加法

使用 `+=` 向变量添加数值：

```txt
{{.score += 10}}     // 向局部变量加 10
{{$total += 5}}      // 向全局变量加 5
```

等同于 `{{addvar::score::10}}` 和 `{{addglobalvar::total::5}}`。返回空字符串。

加法运算符也支持向现有字符串变量追加字符串（当两者均非数字时）：

```txt
{{.myvar += {{noop}} | Second block}}   // 当变量原值为"Content"时，解析结果为"Content | Second block"。
                                        // 使用 `{{noop}}` 可以添加否则会被自动修剪的空白字符。
```

#### 减法

使用 `-=` 从变量中减去数值：

```txt
{{.health -= 10}}    // 从局部变量中减去 10
{{$points -= 5}}     // 从全局变量中减去 5
```

等同于 `{{addvar::score::10}}` 和 `{{addglobalvar::total::5}}`，但使用负数/反转数值。返回空字符串。
如果值不是有效数字，则记录警告且变量保持不变。

#### 逻辑或

当变量为 falsy（空字符串、`0`、`false`）时，使用 `||` 提供回退值：

```txt
{{.name || Anonymous}}     // 如果 .name 为空或 falsy，返回"Anonymous"
{{$setting || default}}    // 如果 $setting 为 falsy，返回"default"
```

若变量为 truthy 则返回变量值，否则返回回退值。回退值**仅在需要时才求值**（惰性求值）。

#### 空值合并

仅当变量不存在时，使用 `??` 提供回退值：

```txt
{{.name ?? Guest}}         // 仅当 .name 未定义时返回"Guest"
{{$config ?? default}}     // 仅当 $config 不存在时返回"default"
```

与 `||` 不同，只要变量存在，即使其值为 falsy（空字符串、`0`、`false`）也会返回变量值。回退值**仅在需要时才求值**（惰性求值）。

#### 逻辑或赋值

当变量当前为 falsy 时，使用 `||=` 将其设置为某值：

```txt
{{.name ||= Anonymous}}    // 如果 .name 为 falsy，设置并返回"Anonymous"
{{$count ||= 0}}           // 如果 $count 为 falsy，设置并返回"0"
```

若变量已为 truthy，则返回当前值且不修改。返回最终值（原有值或新设值）。

#### 空值合并赋值

仅当变量不存在时，使用 `??=` 将其设置为某值：

```txt
{{.name ??= Guest}}        // 仅当 .name 未定义时设置并返回"Guest"
{{$config ??= default}}    // 仅当 $config 不存在时设置并返回"default"
```

与 `||=` 不同，若变量已存在则保留 falsy 值（空字符串、`0`、`false`）。返回最终值（原有值或新设值）。

#### 等于

使用 `==` 将变量值与另一个值进行比较：

```txt
{{.status == active}}      // 如果 .status 等于"active"返回"true"，否则返回"false"
{{$mode == dark}}          // 如果 $mode 等于"dark"返回"true"，否则返回"false"
```

执行字符串比较并返回字面字符串 `"true"` 或 `"false"`。
将不存在的变量、null 变量和空变量视为相同。

在 `{{if}}` 条件中非常有用：

```txt
{{if {{.status == active}} }}Active mode{{/if}}
```

#### 不等于

使用 `!=` 比较变量值与另一个值是否不相等：

```txt
{{.status != inactive}}    // 如果 .status 不是"inactive"返回"true"，否则返回"false"
{{$mode != light}}         // 如果 $mode 不是"light"返回"true"，否则返回"false"
```

执行字符串比较，值不同时返回 `"true"`，值相等时返回 `"false"`。
将不存在的变量、null 变量和空变量视为相同。

在 `{{if}}` 条件中非常有用：

```txt
{{if {{.status != disabled}} }}Feature enabled{{/if}}
```

#### 大于

使用 `>` 检查变量的数值是否大于另一个值：

```txt
{{.score > 50}}        // 如果 .score 大于 50 返回"true"
{{$level > 5}}         // 如果 $level 大于 5 返回"true"
```

执行数值比较并返回字面字符串 `"true"` 或 `"false"`。

在 `{{if}}` 条件中非常有用：

```txt
{{if {{.score > 100}} }}High score!{{/if}}
```

#### 大于或等于

使用 `>=` 检查变量的数值是否大于或等于另一个值：

```txt
{{.level >= 10}}       // 如果 .level 至少为 10 返回"true"
{{$points >= 100}}     // 如果 $points 为 100 或更多返回"true"
```

执行数值比较并返回字面字符串 `"true"` 或 `"false"`。

在 `{{if}}` 条件中非常有用：

```txt
{{if {{$level >= 10}} }}Unlocked advanced features{{/if}}
```

#### 小于

使用 `<` 检查变量的数值是否小于另一个值：

```txt
{{.health < 20}}       // 如果 .health 低于 20 返回"true"
{{$timer < 0}}         // 如果 $timer 为负数返回"true"
```

执行数值比较并返回字面字符串 `"true"` 或 `"false"`。

在 `{{if}}` 条件中非常有用：

```txt
{{if {{.health < 20}} }}Low health warning!{{/if}}
```

#### 小于或等于

使用 `<=` 检查变量的数值是否小于或等于另一个值：

```txt
{{.health <= 0}}       // 如果 .health 为 0 或更低返回"true"
{{$attempts <= 3}}     // 如果 $attempts 为 3 或更少返回"true"
```

执行数值比较并返回字面字符串 `"true"` 或 `"false"`。

在 `{{if}}` 条件中非常有用：

```txt
{{if {{.health <= 0}} }}Game over{{/if}}
```

## 旧版语法

为向后兼容，仍支持尖括号标记：

| 旧版语法 | 等效宏 |
|--------|------------------|
| `<USER>` | `{{user}}` |
| `<BOT>` | `{{char}}` |
| `<CHAR>` | `{{char}}` |
| `<GROUP>` | `{{group}}` |
| `<CHARIFNOTGROUP>` | `{{charIfNotGroup}}` |

这些标记在处理过程中会自动转换为等效宏。

> **注意：** 不推荐使用旧版语法。新内容请改用等效的 `{{macro}}` 语法。

## 常用宏分类

!!!tip
使用 `/? macros` 获取完整的可用宏列表及其详细描述。
!!!

### 名称与参与者

| 宏 | 描述 |
|-------|-------------|
| `{{user}}` | 当前用户/人设名称 |
| `{{char}}` | 当前角色名称 |
| `{{group}}` | 群组成员名称的逗号分隔列表（包含已静音成员），单角色聊天中为角色名称 |
| `{{groupNotMuted}}` | 排除已静音成员的群组成员名称逗号分隔列表 |
| `{{charIfNotGroup}}` | 角色名称（群组中为空） |
| `{{notChar}}` | 除当前发言者外所有参与者的逗号分隔列表 |

### 角色卡与人设字段

| 宏 | 描述 |
|-------|-------------|
| `{{description}}` | 角色描述 |
| `{{personality}}` | 角色性格 |
| `{{scenario}}` | 角色场景 |
| `{{persona}}` | 用户人设描述 |
| `{{charPrompt}}` | 角色的主提示词覆盖 |
| `{{charInstruction}}` | 角色的历史后指令覆盖 |
| `{{charDepthPrompt}}` | 角色的 @ 深度注释 |
| `{{charCreatorNotes}}` | 角色卡中的创作者备注 |
| `{{charVersion}}` | 角色的版本号 |
| `{{mesExamples}}` | 角色的对话示例，已格式化为指令模式 |
| `{{mesExamplesRaw}}` | 角色卡中未格式化的对话示例 |
| `{{charFirstMessage}}` | 角色的第一条消息（问候语）。接受可选索引用于备用问候语，例如 `{{charFirstMessage::1}}` |
| `{{original}}` | 用于在角色提示词覆盖中替换的原始消息内容 |

### 聊天历史与消息

| 宏 | 描述 |
|-------|-------------|
| `{{lastMessage}}` | 聊天中的最后一条消息 |
| `{{lastMessageId}}` | 聊天中最后一条消息的索引 |
| `{{lastUserMessage}}` | 聊天中最后一条用户消息 |
| `{{lastCharMessage}}` | 聊天中最后一条角色/机器人消息 |
| `{{firstIncludedMessageId}}` | 当前上下文中第一条被包含消息的索引 |
| `{{firstDisplayedMessageId}}` | 聊天中第一条显示消息的索引 |
| `{{lastSwipeId}}` | 最后一条消息最后一次滑动的基于 1 的索引 |
| `{{currentSwipeId}}` | 当前滑动的基于 1 的索引 |
| `{{allChatRange}}` | 提供整个聊天的范围（例如 `0-{{lastMessageId}}`），适用于接受消息范围的命令 |
| `{{summary}}` | 来自"Summarize"扩展的最新聊天摘要（当可用时） |

### 时间与日期

| 宏 | 描述 |
|-------|-------------|
| `{{time}}` | 当前本地时间 |
| `{{time::UTC±(offset)}}` | 带 UTC 偏移的时间 |
| `{{date}}` | 当前本地日期（短格式） |
| `{{weekday}}` | 当前星期几 |
| `{{isotime}}` | HH:mm 格式的当前时间 |
| `{{isodate}}` | YYYY-MM-DD 格式的当前日期 |
| `{{datetimeformat::format}}` | 自定义格式的日期/时间（例如 `YYYY-MM-DD HH:mm:ss`） |
| `{{idleDuration}}` | 距上次用户消息的人类可读持续时间 |
| `{{timeDiff::left::right}}` | 两个时间之间的人类可读差值 |

### 变量

| 宏 | 描述 |
|-------|-------------|
| `{{getvar::name}}` | 获取局部变量值 |
| `{{setvar::name::value}}` | 设置局部变量 |
| `{{addvar::name::value}}` | 向局部变量添加值（数值或字符串追加） |
| `{{incvar::name}}` | 将局部变量递增 1 并返回新值 |
| `{{decvar::name}}` | 将局部变量递减 1 并返回新值 |
| `{{hasvar::name}}` | 检查局部变量是否存在（返回"true"或"false"） |
| `{{deletevar::name}}` | 删除局部变量 |
| `{{getglobalvar::name}}` | 获取全局变量值 |
| `{{setglobalvar::name::value}}` | 设置全局变量 |
| `{{addglobalvar::name::value}}` | 向全局变量添加值（数值或字符串追加） |
| `{{incglobalvar::name}}` | 将全局变量递增 1 并返回新值 |
| `{{decglobalvar::name}}` | 将全局变量递减 1 并返回新值 |
| `{{hasglobalvar::name}}` | 检查全局变量是否存在（返回"true"或"false"） |
| `{{deleteglobalvar::name}}` | 删除全局变量 |

### 随机化

| 宏 | 描述 |
|-------|-------------|
| `{{random::a::b::c}}` | 随机选择（每次重新随机） |
| `{{pick::a::b::c}}` | 稳定随机选择（在同一聊天和位置保持一致）。可通过 `/reroll-pick` 命令重新随机 |
| `{{roll::1d20}}` | 使用 droll 语法进行骰子投掷 |

### 运行时状态

| 宏 | 描述 |
|-------|-------------|
| `{{maxPrompt}}` | 最大提示词上下文大小（提示词 token 数 = 上下文 token 数 - 回复 token 数） |
| `{{maxContextTokens}}` | 当前生成设置的最大上下文 token 数 |
| `{{maxResponseTokens}}` | 当前生成设置的最大回复 token 数 |
| `{{model}}` | 当前选定 API 的模型名称 |
| `{{isMobile}}` | 在移动环境中运行时为"true"，否则为"false" |
| `{{lastGenerationType}}` | 上次排队生成请求的类型（例如"normal"、"impersonate"、"regenerate"、"quiet"、"swipe"、"continue"） |
| `{{hasExtension::name}}` | 检查扩展是否激活（返回"true"或"false"）。按扩展名匹配，不区分大小写 |

### 提示词模板

| 宏 | 描述 |
|-------|-------------|
| `{{systemPrompt}}` | 当前活动系统提示词文本（可选由角色覆盖） |
| `{{defaultSystemPrompt}}` | 默认系统提示词 |
| `{{authorsNote}}` | 作者注释内容 |
| `{{charAuthorsNote}}` | 角色作者注释内容 |
| `{{defaultAuthorsNote}}` | 默认作者注释内容 |
| `{{instructStoryStringPrefix}}` | 指令故事字符串前缀 |
| `{{instructStoryStringSuffix}}` | 指令故事字符串后缀 |
| `{{instructUserPrefix}}` | 指令输入/用户前缀序列 |
| `{{instructUserSuffix}}` | 指令输入/用户后缀序列 |
| `{{instructAssistantPrefix}}` | 指令输出/助手前缀序列 |
| `{{instructAssistantSuffix}}` | 指令输出/助手后缀序列 |
| `{{instructSeparator}}` | 指令分隔符序列 |
| `{{instructSystemPrefix}}` | 指令系统前缀序列 |
| `{{instructSystemSuffix}}` | 指令系统后缀序列 |
| `{{instructFirstAssistantPrefix}}` | 指令第一个助手/输出前缀序列 |
| `{{instructLastAssistantPrefix}}` | 指令最后一个助手/输出前缀序列 |
| `{{instructFirstUserPrefix}}` | 指令第一个用户/输入前缀序列 |
| `{{instructLastUserPrefix}}` | 指令最后一个用户/输入前缀序列 |
| `{{instructStop}}` | 指令停止序列 |
| `{{instructUserFiller}}` | 指令用户对齐填充符 |
| `{{instructSystemInstructionPrefix}}` | 指令系统指令前缀序列 |
| `{{chatSeparator}}` | 文本补全提示词中示例聊天块之间的分隔符 |
| `{{chatStart}}` | 文本补全提示词中的聊天开始标记 |
| `{{reasoningPrefix}}` | 推理块之前使用的前缀字符串 |
| `{{reasoningSuffix}}` | 推理块之后使用的后缀字符串 |
| `{{reasoningSeparator}}` | 内容与回复之间的分隔符 |
| `{{charPrefix}}` | 角色的正面图像生成提示词前缀 |
| `{{charNegativePrefix}}` | 角色的负面图像生成提示词前缀 |

### 实用工具

| 宏 | 描述 |
|-------|-------------|
| `{{newline}}` | 插入换行符 |
| `{{newline::count}}` | 插入多个换行符 |
| `{{space}}` | 插入空格字符 |
| `{{space::count}}` | 插入多个空格 |
| `{{noop}}` | 不执行任何操作，产生空字符串 |
| `{{trim}}` | 移除周围的换行符 |
| `{{reverse::text}}` | 反转字符串 |
| `{{input}}` | 当前聊天输入框内容 |
| `{{banned::word}}` | 为文本补全后端禁用某个词 |
| `{{outlet::key}}` | 返回指定出口键的世界信息出口提示词 |
