---
order: 0
icon: file-symlink-file
route: /usage/st-script
---

# STscript 语言参考

## 什么是 STscript？

这是一个简单但功能强大的脚本语言，可用于在不需要严格编程的情况下扩展 SillyTavern 的功能，允许您：

- 创建迷你游戏或速通挑战
- 构建 AI 驱动的聊天洞察
- 释放您的创造力并与他人分享

STscript 是基于斜杠命令引擎构建的，利用命令批处理、数据管道、宏和变量。
这些概念将在以下文档中详细描述。

### 安全注意事项

能力越大，责任越大。在执行脚本之前，请务必仔细检查。

## Hello, World!

要运行您的第一个脚本，打开任何 SillyTavern 聊天并在聊天输入栏中输入以下内容：

```stscript
/pass Hello, World! | /echo
```

| ![Hello World](/static/scripts/hello-world.png) |
|-------------------------------------------------|

您应该会在屏幕顶部看到一条提示消息。现在让我们逐步分析。

脚本是一批命令，每个命令都以斜杠开头，可以带有或不带有命名和未命名参数，并以命令分隔符结束：`|`。

命令按顺序依次执行，并在彼此之间传输数据。

1. `/pass` 命令接受一个常量值 "Hello, World!" 作为未命名参数并将其写入管道。
2. `/echo` 命令通过管道从前一个命令接收值并将其显示为提示通知。

> **提示：** 要查看所有可用命令的列表，请在聊天中输入 `/help slash`。

由于常量未命名参数和管道是可互换的，我们可以简单地将这个脚本重写为：

```stscript
/echo Hello, World!
```

## 用户输入

现在让我们为脚本添加一些交互性。我们将接受用户的输入值并在通知中显示它。

```stscript
/input Enter your name |
/echo Hello, my name is {{pipe}}
```

1. `/input` 命令用于显示一个输入框，其中包含未命名参数中指定的提示，然后将输出写入管道。
2. 因为 `/echo` 已经有一个设置输出模板的未命名参数，我们使用 `{{pipe}}` 宏来指定管道值将被渲染的位置。

| ![Slim Shady Input](/static/scripts/slim-input.png) | ![Slim Shady Output](/static/scripts/slim-output.png) |
|-----------------------------------------------------|-------------------------------------------------------|

### 其他输入/输出命令

- `/popup (text)` — 显示一个阻塞弹出窗口，支持简单的 HTML 格式，例如：`/popup <font color=red>I'm red!</font>`。
- `/setinput (text)` — 用提供的文本替换用户输入栏的内容。
- `/speak voice="name" (text)` — 使用选定的 TTS 引擎和语音映射中的角色名称朗读文本，例如 `/speak name="Donald Duck" Quack!`。
- `/buttons labels=["a","b"] (text)` — 显示一个带有指定文本和按钮标签的阻塞弹出窗口。`labels` 必须是 JSON 序列化的字符串数组或包含此类数组的变量名。将点击的按钮标签返回到管道中，如果取消则返回空字符串。文本支持简单的 HTML 格式。

#### `/popup` 和 `/input` 的参数

`/popup` 和 `/input` 支持以下附加命名参数：
- `large=on/off` - 增加弹出窗口的垂直大小。默认：`off`。
- `wide=on/off` - 增加弹出窗口的水平大小。默认：`off`。
- `okButton=string` - 添加自定义"确定"按钮文本的功能。默认：`Ok`。
- `rows=number` - (仅用于 `/input`) 增加输入控件的大小。默认：1。

示例：
```stscript
/popup large=on wide=on okButton="Accept" Please accept our terms and conditions....
```

#### `/echo` 的参数

`/echo` 支持以下 `severity` 附加参数值，用于设置显示消息的样式。
  - `warning`
  - `error`
  - `info` (默认)
  - `success`

示例：

```stscript
/echo severity=error Something really bad happened.
```

## 变量

变量用于在脚本中存储和操作数据，可以使用命令或宏。变量可以是以下类型之一：

- 本地变量 — 保存到当前聊天的元数据中，对其唯一。
- 全局变量 — 保存到 settings.json 中，在整个应用程序中都存在。

1. `/getvar name` 或 `{{getvar\:\:name}}` — 获取本地变量的值。
2. `/setvar key=name value` 或 `{{setvar::name::value}}` — 设置本地变量的值。
3. `/addvar key=name increment` 或 `{{addvar::name::increment}}` — 将 `increment` 添加到本地变量的值中。
4. `/incvar name` 或 `{{incvar::name}}` — 将本地变量的值增加 1。
5. `/decvar name` 或 `{{decvar::name}}` — 将本地变量的值减少 1。
6. `/getglobalvar name` 或 `{{getglobalvar::name}}` — 获取全局变量的值。
7. `/setglobalvar key=name` 或 `{{setglobalvar::name::value}}` — 设置全局变量的值。
8. `/addglobalvar key=name` 或 `{{addglobalvar::name:increment}}` — 将 `increment` 添加到全局变量的值中。
9. `/incglobalvar name` 或 `{{incglobalvar::name}}` — 将全局变量的值增加 1。
10. `/decglobalvar name` 或 `{{decglobalvar::name}}` — 将全局变量的值减少 1。
11. `/flushvar name` — 删除本地变量的值。
12. `/flushglobalvar name` — 删除全局变量的值。

- 之前未定义的变量的默认值是空字符串，或者如果首次在 `/addvar`、`/incvar`、`/decvar` 命令中使用，则为零。
- `/addvar` 命令中的增量如果增量和变量值都可以转换为数字，则执行加法或减法，否则执行字符串连接。
- 如果命令参数接受变量名，并且同名的本地和全局变量都存在，则*本地变量*优先。
- 所有用于变量操作的*斜杠命令*都将结果值写入管道供下一个命令使用。
- 对于*宏*，只有 "get"、"inc" 和 "dec" 类型的宏返回值，"add" 和 "set" 则替换为空字符串。

现在，让我们考虑以下示例：

```stscript
/input What do you want to generate? |
/setvar key=SDinput |
/echo Requesting an image of {{getvar::SDinput}} |
/getvar SDinput |
/imagine
```

1. 用户输入的值保存在名为 `SDinput` 的本地变量中。
2. `getvar` 宏用于在 `/echo` 命令中显示值。
3. `getvar` 命令用于检索变量的值并通过管道传递。
4. 该值传递给 `/imagine` 命令（由图像生成插件提供）作为其输入提示。

由于变量在脚本执行之间保存且不会被清除，您可以在其他脚本中和通过宏引用该变量，它将解析为与示例脚本执行期间相同的值。要确保值被丢弃，请在脚本中添加 `/flushvar` 命令。

### 数组和对象

变量值可以包含 JSON 序列化的数组或键值对（对象）。

示例：
- 数组：`["apple","banana","orange"]`
- 对象：`{"fruits":["apple","banana","orange"]}`

以下修改可以应用于命令以处理这些变量：

- `/len` 命令获取数组中的项目数。
- `index=number/string` 命名参数可以添加到 `/getvar` 或 `/setvar` 及其全局对应项，以通过数组的从零开始的索引或对象的字符串键获取或设置子值。
  - 如果在不存在的变量上使用数字索引，该变量将被创建为空数组 `[]`。
  - 如果在不存在的变量上使用字符串索引，该变量将被创建为空对象 `{}`。
- `/addvar` 和 `/addglobalvar` 命令支持将新值推送到数组类型的变量。

## 流程控制 - 条件语句

您可以使用 `/if` 命令创建基于定义规则分支执行的条件表达式。

`/if left=valueA right=valueB rule=comparison else="(command on false)" "(command on true)"`

让我们看看以下示例：

```stscript
/input What's your favorite drink? |
/if left={{pipe}} right="black tea" rule=eq else="/echo You shall not pass \| /abort" "/echo Welcome to the club, \{\{user\}\}"
```

这个脚本根据用户输入评估所需值，并根据输入值显示不同的消息。

### `/if` 的参数

1. `left` 是第一个操作数。让我们称之为 A。
2. `right` 是第二个操作数。让我们称之为 B。
3. `rule` 是要应用于操作数的操作。
4. `else` 是可选的子命令字符串，如果布尔比较结果为 false 则执行。
5. 未命名参数是如果布尔比较结果为 true 则执行的子命令。

操作数值按以下顺序评估：

1. 数字字面量
2. 本地变量名
3. 全局变量名
4. 字符串字面量

命名参数的字符串值可以用引号转义以允许多词字符串。然后丢弃引号。

### 布尔运算

支持以下布尔比较规则。应用于操作数的操作结果为 true 或 false 值。

1. `eq`（等于）=> A = B
2. `neq`（不等于）=> A != B
3. `lt`（小于）=> A < B
4. `gt`（大于）=> A > B
5. `lte`（小于或等于）=> A <= B
6. `gte`（大于或等于）=> A >= B
7. `not`（一元否定）=> !A
8. `in`（包含子字符串）=> A 包含 B，不区分大小写
9. `nin`（不包含子字符串）=> A 不包含 B，不区分大小写

### 子命令

子命令是包含要执行的斜杠命令列表的字符串。

1. 要在子命令中使用命令批处理，命令分隔符字符应该被转义（见下文）。
2. 由于宏值在进入条件时执行，而不是在执行子命令时执行，因此可以额外转义宏以延迟其评估到子命令执行时间。
3. 子命令执行的结果通过管道传递给 `/if` 之后的命令。
4. 遇到 `/abort` 命令时中断脚本执行。

`/if` 命令可以用作三元运算符。
以下示例将在变量 `a` 等于 5 时将 "true" 字符串传递给下一个命令，否则传递 "false" 字符串。

```stscript
/if left=a right=5 rule=eq else="/pass false" "/pass true" |
/echo
```

## 转义序列

### 宏

宏的转义工作方式与以前一样。但是，使用闭包时，您需要转义宏的频率会大大降低。可以转义两个开始的大括号，或者同时转义开始和结束的大括号对。

```stscript
/echo \{\{char}} |
/echo \{\{char\}\}
```

### 管道

在闭包中不需要转义管道（当用作命令分隔符时）。在任何想要使用字面管道字符而不是命令分隔符的地方，都需要转义它。

```stscript
/echo title="a\|b" c\|d |
/echo title=a\|b c\|d |
```

使用解析器标志 `STRICT_ESCAPING` 时，不需要在引用值中转义管道。

```stscript
/parser-flag STRICT_ESCAPING |
/echo title="a|b" c\|d |
/echo title=a\|b c\|d |
```

### 引号

要在引号值内使用字面引号字符，必须转义该字符。

```stscript
/echo title="a \"b\" c" d "e" f
```

### 空格

要在命名参数的值中使用空格，您必须用引号括起该值，或者转义空格字符。

```stscript
/echo title="a b" c d |
/echo title=a\ b c d
```

### 闭包分隔符

如果要使用用于标记闭包开始或结束的字符组合，必须使用单个反斜杠转义该序列。

```stscript
/echo \{: |
/echo \:}
```

## 管道中断符

```stscript
||
```

要防止前一个命令的输出自动注入到下一个命令的未命名参数中，请在两个命令之间放置双管道符。

```stscript
/echo we don't want to pass this on ||
/world
```

## 闭包

```stscript
{: ... :}
```

闭包（块语句、lambda、匿名函数，随你怎么称呼）是包装在 `{:` 和 `:}` 之间的一系列命令，只有在执行到代码的该部分时才会被评估。

### 子命令

闭包使子命令的使用变得更加容易，并且不再需要转义管道符和宏。

```stscript
// if without closures |
/if left=1 rule=eq right=1
    else="
        /echo not equal \|
        /return 0
    "
    /echo equal \|
    /return \{\{pipe}}
```

```stscript
// if with closures |
/if left=1 rule=eq right=1
    else={:
        /echo not equal |
        /return 0
    :}
    {:
        /echo equal |
        /return {{pipe}}
    :}
```

### 作用域

闭包有自己的作用域并支持作用域变量。作用域变量使用 `/let` 声明，使用 `/var` 设置和检索其值。获取作用域变量的另一种方法是使用 `{{var::}}` 宏。

```stscript
/let x |
/let y 2 |
/var x 1 |
/var y |
/echo x is {{var::x}} and y is {{pipe}}.
```

在闭包内，您可以访问在同一闭包或其祖先之一中声明的所有变量。您无法访问在闭包的后代中声明的变量。
如果声明了与在闭包的祖先之一中声明的变量同名的变量，则在此闭包及其后代中无法访问祖先变量。

```stscript
/let x this is root x |
/let y this is root y |
/return {:
    /echo called from level-1: x is "{{var::x}}" and y is "{{var::y}}" |
    /delay 500 |
    /let x this is level-1 x |
    /echo called from level-1: x is "{{var::x}}" and y is "{{var::y}}" |
    /delay 500 |
    /return {:
        /echo called from level-2: x is "{{var::x}}" and y is "{{var::y}}" |
        /let x this is level-2 x |
        /echo called from level-2: x is "{{var::x}}" and y is "{{var::y}}" |
        /delay 500
    :}()
:}() |
/echo called from root: x is "{{var::x}}" and y is "{{var::y}}"
```

### 命名闭包

```stscript
/let x {: ... :} | /:x
```

闭包可以分配给变量（仅作用域变量），以便稍后调用或用作子命令。

```stscript
/let myClosure {:
    /echo this is my closure
:} |
/:myClosure
```

```stscript
/let myClosure {:
    /echo this is my closure |
    /delay 500
:} |
/times 3 {{var::myClosure}}
```

`/:` 也可用于执行快速回复，因为它只是 `/run` 的简写。

```stscript
/:QrSetName.QrButtonLabel |
/run QrSetName.QrButtonLabel
```

### 闭包参数

命名闭包可以接受命名参数，就像斜杠命令一样。参数可以有默认值。

```stscript
/let myClosure {: a=1 b=
    /echo a is {{var::a}} and b is {{var::b}}
:} |
/:myClosure b=10
```

### 闭包和管道参数

父闭包的管道值不会自动注入到子闭包的第一个命令中。
您仍然可以使用 `{{pipe}}` 显式引用父闭包的管道值，但如果将闭包内第一个命令的未命名参数留空，该值将*不会*被自动注入。

```stscript
/* This used to attempt to change the model to "foo"
   because the value "foo" from the /echo outside of
   the loop was injected into the /model command
   inside of the loop.
   Now it will simply echo the current model without 
   attempting to change it.
*|
/echo foo |
/times 2 {:
	/model |
	/echo |
:} |
```
```stscript
/* You can still recreate the old behavior by
   explicitly using the {{pipe}} macro.
*|
/echo foo |
/times 2 {:
	/model {{pipe}} |
	/echo |
:} |
```

### 立即执行的闭包

```stscript
{: ... :}()
```

闭包可以立即执行，这意味着它们将被替换为其返回值。这在没有明确支持闭包的地方很有用，并且可以缩短一些否则需要大量中间变量的命令。

```stscript
// a simple length comparison of two strings without closures |
/len foo |
/var lenOfFoo {{pipe}} |
/len bar |
/var lenOfBar {{pipe}} |
/if left={{var::lenOfFoo}} rule=eq right={{var:lenOfBar}} /echo yay!
```

```stscript
// the same comparison with immediately executed closures |
/if left={:/len foo:}() rule=eq right={:/len bar:}() /echo yay!
```

除了运行保存在作用域变量中的命名闭包外，`/run` 命令还可用于立即执行闭包。

```stscript
/run {:
	/add 1 2 3 4 |
:} |
/echo |
```

## 注释

```stscript
// ... | /# ...
```

注释是脚本代码中人类可读的解释或标注。注释不会中断管道。

```stscript
// this is a comment |
/echo foo |
/# this is also a comment
```

### 块注释

块注释可用于快速注释掉多个命令。它们不会在管道符处终止。

```stscript
/echo foo |
/*
/echo bar |
/echo foobar |
*|
/echo foo again |
```


## 流程控制

### 循环：`/while` 和 `/times`

如果需要在循环中运行某些命令直到满足特定条件，请使用 `/while` 命令。

```stscript
/while left=valueA right=valueB rule=operation guard=on "commands"
```

在循环的每一步中，它会将变量 A 的值与变量 B 的值进行比较，如果条件为真，则执行引号中的任何有效斜杠命令，否则退出循环。此命令不会向输出管道写入任何内容。

#### `/while` 的参数

**可用的布尔比较集、变量处理、字面值和子命令与 `/if` 命令相同。**

可选的 `guard` 命名参数（默认为 `on`）用于防止无限循环，将迭代次数限制为 100 次。
要禁用并允许无限循环，请设置 `guard=off`。

此示例将 `i` 的值加 1 直到达到 10，然后输出结果值（在这种情况下为 10）。

```stscript
/setvar key=i 0 |
/while left=i right=10 rule=lt "/addvar key=i 1" |
/echo {{getvar::i}} |
/flushvar i
```

#### `/times` 的参数

运行子命令指定的次数。

`/times (repeats) "(command)"` — 引号中的任何有效斜杠命令重复多次，例如 `/setvar key=i 1 | /times 5 "/addvar key=i 1"` 将 "i" 的值加 1 共 5 次。
- {{timesIndex}} 被替换为迭代编号（从零开始），例如 `/times 4 "/echo {{timesIndex}}"` 回显数字 0 到 4。
- 循环默认限制为 100 次迭代，传递 `guard=off` 以禁用。

### 跳出循环和闭包

```stscript
/break |
```

`/break` 命令可用于提前跳出循环（`/while` 或 `/times`）或闭包。`/break` 的未命名参数可用于传递与当前管道不同的值。
`/break` 目前在以下命令中实现：
- `/while` - 提前退出循环
- `/times` - 提前退出循环
- `/run`（带闭包或通过变量的闭包）- 提前退出闭包
- `/:`（带闭包）- 提前退出闭包

```stscript
/times 10 {:
	/echo {{timesIndex}}
	/delay 500 |
	/if left={{timesIndex}} rule=gt right=3 {:
		/break
	:} |
:} |
```

```stscript
/let x {: iterations=2
	/if left={{var::iterations}} rule=gt right=10 {:
		/break too many iterations! |
	:} |
	/times {{var::iterations}} {:
		/delay 500 |
		/echo {{timesIndex}} |
	:} |
:} |
/:x iterations=30 |
/echo the final result is: {{pipe}}
```

```stscript
/run {:
	/break 1 |
	/pass 2 |
:} |
/echo pipe will be one: {{pipe}} |
```

```stscript
/let x {:
	/break 1 |
	/pass 2 |
:} |
/:x |
/echo pipe will be one: {{pipe}} |
```

## 数学运算

- 以下所有运算都接受一系列数字或变量名，并将结果输出到管道。
- 无效运算（例如除以零）以及导致 NaN 值或无穷大的运算返回零。
- 乘法、加法、最小值和最大值接受无限数量的参数，用空格分隔。
- 减法、除法、指数运算和模运算接受用空格分隔的两个参数。
- 正弦、余弦、自然对数、平方根、绝对值和四舍五入接受一个参数。

**运算列表：**

1. `/add (a b c d)` — 对值集进行加法运算，例如 `/add 10 i 30 j`
2. `/mul (a b c d)` — 对值集进行乘法运算，例如 `/mul 10 i 30 j`
3. `/max (a b c d)` — 从值集中返回最大值，例如 `/max 1 0 4 k`
4. `/min (a b c d)` — 从值集中返回最小值，例如 `/min 5 4 i 2`
5. `/sub (a b)` — 对两个值进行减法运算，例如 `/sub i 5`
6. `/div (a b)` — 对两个值进行除法运算，例如 `/div 10 i`
7. `/mod (a b)` — 对两个值进行模运算，例如 `/mod i 2`
8. `/pow (a b)` — 对两个值进行幂运算，例如 `/pow i 2`
9. `/sin (a)` — 对值进行正弦运算，例如 `/sin i`
10. `/cos (a)` — 对值进行余弦运算，例如 `/cos i`
11. `/log (a)` — 对值进行自然对数运算，例如 `/log i`
12. `/abs (a)` — 对值进行绝对值运算，例如 `/abs -10`
13. `/sqrt (a)` — 对值进行平方根运算，例如 `/sqrt 9`
14. `/round (a)` — 对值进行四舍五入到最接近整数的运算，例如 `/round 3.14`
15. `/rand (round=round|ceil|floor from=number=0 to=number=1)` — 返回介于 from 和 to 之间的随机数，例如 `/rand` 或 `/rand 10` 或 `/rand from=5 to=10`。范围是包含的。返回的值将包含小数部分。使用 `round` 命名参数获取整数值，例如 `/rand round=ceil` 向上舍入，`round=floor` 向下舍入，`round=round` 舍入到最接近的值。

### 示例 1：获取半径为 50 的圆的面积。

```stscript
/setglobalvar key=PI 3.1415 |
/setvar key=r 50 |
/mul r r PI |
/round |
/echo Circle area: {{pipe}}
```

### 示例 2：计算 5 的阶乘。

```stscript
/setvar key=input 5 |
/setvar key=i 1 |
/setvar key=product 1 |
/while left=i right=input rule=lte "/mul product i \| /setvar key=product \| /addvar key=i 1" |
/getvar product |
/echo Factorial of {{getvar::input}}: {{pipe}} |
/flushvar input |
/flushvar i |
/flushvar product
```

## 使用 LLM

脚本可以使用以下命令向当前连接的 LLM API 发出请求：

- `/gen (prompt)` — 使用提供的提示词为所选角色生成文本，并包含聊天消息。
- `/genraw (prompt)` — 仅使用提供的提示词生成文本，忽略当前角色和聊天。
- `/trigger` — 触发正常生成（相当于单击"发送"按钮）。如果在群组聊天中，您可以选择提供基于 1 的群组成员索引或角色名称让他们回复，否则根据群组设置触发群组回合。

### `/gen` 和 `/genraw` 的参数

```stscript
/genraw lock=on/off stop=[] instruct=on/off (prompt)
```

- `lock` — 可以是 `on` 或 `off`。指定在生成进行时是否应阻止用户输入。默认值：`off`。
- `stop` — JSON 序列化的字符串数组。仅为此生成添加自定义停止字符串（如果 API 支持）。默认值：无。
- `instruct`（仅 `/genraw`）— 可以是 `on` 或 `off`。允许在输入提示词上使用指令格式（如果启用了指令模式并且 API 支持）。设置为 `off` 以强制纯提示词。默认值：`on`。
- `as`（用于文本补全 API）— 可以是 `system`（默认）或 `char`。定义最后一行提示词的格式。`char` 将使用角色名称，`system` 将不使用或使用中性名称。

然后，生成的文本通过管道传递到下一个命令，并可以保存到变量或使用 I/O 功能显示：

```stscript
/genraw Write a funny message from Cthulhu about taking over the world. Use emojis. |
/popup <h3>Cthulhu says:</h3><div>{{pipe}}</div>
```

| ![Cthulhu Says](/static/scripts/cthulhu-says.png) |
|---------------------------------------------------|

或将生成的消息作为角色的回应插入：

```stscript
/genraw You have been memory wiped, your name is now Lisa and you're tearing me apart. You're tearing me apart Lisa! |
/sendas name={{char}} {{pipe}}
```

## 提示词注入

脚本可以添加自定义 LLM 提示词注入，使其本质上相当于无限的作者注释。

- `/inject (text)` — 将任何文本插入到当前聊天的正常 LLM 提示词中，并需要唯一标识符。保存到聊天元数据。
- `/listinjects` — 在系统消息中显示脚本为当前聊天添加的所有提示词注入的列表。
- `/flushinjects` — 删除脚本为当前聊天添加的所有提示词注入。
- `/note (text)` — 设置当前聊天的作者注释值。保存到聊天元数据。
- `/interval` — 设置当前聊天的作者注释插入间隔。
- `/depth` — 设置聊天内位置的作者注释插入深度。
- `/position` — 设置当前聊天的作者注释位置。

### `/inject` 的参数

```stscript
/inject id=IdGoesHere position=chat depth=4 My prompt injection
```

- `id` — 标识符字符串或对变量的引用。使用相同 ID 的 `/inject` 的后续调用将覆盖以前的文本注入。**必需参数。**
- `position` — 设置注入的位置。默认值：`after`。可能的值：
  - `after`：在主提示词之后。
  - `before`：在主提示词之前。
  - `chat`：聊天内。
- `depth` — 设置聊天内位置的注入深度。0 表示在最后一条消息之后插入，1 - 在最后一条消息之前，等等。默认值：4。
- 未命名参数是要注入的文本。空字符串将取消设置所提供标识符的先前值。

## 访问聊天消息

### 读取消息

您可以使用 `/messages` 命令访问当前选定聊天中的消息。

```stscript
/messages names=on/off start-finish
```

- `names` 参数用于指定是否要包含角色名称，默认值：`on`。
- 在未命名参数中，它接受 `start-finish` 格式的消息索引或范围。范围是包含的！
- 如果范围无法满足，即请求的索引无效或消息数量超过存在的消息数量，则返回空字符串。
- 从提示词中隐藏的消息（由幽灵图标表示）将从输出中排除。
- 如果您想知道最新消息的索引，请使用 `{{lastMessageId}}` 宏，`{{lastMessage}}` 将为您获取消息本身。

要计算范围的起始索引，例如，当您需要获取最后 N 条消息时，请使用变量减法。
此示例将为您获取聊天中的最后 3 条消息：

```stscript
/setvar key=start {{lastMessageId}} |
/addvar key=start -2 |
/messages names=off {{getvar::start}}-{{lastMessageId}} |
/setinput
```

### 发送消息

脚本可以以用户、角色、人设、中立叙述者的身份发送消息，或添加注释。

1. `/send (text)` — 以当前选定的人设添加消息。
2. `/sendas name=charname (text)` — 以任何角色添加消息，按其名称匹配。`name` 参数是必需的。使用 `{{char}}` 宏以当前角色发送。
3. `/sys (text)` — 添加来自中立叙述者的消息，该消息不属于用户或角色。显示的名称纯粹是装饰性的，可以使用 `/sysname` 命令自定义。
4. `/comment (text)` — 添加在聊天中显示但对提示词不可见的隐藏注释。
5. `/addswipe (text)` — 向最后一条角色消息添加滑动。无法向用户或隐藏消息添加滑动。
6. `/hide (message id or range)` — 根据提供的消息索引或 `start-finish` 格式的包含范围从提示词中隐藏一条或多条消息。
7. `/unhide (message id or range)` — 根据提供的消息索引或 `start-finish` 格式的包含范围将一条或多条消息返回到提示词。

`/send`、`/sendas`、`/sys` 和 `/comment` 命令可选择接受命名参数 `at`，其值为从零开始的数值（或包含此类值的变量名），用于指定消息插入的确切位置。默认情况下，新消息插入到聊天日志的末尾。

这将在对话历史的开头插入一条用户消息：

```stscript
/send at=0 Hi, I use Linux.
```

### 删除消息

**这些命令具有潜在破坏性，并且没有"撤消"功能。如果您不小心删除了重要内容，请检查 /backups/ 文件夹。**

1. `/cut (message id or range)` — 根据提供的消息索引或 `start-finish` 格式的包含范围从聊天中剪切一条或多条消息。
2. `/del (number)` — 从聊天中删除最后 N 条消息。
3. `/delswipe (1-based swipe id)` — 根据提供的基于 1 的滑动 ID 从最后一条角色消息中删除滑动。
4. `/delname (character name)` — 删除当前聊天中属于具有指定名称的角色的所有消息。
5. `/delchat` — 删除当前聊天。

## World Info 命令

World Info（也称为 Lorebook）是一个高度实用的工具，用于动态地将数据插入提示词中。有关更详细的说明，请参阅专用页面：[World Info](/Usage/worldinfo.md)。

1. `/getchatbook` — 获取聊天绑定的 World Info 文件的名称，如果未绑定则创建一个新文件，并将其传递到管道。
2. `/findentry file=bookName field=fieldName [text]` — 使用字段值与提供的文本的模糊匹配从指定文件（或指向文件名的变量）中查找记录的 UID（默认字段：`key`），并将 UID 传递到管道，例如 `/findentry file=chatLore field=key Shadowfang`。
3. `/getentryfield file=bookName field=field [UID]` — 从指定的 World Info 文件（或指向文件名的变量）中获取具有 UID 的记录的字段值（默认字段：`content`），并将值传递到管道，例如 `/getentryfield file=chatLore field=content 123`。
4. `/setentryfield file=bookName uid=UID field=field [text]` — 从指定的 World Info 文件（或指向文件名的变量）中设置具有 UID（或指向 UID 的变量）的记录的字段值（默认字段：`content`）。要为关键字字段设置多个值，请使用逗号分隔的列表作为文本值，例如 `/setentryfield file=chatLore uid=123 field=key Shadowfang,sword,weapon`。
5. `/createentry file=bookName key=keyValue [content text]` — 在指定文件（或指向文件名的变量）中创建具有关键字和内容（这两个参数都是*可选的*）的新记录，并将 UID 传递到管道，例如 `/createentry file=chatLore key=Shadowfang The sword of the king`。

### 有效的条目字段

| 字段               | UI 元素           | 值类型          |
|:-------------------|:------------------|:----------------|
| `content`          | 内容              | 字符串          |
| `comment`          | 标题 / 备注       | 字符串          |
| `key`              | 主要关键字        | 字符串列表      |
| `keysecondary`     | 可选过滤器        | 字符串列表      |
| `constant`         | 常量状态          | 布尔值 (1/0)    |
| `disable`          | 禁用状态          | 布尔值 (1/0)    |
| `order`            | 顺序              | 数字            |
| `selectiveLogic`   | 逻辑              | (见下文)        |
| `excludeRecursion` | 非递归            | 布尔值 (1/0)    |
| `probability`      | 触发%             | 数字 (0-100)    |
| `depth`            | 深度              | 数字 (0-999)    |
| `position`         | 位置              | (见下文)        |
| `role`             | 深度角色          | (见下文)        |
| `scanDepth`        | 扫描深度          | 数字 (0-100)    |
| `caseSensitive`    | 区分大小写        | 布尔值 (1/0)    |
| `matchWholeWords`  | 匹配整个单词      | 布尔值 (1/0)    |

**逻辑值**

- 0 = AND ANY（任一）
- 1 = NOT ALL（非全部）
- 2 = NOT ANY（非任一）
- 3 = AND ALL（全部）

**位置值**

- 0 = 在主提示词之前
- 1 = 在主提示词之后
- 2 = 作者注释顶部
- 3 = 作者注释底部
- 4 = 聊天内指定深度
- 5 = 示例消息顶部
- 6 = 示例消息底部

**角色值**（仅位置 = 4）
- 0 = 系统
- 1 = 用户
- 2 = 助手

### 示例 1：通过关键字从聊天 lorebook 中读取内容

```stscript
/getchatbook | /setvar key=chatLore |
/findentry file={{getvar::chatLore}} field=key Shadowfang |
/getentryfield file={{getvar::chatLore}} field=key |
/echo
```

### 示例 2：创建带有关键字和内容的聊天 lorebook 条目

```stscript
/getchatbook | /setvar key=chatLore |
/createentry file={{getvar::chatLore}} key="Milla" Milla Basset is a friend of Lilac and Carol. She is a hush basset puppy who possesses the power of alchemy. |
/echo
```

### 示例 3：使用聊天中的新信息扩展现有 lorebook 条目

```stscript
/getchatbook | /setvar key=chatLore |
/findentry file={{getvar::chatLore}} field=key Milla |
/setvar key=millaUid |
/getentryfield file={{getvar::chatLore}} field=content |
/setvar key=millaContent |
/gen lock=on Tell me more about Milla Basset based on the provided conversation history. Incorporate existing information into your reply: {{getvar::millaContent}} |
/setvar key=millaContent |
/echo New content: {{pipe}} |
/setentryfield file={{getvar::chatLore}} uid=millaUid field=content {{getvar::millaContent}}
```

## 文本操作

有多种有用的文本操作实用命令可在各种脚本场景中使用。

1. `/trimtokens` — 从开始或结束处将输入修剪为指定数量的文本 token，并将结果输出到管道。
2. `/trimstart` — 将输入修剪到第一个完整句子的开始，并将结果输出到管道。
3. `/trimend` — 将输入修剪到最后一个完整句子的结束，并将结果输出到管道。
4. `/fuzzy` — 对输入文本与字符串列表执行模糊匹配，将最佳字符串匹配输出到管道。
5. `/regex name=scriptName [text]` — 为指定文本执行 Regex 扩展中的正则表达式脚本。脚本必须启用。

### `/trimtokens` 的参数

```stscript
/trimtokens limit=number direction=start/end (input)
```

1. `direction` 设置修剪方向，可以是 `start` 或 `end`。默认值：`end`。
2. `limit` 设置输出中剩余的 token 数量。也可以指定包含数字的变量名。**必需参数。**
3. 未命名参数是要修剪的输入文本。

### `/fuzzy` 的参数

```stscript
/fuzzy list=["candidate1","candidate2"] (input)
```

1. `list` 是包含候选项的 JSON 序列化字符串数组。也可以指定包含列表的变量名。**必需参数。**
2. 未命名参数是要匹配的输入文本。输出是与输入最匹配的候选项之一。

## 自动完成

- 自动完成在聊天输入和大型快速回复编辑器中都已启用。
- 自动完成在您输入的任何地方都有效。即使使用多个管道命令和嵌套闭包也是如此。
- 自动完成支持三种查找匹配命令的方式（*用户设置* -> *STscript 匹配*）。

1. **开头匹配** "旧"方式。只有精确以键入值开头的命令才会显示。
2. **包含匹配** 所有*包含*键入值的命令都会显示。示例：输入 `/delete` 时，命令 `/qr-delete` 和 `/qr-set-delete` 将显示在自动完成列表中（/qr-**delete**，/qr-set-**delete**）。
3. **模糊匹配** 所有可以与键入值进行模糊匹配的命令都会显示。示例：输入 `/seas` 时，命令 `/sendas` 将显示在自动完成列表中（/**se**nd**as**）。

- 自动完成也支持命令参数。列表将自动显示必需参数。对于可选参数，按 *Ctrl*+*Space* 打开可用选项列表。
- 输入 `/:` 以执行闭包或 QR 时，自动完成将显示作用域变量和 QR 的列表。
- 自动完成对宏（在斜杠命令中）的支持有限。输入 `{{` 以获取可用宏的列表。
- 使用*向上*和*向下*箭头键从自动完成选项列表中选择选项。
- 按 *Enter* 或 *Tab* 或*单击*选项以将选项放置在光标处。
- 按 *Escape* 关闭自动完成列表。
- 按 *Ctrl*+*Space* 打开自动完成列表或切换所选选项的详细信息。

## 解析器标志

```stscript
/parser-flag
```

解析器接受标志以修改其行为。这些标志可以在脚本中的任何时候打开和关闭，所有后续输入都将相应地进行评估。
您可以在用户设置中设置默认标志。

### 严格转义

```stscript
/parser-flag STRICT_ESCAPING on |
```

启用 `STRICT_ESCAPING` 后的更改如下。

#### 管道符

引号值中的管道符不需要转义。

```stscript
/echo title="a|b" c\|d
```

#### 反斜杠

符号前面的反斜杠可以被转义，以提供后跟功能符号的字面反斜杠。

```stscript
// this will echo "foo \", then echo "bar" |
/echo foo \\|
/echo bar
```

```stscript
/echo \\|
/echo \\\|
```

### 替换变量宏

```stscript
/parser-flag REPLACE_GETVAR on |
```

此标志有助于避免当变量值包含可以解释为宏的文本时的双重替换。`{{var::}}` 宏最后被替换，并且不会对生成的文本/变量值进行进一步的替换。

将所有 `{{getvar::}}` 和 `{{getglobalvar::}}` 宏替换为 `{{var::}}`。
在幕后，解析器将在带有替换宏的命令之前插入一系列命令执行器：

- 调用 `/let` 将当前 `{{pipe}}` 保存到作用域变量
- 调用 `/getvar` 或 `/getglobalvar` 获取宏中使用的变量
- 调用 `/let` 将检索到的变量保存到作用域变量
- 使用保存的 `{{pipe}}` 值调用 `/return` 以恢复下一个命令的正确管道值

```stscript
// the following will echo the last message's id / number |
/setvar key=x \{\{lastMessageId}} |
/echo {{getvar::x}}
```

```stscript
// this will echo the literal text {{lastMessageId}} |
/parser-flag REPLACE_GETVAR |
/setvar key=x \{\{lastMessageId}} |
/echo {{getvar::x}}
```

## Quick Replies: script library and auto-execution

Quick Replies is a built-in SillyTavern extension that provides an easy way to store and execute your scripts.

### Configuring Quick Replies

In order to get started, enable open the extensions panel (stacked blocks icon), and expand the Quick Replies menu.

<div style="display:flex;justify-content:center">

![Quick Reply](/static/scripts/quick-reply.png)

</div>

**Quick Replies are disabled by default, you need to enable them first.** Then you will see a bar appearing above your chat input bar.

You can set the displayed button text label (we recommend using emojis for brevity) and the script that will be executed when you click the button.

The number of buttons is controlled by the **Number of slots** settings (max = 100), adjust it according to your needs and click "Apply" when done.

**Inject user input automatically** recommended to be disabled when using STscript, otherwise it may interfere with your inputs, use the `{{input}}` macro to get the current value of the input bar in scripts instead.

**Quick Reply presets** allow to have multiple sets of predefined Quick Replies and switch between manually or by using the `/qrset (name of set)` command.
Don't forget to click "Update" before switching to a different set to write your changes to the currently used preset!

### Manual execution

Now you can add your first script to the library. Pick any free slot (or create one), type "Click me" into the left box to set the label, then paste this into the right box:

```stscript
/addvar key=clicks 1 |
/if left=clicks right=5 rule=eq else="/echo Keep going..." "/echo You did it!  \| /flushvar clicks"
```

Then click 5 times on the button that appeared above the chat bar.
Every click increments the variable `clicks` by one and displays a different message when the value equals 5 and resets the variable.

### Automatic execution

Open the modal menu by clicking the `⋮` button for the created command.

| ![Automatic execution](/static/scripts/autoexecute.png) |
|---------------------------------------------------------|

In this menu you can do the following:

- Edit the script in a convenient full-screen editor
- Hide the button from the chat bar, making it accessible only for auto-execution.
- Enable automatic execution on one or more of the following conditions:
  * App startup
  * Sending a user message to the chat
  * Receiving an AI message in the chat
  * Opening a character or group chat
  * Triggering a reply from a group member
  * Activating a World Info entry using the same Automation ID
- Provide a custom tool tip for the quick reply (text displayed when hovering over the quick reply in your UI)
- Execute the script for test purposes

Commands are executed automatically only if the Quick Replies extension is enabled.

For example, you can display a message after sending five user messages by adding the following script and setting it to auto-execute on the user message.

```stscript
/addvar key=usercounter 1 |
/echo You've sent {{pipe}} messages. |
/if left=usercounter right=5 rule=gte "/echo Game over! \| /flushvar usercounter"
```

### Debugger

A basic debugger exists inside the expanded Quick Reply editor. Set breakpoints with `/breakpoint |` anywhere in your script. When executing the script from the QR editor, the execution will be interrupted at that point, allowing you to examine the currently available variables, pipe, command arguments, and more, and to step through the rest of the code one by one.

```stscript
/let x {: n=1
	/echo n is {{var::n}} |
	/mul n n |
:} |
/breakpoint |
/:x n=3 |
/echo result is {{pipe}} |
```

| ![QR Editor Debugger](/static/scripts/st-debugger.png) |
|--------------------------------------------------------|

### Calling procedures

A `/run` command can call scripts defined in the Quick Replies by their label, basically providing the ability to define procedures and return results from them. This allows to have reusable script blocks that other scripts could reference. The last result from the procedure's pipe is passed to the next command after it.

```stscript
/run ScriptLabel
```

Let's create two Quick Replies:

***
**Label:**

`GetRandom`

**Command:**

```stscript
/pass {{roll:d100}}
```
***
**Label:**

`GetMessage`

**Command:**
```stscript
/run GetRandom | /echo Your lucky number is: {{pipe}}
```
***

Clicking on the `GetMessage` button will call the `GetRandom` procedure which will resolve the `{{roll}}` macro and pass the number to the caller, displaying it to the user.

- Procedures do not accept named or unnamed arguments, but can reference the same variables as the caller.
- Avoid recursion when calling procedures as it may produce the "call stack exceeded" error if handled unadvisedly.

#### Calling procedures from a different Quick Reply preset

You can call a procedure from a different quick reply preset using the `a.b` syntax, where a = QR preset name and b = QR label name

```stscript
/run QRpreset1.QRlabel1
```

By default, the system will first look for a quick reply label `a.b`, so if one of your labels is literally "QRpreset1.QRlabel1" it will try to run that. If no such label is found, it will search for a QR preset name "QRpreset1" with a QR labeled "QRlabel1".

### Quick Replies management commands

#### Create Quick Reply

* `/qr-create (arguments, [message])` – creates a new Quick Reply, example: `/qr-create set=MyPreset label=MyButton /echo 123`

Arguments:
- `label`    - string - text on the button, e.g., `label=MyButton`
- `set`      - string - name of the QR set, e.g., `set=PresetName1`
- `hidden`   - bool   - whether the button should be hidden, e.g., `hidden=true`
- `startup`  - bool   - auto execute on app startup, e.g., `startup=true`
- `user`     - bool   - auto execute on user message, e.g., `user=true`
- `bot`      - bool   - auto execute on AI message, e.g., `bot=true`
- `load`     - bool   - auto execute on chat load, e.g., `load=true`
- `title`    - bool   - title / tooltip to be shown on button, e.g., `title="My Fancy Button"`

#### Delete Quick Reply

* `/qr-delete (set=string [label])` – deletes Quick Reply

#### Update Quick Reply

* `/qr-update (arguments, [message])` – updates Quick Reply, example: `/qr-update set=MyPreset label=MyButton newlabel=MyRenamedButton /echo 123`

Arguments:
- `newlabel` - string - new text fort the button, e.g. `newlabel=MyRenamedButton`
- `label`    - string - text on the button, e.g., `label=MyButton`
- `set`      - string - name of the QR set, e.g., `set=PresetName1`
- `hidden`   - bool   - whether the button should be hidden, e.g., `hidden=true`
- `startup`  - bool   - auto execute on app startup, e.g., `startup=true`
- `user`     - bool   - auto execute on user message, e.g., `user=true`
- `bot`      - bool   - auto execute on AI message, e.g., `bot=true`
- `load`     - bool   - auto execute on chat load, e.g., `load=true`
- `title`    - bool   - title / tooltip to be shown on button, e.g., `title="My Fancy Button"`

####

* `qr-get` - retrieves all of a Quick Reply's properties, eample: `/qr-get set=myQrSet id=42`

#### Create or update QR preset

* `/qr-presetupdate (arguments [label])` or `/qr-presetadd (arguments [label])`

Arguments:
- `enabled` - bool - enable or disable the preset
- `nosend`  - bool - disable send / insert in user input (invalid for slash commands)
- `before`  - bool - place QR before user input
- `slots`   - int  - number of slots
- `inject`  - bool - inject user input automatically (if disabled use `{{input}}`)

Create a new preset (overrides existing ones), example: `/qr-presetadd slots=3 MyNewPreset`

#### Add QR context menu

* `/qr-contextadd (set=string label=string chain=bool [preset name])` – add context menu preset to a QR, example: `/qr-contextadd set=MyPreset label=MyButton chain=true MyOtherPreset`

#### Remove all context menus

* `/qr-contextclear (set=string [label])` – remove all context menu presets from a QR, example: `/qr-contextclear set=MyPreset MyButton`

#### Remove one context menu

* `/qr-contextdel (set=string label=string [preset name])` – remove context menu preset from a QR, example: `/qr-contextdel set=MyPreset label=MyButton MyOtherPreset`

### Quick Reply value escaping

`|{}` can be escaped with backslash in the QR message / command.

For example, use `/qr-create label=MyButton /getvar myvar \| /echo \{\{pipe\}\}` to create a QR that calls `/getvar myvar | /echo {{pipe}}`.

## Extension commands

SillyTavern extensions (both built-in, downloadable and third-party) can add their own slash command. Below is just an example of the capabilities in the official extensions. The list may be incomplete, make sure to check `/help slash` for the most complete list of available commands.

1. `/websearch (query)` — searches snippets of the web pages online for the specified query and returns the result into the pipe. Provided by the Web Search extension.
2. `/imagine (prompt)` — generates an image using the provided prompt. Provided by the Image Generation extension.
3. `/emote (sprite)` — sets a sprite for the active character by fuzzy matching its name. Provided by the Character Expressions extension.
4. `/costume (subfolder)` — sets a sprite set override for the active character. Provided by the Character Expressions extension.
5. `/music (name)` — force changes a played background music file by its name. Provided by the Dynamic Audio extension.
6. `/ambient (name)` — force changes a played ambient sound file by its name. Provided by the Dynamic Audio extension.
7. `/roll (dice formula)` — adds a hidden message to the chat with the result of a dice roll. Provided by the D&D Dice extension.

## UI interaction

Scripts can also interact with SillyTavern's UI: navigate through the chats or change styling parameters.

### Character navigation

1. `/random` — opens a chat with the random character.
2. `/go (name)` — opens a chat with the character of the specified name. First, searches for the exact name match, then by a prefix, then by a substring.

### UI styling

1. `/bubble` — sets the message style to the "bubble chat" style.
2. `/flat` — sets the message style to the "flat chat" style.
3. `/single` — sets the message style to the "single document" style.
4. `/movingui (name)` — activates a MovingUI preset by name.
5. `/resetui` — resets the MovingUI panels state to their original positions.
6. `/panels` — toggles the UI panels visibility: top bar, left and right drawers.
7. `/bg (name)` — finds and sets a background using fuzzy names matching. Respect the chat background lock state.
8. `/lockbg` — locks the background image for the current chat.
9. `/unlockbg` — unlocks the background image for the current chat.

## More examples

### Generate chat summary (by @IkariDevGIT)

```stscript
/setglobalvar key=summaryPrompt Summarize the most important facts and events that have happened in the chat given to you in the Input header. Limit the summary to 100 words or less. Your response should include nothing but the summary. |
/setvar key=tmp |
/messages 0-{{lastMessageId}} |
/trimtokens limit=3000 direction=end |
/setvar key=s1 |
/echo Generating, please wait... |
/genraw lock=on instruct=off {{instructInput}}{{newline}}{{getglobalvar::summaryPrompt}}{{newline}}{{newline}}{{instructInput}}{{newline}}{{getvar::s1}}{{newline}}{{newline}}{{instructOutput}}{{newline}}The chat summary:{{newline}} |
/setvar key=tmp |
/echo Done! |
/setinput {{getvar::tmp}} |
/flushvar tmp |
/flushvar s1
```

### Buttons popup usage

```stscript
/setglobalvar key=genders ["boy", "girl", "other"] |
/buttons labels=genders Who are you? |
/echo You picked: {{pipe}}
```

### Get Nth Fibonacci's number (using Binet's formula)

> **Hint**: Set value of `fib_no` to the desired number

```stscript
/setvar key=fib_no 5 |
/pow 5 0.5 | /setglobalvar key=SQRT5 |
/setglobalvar key=PHI 1.618033 |
/pow PHI fib_no | /div {{pipe}} SQRT5 |
/round |
/echo {{getvar::fib_no}}th Fibonacci's number is: {{pipe}}
```

### Recursive Factorial (using closures)

```stscript
/let fact {: n=
    /if left={{var::n}} rule=gt right=1
        else={:
            /return 1
        :}
        {:
            /sub {{var::n}} 1 |
            /:fact n={{pipe}} |
            /mul {{var::n}} {{pipe}}
        :}
:} |

/input Calculate factorial of: |
/let n {{pipe}} |
/:fact n={{var::n}} |
/echo factorial of {{var::n}} is {{pipe}}
```
