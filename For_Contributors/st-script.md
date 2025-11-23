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

### Quotes

To use a literal quote-character inside a quoted value, the character must be escaped.

```stscript
/echo title="a \"b\" c" d "e" f
```

### Spaces

To use space in the value of a named argument, you either have to surround the value in quote, or escape the space character.

```stscript
/echo title="a b" c d |
/echo title=a\ b c d
```

### Closure Delimiters

If you want to use the character combinations used to mark the beginning or end of a closure, you have to escape the sequence with a single backslash.

```stscript
/echo \{: |
/echo \:}
```

## Pipe Breakers

```stscript
||
```

To prevent the previous command's output from being automatically injected as the unnamed argument into the next command, put double pipes between the two commands.

```stscript
/echo we don't want to pass this on ||
/world
```

## Closures

```stscript
{: ... :}
```

Closures (block statements, lambdas, anonymous functions, whatever you want to call them) are a series of commands wrapped between `{:` and `:}`, that are only evaluated once that part of the code is executed.

### Sub-Commands

Closures make using sub-commands a lot easier and get rid of the need to escape pipes and macros.

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

### Scopes

Closures have their own scope and support scoped variables. Scoped variables are declared with `/let`, their values set and retrieved with `/var`. Another way to get a scoped variable is the `{{var::}}` macro.

```stscript
/let x |
/let y 2 |
/var x 1 |
/var y |
/echo x is {{var::x}} and y is {{pipe}}.
```

Within a closure, you have access to all variables declared within that same closure or in one of its ancestors. You don't have access to variables declared in a closure's descendants.  
If a variable is declared with the same name as a variable that was declared in one of the closure's ancestors, you don't have access to the ancestor variable in this closure and its descendants.

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

### Named Closures

```stscript
/let x {: ... :} | /:x
```

Closures can be assigned to variables (only scoped variables) to be called at a later point or to be used as sub-commands.

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

`/:` can also be used to execute Quick Replies, as it is just a shorthand for `/run`.

```stscript
/:QrSetName.QrButtonLabel |
/run QrSetName.QrButtonLabel
```

### Closure Arguments

Named closures can take named arguments, just like slash commands. The arguments can have default values.

```stscript
/let myClosure {: a=1 b=
    /echo a is {{var::a}} and b is {{var::b}}
:} |
/:myClosure b=10
```

### Closures and Piped Arguments

The piped value from a parent closure will not be automatically injected into the first command of a child closure.  
You can still explicitly reference the parent's piped value with `{{pipe}}`, but if you leave the unnamed argument of the first command inside a closure blank, the value will *not* be automatically injected.

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

### Immediately Executed Closures

```stscript
{: ... :}()
```

Closures can be immediately executed, meaning they will be replaced with their return value. This is helpful in places where no explicit support for closures exists, and to shorten some commands that would otherwise require a lot of intermediate variables.

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

In addition to running named closures saved inside scoped variables, the `/run` command can also be used to execute closures immediately.

```stscript
/run {:
	/add 1 2 3 4 |
:} |
/echo |
```

## Comments

```stscript
// ... | /# ...
```

A comment is a human-readable explanation or annotation in the script code. Comments don't break pipes.

```stscript
// this is a comment |
/echo foo |
/# this is also a comment
```

### Block Comments

Block comments can be used to quickly comment out multiple commands at once. They will not terminate on a pipe.

```stscript
/echo foo |
/*
/echo bar |
/echo foobar |
*|
/echo foo again |
```


## Flow Control

### Loops: `/while` and `/times`

If you need to run some command in a loop until a certain condition is met, use the `/while` command.

```stscript
/while left=valueA right=valueB rule=operation guard=on "commands"
```

On each step of the loop it compares the value of variable A with the value of variable B, and if the condition yields true, then executes any valid slash command enclosed in quotes, otherwise exists the loop. This command doesn't write anything to the output pipe.

#### Arguments for `/while`

**The set of available boolean comparisons, handing of variables, literal values, and subcommands is the same as for the `/if` command.**

The optional `guard` named argument (`on` by default) is used to protect against endless loops, limiting the number of iterations to 100.
To disable and allow endless loops, set `guard=off`.

This example adds 1 to the value of `i` until it reaches 10, then outputs the resulting value (10 in this case).

```stscript
/setvar key=i 0 |
/while left=i right=10 rule=lt "/addvar key=i 1" |
/echo {{getvar::i}} |
/flushvar i
```

#### Arguments for `/times`

Runs a subcommand a specified number of times.

`/times (repeats) "(command)"` – any valid slash command enclosed in quotes repeats a number of times, e.g. `/setvar key=i 1 | /times 5 "/addvar key=i 1"` adds 1 to the value of "i" 5 times.
- {{timesIndex}} is replaced with the iteration number (zero-based), e.g. `/times 4 "/echo {{timesIndex}}"` echoes the numbers 0 through 4.
- Loops are limited to 100 iterations by default, pass `guard=off` to disable.

### Breaking out of Loops and Closures

```stscript
/break |
```

The `/break` command can be used to break out of a loop (`/while` or `/times`) or a closure early. The unnamed argument of `/break` can be used to pass a value different from the current pipe along.  
`/break` is currently implemented in the following commands:
- `/while` - exits the loop early
- `/times` - exits the loop early
- `/run` (with a closure or closure via variable) - exits the closure early
- `/:` (with a closure) - exits the closure early

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

## Math operations

- All of the following operations accept a series of numbers or variable names and output the result to the pipe.
- Invalid operations (such as division by zero), and operations that result in a NaN value or infinity return zero.
- Multiplication, addition, minimum and maximum accept an unlimited number of arguments separated by spaces.
- Subtraction, division, exponentiation, and modulo accept two arguments separated by spaces.
- Sine, cosine, natural logarithm, square root, absolute value, and rounding accept one argument.

**List of operations:**

1. `/add (a b c d)` – performs an addition of the set of values, e.g. `/add 10 i 30 j`
2. `/mul (a b c d)` – performs a multiplication of the set of values, e.g. `/mul 10 i 30 j`
3. `/max (a b c d)` – returns a maximum from the set of values, e.g. `/max 1 0 4 k`
4. `/min (a b c d)` – return a minimum from the set of values, e.g. `/min 5 4 i 2`
5. `/sub (a b)` – performs a subtraction of two values, e.g. `/sub i 5`
6. `/div (a b)` – performs a division of two values, e.g. `/div 10 i`
7. `/mod (a b)` – performs a modulo operation of two values, e.g. `/mod i 2`
8. `/pow (a b)` – performs a power operation of two values, e.g. `/pow i 2`
9. `/sin (a)` – performs a sine operation of a value, e.g. `/sin i`
10. `/cos (a)` – performs a cosine operation of a value, e.g. `/cos i`
11. `/log (a)` – performs a natural logarithm operation of a value, e.g. `/log i`
12. `/abs (a)` – performs an absolute value operation of a value, e.g. `/abs -10`
13. `/sqrt (a)`– performs a square root operation of a value, e.g. `/sqrt 9`
14. `/round (a)` – performs a rounding to the nearest integer operation of a value, e.g. `/round 3.14`
15. `/rand (round=round|ceil|floor from=number=0 to=number=1)` – returns a random number between from and to, e.g. `/rand` or `/rand 10` or `/rand from=5 to=10`. Ranges are inclusive. The returned value will contain a fractional part. Use `round` named argument to get an integral value, e.g. `/rand round=ceil` to round up, `round=floor` to round down, and `round=round` to round to nearest.

### Example 1: get an area of a circle with a radius of 50.

```stscript
/setglobalvar key=PI 3.1415 |
/setvar key=r 50 |
/mul r r PI |
/round |
/echo Circle area: {{pipe}}
```

### Example 2: calculate a factorial of 5.

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

## Using the LLM

Scripts can make requests to your currently connected LLM API using the following commands:

- `/gen (prompt)` — generates text using the provided prompt for the selected character and including chat messages.
- `/genraw (prompt)` — generates text using just the provided prompt, ignoring the current character and chat.
- `/trigger` — triggers a normal generation (equivalent to clicking a "Send" button). If in group chat, you can optionally provide a 1-based group member index or a character name to have them reply, otherwise triggers a group round according to the group settings.

### Arguments for `/gen` and `/genraw`

```stscript
/genraw lock=on/off stop=[] instruct=on/off (prompt)
```

- `lock` — can be `on` or `off`. Specifies whether a user input should be blocked while the generation is in progress. Default: `off`.
- `stop` — JSON-serialized array of strings. Adds a custom stop string (if the API supports it) just for this generation. Default: none.
- `instruct` (only `/genraw`) — can be `on` or `off`. Allows to use instruct formatting on the input prompt (if instruct mode is enabled and the API supports it). Set to `off` to force pure prompts. Default: `on`.
- `as` (for Text Completion APIs) — can be `system` (default) or `char`. Defines how the last prompt line will be formatted. `char` will use a character name, `system` will use no or neutral name.

The generated text is then passed through the pipe to the next command and can be saved to a variable or displaced using the I/O capabilities:

```stscript
/genraw Write a funny message from Cthulhu about taking over the world. Use emojis. |
/popup <h3>Cthulhu says:</h3><div>{{pipe}}</div>
```

| ![Cthulhu Says](/static/scripts/cthulhu-says.png) |
|---------------------------------------------------|

or to insert the generated message as a response from your character:

```stscript
/genraw You have been memory wiped, your name is now Lisa and you're tearing me apart. You're tearing me apart Lisa! |
/sendas name={{char}} {{pipe}}
```

## Prompt injections

Scripts can add custom LLM prompt injections, making it essentially an equivalent of unlimited Author's Notes.

- `/inject (text)` — inserts any text into the normal LLM prompt for the current chat, and requires a unique identifier. Saved to chat metadata.
- `/listinjects` — shows a list of all prompt injections added by scripts for the current chat in a system message.
- `/flushinjects` — deletes all prompt injections added by scripts for the current chat.
- `/note (text)` — sets the Author's Note value for the current chat. Saved to chat metadata.
- `/interval` — sets the Author's Note insertion interval for the current chat.
- `/depth` — sets the Author's Note insertion depth for the in-chat position.
- `/position`  — sets the Author's Note position for the current chat.

### Arguments for `/inject`

```stscript
/inject id=IdGoesHere position=chat depth=4 My prompt injection
```

- `id` — an identifier string or a reference to a variable. Consequent calls of `/inject` with the same ID will overwrite the previous text injection. **Required argument.**
- `position` — sets a position for the injection. Default: `after`. Possible values:
  - `after`: after the main prompt.
  - `before`: before main prompt.
  - `chat`: in-chat.
- `depth` — sets an injection depth for the in-chat position. 0 means insertion after the last message, 1 - before the last message, etc. Default: 4.
- Unnamed argument is a text to be injected. An empty string will unset the previous value for the provided identifier.

## Access chat messages

### Read messages

You can access messages in the currently selected chat using the `/messages` command.

```stscript
/messages names=on/off start-finish
```

- The `names` argument is used to specify whether you want to include character names or not, default: `on`.
- In an unnamed argument, it accepts a message index or range in the `start-finish` format. Ranges are inclusive!
- If the range is unsatisfiable, i.e. an invalid index or more messages than exist are requested, then an empty string is returned.
- Messages that are hidden from the prompt (denoted by the ghost icon) are excluded from the output.
- If you want to know the index of the latest message, use the `{{lastMessageId}}` macro, and `{{lastMessage}}` will get you the message itself.

To calculate the start index for a range, for example, when you need to get the last N messages, use variable subtraction.
This example will get you 3 last messages in the chat:

```stscript
/setvar key=start {{lastMessageId}} |
/addvar key=start -2 |
/messages names=off {{getvar::start}}-{{lastMessageId}} |
/setinput
```

### Send messages

A script can send messages as either a user, character, persona, neutral narrator, or add comments.

1. `/send (text)` — adds a message as the currently selected persona.
2. `/sendas name=charname (text)` — adds a message as any character, matching by their name. `name` argument is required. Use the `{{char}}` macro to send as the current character.
3. `/sys (text)` — adds a message from the neutral narrator that doesn't belong to the user or character. The displayed name is purely cosmetic and can be customized with the `/sysname` command.
4. `/comment (text)` — adds a hidden comment that is displayed in the chat but is not visible to the prompt.
5. `/addswipe (text)` — adds a swipe to the last character message. Can't add a swipe to the user or hidden messages.
6. `/hide (message id or range)` — hides one or several messages from the prompt based on the provided message index or inclusive range in the `start-finish` format.
7. `/unhide (message id or range)` — returns one or several messages to the prompt based on the provided message index or inclusive range in the `start-finish` format.

`/send`, `/sendas`, `/sys`, and `/comment` commands optionally accept a named argument `at` with a zero-based numeric value (or a variable name that contains such a value) that specifies an exact position of message insertion. By default new messages are inserted at the end of the chat log.

This will insert a user message at the beginning of the conversation history:

```stscript
/send at=0 Hi, I use Linux.
```

### Delete messages

**These commands are potentially destructive and have no "undo" function. Check the /backups/ folder if you accidentally deleted something important.**

1. `/cut (message id or range)` — cuts one or several messages from the chat based on the provided message index or inclusive range in the `start-finish` format.
2. `/del (number)` — deletes last N messages from the chat.
3. `/delswipe (1-based swipe id)` — deletes a swipe from the last character message based on the provided 1-based swipe ID.
4. `/delname (character name)` — deletes all messages in the current chat that belong to a character with the specified name.
5. `/delchat` — deletes the current chat.

## World Info commands

World Info (also known as Lorebook) is a highly utilitarian tool for dynamically inserting data into the prompt. See the dedicated page for more detailed explanation: [World Info](/Usage/worldinfo.md).

1. `/getchatbook` – gets a name of the chat-bound World Info file or create a new one if was unbound, and pass it down the pipe.
2. `/findentry file=bookName field=fieldName [text]` – finds a UID of the record from the specified file (or a variable pointing to a file name) using fuzzy matching of a field value with the provided text (default field: `key`) and passes the UID down the pipe, e.g. `/findentry file=chatLore field=key Shadowfang`.
3. `/getentryfield file=bookName field=field [UID]` – gets a field value (default field: `content`) of the record with the UID from the specified World Info file (or a variable pointing to a file name) and passes the value down the pipe, e.g. `/getentryfield file=chatLore field=content 123`.
4. `/setentryfield file=bookName uid=UID field=field [text]` – sets a field value (default field: `content`) of the record with the UID (or a variable pointing to UID) from the specified World Info file (or a variable pointing to a file name). To set multiple values for key fields, use a comma-delimited list as a text value, e.g. `/setentryfield file=chatLore uid=123 field=key Shadowfang,sword,weapon`.
5. `/createentry file=bookName key=keyValue [content text]` – creates a new record in the specified file  (or a variable pointing to a file name) with the key and content (both of these arguments are *optional*) and passes the UID down the pipe, e.g. `/createentry file=chatLore key=Shadowfang The sword of the king`.

### Valid entry fields

| Field              | UI element        | Value type      |
|:-------------------|:------------------|:----------------|
| `content`          | Content           | String          |
| `comment`          | Title / Memo      | String          |
| `key`              | Primary Keywords  | List of strings |
| `keysecondary`     | Optional Filter   | List of strings |
| `constant`         | Constant Status   | Boolean (1/0)   |
| `disable`          | Disabled Status   | Boolean (1/0)   |
| `order`            | Order             | Number          |
| `selectiveLogic`   | Logic             | (see below)     |
| `excludeRecursion` | Non-recursable    | Boolean (1/0)   |
| `probability`      | Trigger%          | Number (0-100)  |
| `depth`            | Depth             | Number (0-999)  |
| `position`         | Position          | (see below)     |
| `role`             | Depth Role        | (see below)     |
| `scanDepth`        | Scan Depth        | Number (0-100)  |
| `caseSensitive`    | Case-Sensitive    | Boolean (1/0)   |
| `matchWholeWords`  | Match Whole Words | Boolean (1/0)   |

**Logic values**

- 0 = AND ANY
- 1 = NOT ALL
- 2 = NOT ANY
- 3 = AND ALL

**Position values**

- 0 = before main prompt
- 1 = after main prompt
- 2 = top of Author's Note
- 3 = bottom of Author's Note
- 4 = in-chat at depth
- 5 = top of example messages
- 6 = bottom of example messages

**Role values** (Position = 4 only)
- 0 = System
- 1 = User
- 2 = Assistant

### Example 1: Read a content from the chat lorebook by key

```stscript
/getchatbook | /setvar key=chatLore |
/findentry file={{getvar::chatLore}} field=key Shadowfang |
/getentryfield file={{getvar::chatLore}} field=key |
/echo
```

### Example 2: Create a chat lorebook entry with key and content

```stscript
/getchatbook | /setvar key=chatLore |
/createentry file={{getvar::chatLore}} key="Milla" Milla Basset is a friend of Lilac and Carol. She is a hush basset puppy who possesses the power of alchemy. |
/echo
```

### Example 3: Expand an existing lorebook entry with new information from the chat

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

## Text manipulation

There's a variety of useful text manipulation utility commands to be used in various script scenarios.

1. `/trimtokens` — trims the input to the specified number of text tokens from the start or from the end and outputs the result to the pipe.
2. `/trimstart` — trims the input to the start of the first complete sentence and outputs the result to the pipe.
3. `/trimend` — trims the input to the end of the last complete sentence and outputs the result to the pipe.
4. `/fuzzy` — performs fuzzy matching of the input text to the list of strings, outputting the best string match to the pipe.
5. `/regex name=scriptName [text]` — executes a regex script from the Regex extension for the specified text. The script must be enabled.

### Arguments for `/trimtokens`

```stscript
/trimtokens limit=number direction=start/end (input)
```

1. `direction` sets the direction for trimming, which can be either `start` or `end`. Default: `end`.
2. `limit` sets the amount of tokens to left in the output. Can also specify a variable name containing the number. **Required argument.**
3. Unnamed argument is the input text to be trimmed.

### Arguments for `/fuzzy`

```stscript
/fuzzy list=["candidate1","candidate2"] (input)
```

1. `list` is a JSON-serialized array of strings containing the candidates. Can also specify a variable name containing the list. **Required argument.**
2. Unnamed argument is the input text to be matched. Output is one of the candidates matching the input most closely.

## Autocomplete

- Autocomplete is enabled both on the chat input, and the large Quick Reply editor.
- Autocomplete works anywhere in your input. Even with multiple piped commands and nested closures.
- Autocomplete supports three ways of looking up matching commands (*User Settings* -> *STscript Matching*).

1. **Starts with** The "old" way. Only commands that begin exactly with the typed value will show up.
2. **Includes**  All commands that *include* the type value will show up. Example: When entering `/delete`, the commands `/qr-delete` and `/qr-set-delete` will show up in the autocomplete list (/qr-**delete**, /qr-set-**delete**).
3. **Fuzzy**  All commands that can be fuzzy-matched against the typed value will show up. Example: When entering `/seas`, the command `/sendas` will show up in the autocomplete list (/**se**nd**as**).

- Command arguments are supported by autocomplete as well. The list will show up for required arguments automatically. For optional arguments, press *Ctrl*+*Space* to open the list of available options.
- When entering `/:` to execute a closure or QR, autocomplete will show a list of scoped variables and QRs.
- Autocomplete has limited support for macros (in slash commands). Type `{{` to get a list of available macros.
- Use the *up* and *down* *arrow keys* to select an option from the list of autocomplete options.
- Press *Enter* or *Tab* or *click* on an option to place the option at the cursor.
- Press *Escape* to close the autocomplete list.
- Press *Ctrl*+*Space* to open the autocomplete list or toggle the selected option's details.

## Parser Flags

```stscript
/parser-flag
```

The parser accepts flags to modify its behavior. These flags can be toggled on and off at any point in a script and all following input will be evaluated accordingly.  
You can set your default flags in user settings.

### Strict Escaping

```stscript
/parser-flag STRICT_ESCAPING on |
```

Changes with `STRICT_ESCAPING` enabled are as follows.

#### Pipes

Pipes don't need to be escaped in quoted values.

```stscript
/echo title="a|b" c\|d
```

#### Backslashes

A backslash in front of a symbol can be escaped to provide the literal backslash followed by the functional symbol.

```stscript
// this will echo "foo \", then echo "bar" |
/echo foo \\|
/echo bar
```

```stscript
/echo \\|
/echo \\\|
```

### Replace Variable Macros

```stscript
/parser-flag REPLACE_GETVAR on |
```

This flag helps to avoid double-substitutions when the variable values contain text that could be interpreted as macros. The `{{var::}}` macros get substituted last and no further substitutions happen on the resulting text / variable value.

Replaces all `{{getvar::}}` and `{{getglobalvar::}}` macros with `{{var::}}`.
Behind the scenes, the parser will insert a series of command executors before the command with the replaced macros:

- call `/let` to save the current `{{pipe}}` to a scoped variable
- call `/getvar` or `/getglobalvar` to get the variable used in the macro
- call `/let` to save the retrieved variable to a scoped variable
- call `/return` with the saved `{{pipe}}` value to restore the correct piped value for the next command

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
