# Web Search (网络搜索)

为 LLM 提示添加网络搜索结果。

## 可用源

### Selenium 插件

需要安装并启用官方服务器插件。

详见 [SillyTavern-WebSearch-Selenium](https://github.com/SillyTavern/SillyTavern-WebSearch-Selenium)。

支持 Google 和 DuckDuckGo 搜索引擎。

### Extras API

需要 `websearch` 模块和在主机上安装的 Chrome/Firefox 网络浏览器。

支持 Google 和 DuckDuckGo 搜索引擎。

### SerpApi

需要 SerpApi 密钥并提供对 Google 搜索的访问。

在此处获取密钥：https://serpapi.com/dashboard

### SearXNG

需要 SearXNG 实例 URL（私有或公共）。使用 HTML 格式显示搜索结果。

了解更多：<https://docs.searxng.org/>

### Tavily AI

需要 API 密钥。在此处获取密钥：<https://app.tavily.com/> :icon-lock: 

### KoboldCpp

必须在文本补全 API 设置中提供 KoboldCpp URL。KoboldCpp 版本必须 >= 1.81.1，并且必须在启动时启用 WebSearch 模块：在 GUI 启动器中启用 Network => Enable WebSearch，或在命令行中添加 `--websearch`。

参见：<https://github.com/LostRuins/koboldcpp/releases/tag/v1.81.1>

## 如何使用

1. 确保您使用的是最新版本的 SillyTavern。
2. 通过 SillyTavern 中的"Download Extensions & Assets"菜单安装扩展。
3. 打开"Web Search"扩展设置，设置您的 API 密钥或连接到 Extras，并启用扩展。
4. 在聊天过程中，网络搜索结果将自然地添加到提示中。**只有用户消息会触发搜索。**
5. 要更自然地包含搜索结果，请用单引号包裹搜索查询：```Tell me about the `latest Ryan Gosling movie`.``` 将产生搜索查询 `latest Ryan Gosling movie`。
6. 您可以根据喜好配置设置。

## 设置

### 常规

1. 启用 - 打开和关闭扩展。
2. 源 - 设置搜索结果源。
3. 缓存生命周期 - 搜索结果在您的提示中缓存的时间（以秒为单位）。默认 = 一周。

### 提示设置

1. 提示预算 - 设置插入文本的最大容量（以文本字符为单位，而不是标记）。经验法则：1 个标记 ~ 3-4 个字符，根据您的模型上下文限制进行调整。默认 = 1500 个字符。
2. 插入模板 - 结果如何插入到提示中。支持常用宏 + 特殊宏：\{\{query\}\} 用于搜索查询，\{\{text\}\} 用于搜索结果。
3. 注入位置 - 结果在提示中的位置。与作者注释相同的选项：作为聊天内注入或在系统提示之前/之后。

### 搜索激活

1. 使用函数工具 - 使用[函数调用](/For_Contributors/Function-Calling.md)来激活搜索或抓取网页。必须使用支持的聊天补全 API，并在 AI 响应设置中启用。**启用时禁用所有其他激活方法。**
2. 使用反引号 - 启用使用单引号包裹的单词进行搜索激活。
3. 使用触发短语 - 启用使用触发短语进行搜索激活。
4. 正则表达式 - 提供 JS 风格的正则表达式来匹配用户消息。如果正则表达式匹配，将触发具有给定查询的搜索。搜索查询支持 `{{macros}}` 和 $1 语法来引用匹配的组。示例：`/what is happening in (.*)/i` 正则表达式，搜索查询为 `news in $1`，将匹配包含 `what is happening in New York` 的消息，并触发查询 `news in New York` 的搜索。
5. 触发短语 - 逐个添加将触发搜索的短语。它可以在消息的任何位置，查询从触发词开始，总共跨越到"最大单词数"。要排除特定消息的处理，它必须以句点开头，例如 `.What do you think?`。触发器优先级：首先按文本框中的顺序，然后是用户消息中的第一个。
6. 最大单词数 - 搜索查询中包含的单词数（包括触发短语）。Google 每个提示的限制约为 32 个单词。默认 = 10 个单词。

### 页面抓取

1. 访问链接 - 将从访问的搜索结果页面提取文本并保存到文件附件中。
2. 访问计数 - 将访问和解析多少个链接的文本。
3. 访问域名黑名单 - 要排除访问的网站域名。每行一个。
4. 文件头 - 文件头模板，插入到文本文件的开头，有一个额外的 \{\{query\}\} 宏。
5. 块头 - 链接块模板，与每个链接的解析内容一起插入。使用 \{\{link\}\} 宏表示页面 URL，\{\{text\}\} 表示页面内容。
6. 保存目标 - 保存抓取结果的位置。可能的选项：触发消息附件，或 Data Bank 的聊天附件。

## 更多信息

最新查询的搜索结果将保留在提示中，直到找到下一个有效查询。
如果您想问额外的问题而不意外触发搜索，请以句点开始您的消息。

!!! info
如果启用并可用，Web Search 函数工具始终会覆盖其他触发器。
!!!

触发器优先级（如果启用多个）：

1. 反引号。
2. 正则表达式。
3. 触发短语。

要丢弃所有先前查询的处理，请在用户消息开头加上感叹号，例如，用户消息 `!Now let's talk about...` 将丢弃此消息和它上面的每条消息。

此扩展还提供了一个 `/websearch` 斜杠命令，用于在 STscript 中使用。更多信息请参见：[STscript 语言参考](/For_Contributors/st-script.md#extension-commands)

```
/websearch (links=on|off snippets=on|off [query]) – 执行网络搜索查询。使用命名参数指定要返回的内容 - 页面片段（默认：on），完整解析页面（默认：off）或两者都返回。

示例：/websearch links=off snippets=on how to make a sandwich
```

### 搜索结果中可以包含什么？

#### SerpApi

1. 答案框。问题的直接答案。
2. 知识图谱。关于主题的百科知识。
3. 页面片段（最多 10 个）。来自网页的相关摘录。
4. 相关问题（最多 10 个）。类似主题的问题和答案。

#### Selenium 插件和 Extras API

1. Google - 答案框、知识图谱、页面片段。
2. DuckDuckGo - 页面片段。

#### SearXNG

1. 信息框。
2. 页面片段。

#### Tavily AI

1. 答案。
2. 页面内容。

#### KoboldCpp

1. 页面标题。
2. 页面片段。
