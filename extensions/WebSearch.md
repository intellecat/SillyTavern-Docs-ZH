---
route: /extensions/websearch/
---

# Web Search（网络搜索）

将网络搜索结果添加到 LLM 提示词中。

!!! 注意
某些 [Chat Completion](/Usage/API_Connections/openai.md) 来源提供内置的网络搜索功能。在这种情况下，此扩展基本上是多余的。请在 **<i class="fa-solid fa-sliders"></i> AI 响应配置**面板中查找"启用网络搜索"开关。例如，Claude、Google AI Studio / Vertex AI、OpenRouter、Chutes 和其他后端均提供此功能。
!!!

## 可用来源

### Selenium 插件

需要安装并启用官方服务器插件。

详情见 [SillyTavern-WebSearch-Selenium](https://github.com/SillyTavern/SillyTavern-WebSearch-Selenium)。

支持 Google 和 DuckDuckGo 引擎。

### Extras API

需要在主机上安装 `websearch` 模块以及 Chrome/Firefox 浏览器。

支持 Google 和 DuckDuckGo 引擎。

### SerpApi

需要 API 密钥。

在此获取密钥：<https://serpapi.com/dashboard>

### SearXNG

需要提供 SearXNG 实例 URL（私有或公开均可）。使用 HTML 格式获取搜索结果。

SearXNG 偏好字符串：从 SearXNG - 偏好设置 - COOKIES - 复制偏好哈希值获取。

了解更多：<https://docs.searxng.org/>

### Tavily AI

需要 API 密钥。

在此获取密钥：<https://app.tavily.com/>

### KoboldCpp

必须在文本补全 API 设置中提供 KoboldCpp URL。KoboldCpp 版本须 >= 1.81.1，且启动时必须启用 WebSearch 模块：在 GUI 启动器中启用"网络 => 启用 WebSearch"，或在命令行中添加 `--websearch`。

详见：<https://github.com/LostRuins/koboldcpp/releases/tag/v1.81.1>

### Serper

需要 API 密钥。

在此获取密钥：<https://serper.dev/>

### Z.AI

需要 API 密钥，请先在 Chat Completion API 设置中配置。与编程 API 订阅不兼容！

在此获取密钥：<https://z.ai/manage-apikey/apikey-list/>

文档：<https://docs.z.ai/api-reference/tools/web-search>

## 使用方法

1. 确保你使用的是最新版本的 SillyTavern。
2. 通过 SillyTavern 中的"下载扩展和资源"菜单安装该扩展。
3. 打开"网络搜索"扩展设置，填写 API 密钥或连接到 Extras，然后启用该扩展。
4. 聊天过程中，网络搜索结果将自然地添加到提示词中。**只有用户消息才会触发搜索。**
5. 要使搜索结果更自然地融入对话，可以用单个反引号包裹搜索查询：```Tell me about the `latest Ryan Gosling movie`.``` 将产生搜索查询 `latest Ryan Gosling movie`。
6. 可选择根据你的喜好配置设置。

## 设置

### 常规

1. 启用——开启或关闭扩展。
2. 来源——设置搜索结果来源。
3. 缓存有效期——搜索结果在提示词中的缓存时长（秒）。默认为一周。

### 提示词设置

1. 提示词预算——设置插入文本的最大容量（以文本字符数计，而非 token 数）。经验法则：1 token 约等于 3-4 个字符，请根据你的模型上下文限制进行调整。默认为 1500 个字符。
2. 插入模板——结果插入提示词的方式。支持常规宏，以及特殊宏：\{\{query\}\} 表示搜索查询，\{\{text\}\} 表示搜索结果。
3. 注入位置——结果在提示词中的位置。与"作者注释"相同的选项：作为聊天内注入，或在系统提示词之前/之后。

### 搜索激活

1. 使用函数工具——使用[函数调用](/For_Contributors/Function-Calling.md)激活搜索或抓取网页。必须使用支持的 Chat Completion API 并在 AI 响应设置中启用。**启用后将禁用所有其他激活方式。**
2. 使用反引号——启用通过单反引号包裹的词语激活搜索。
3. 使用触发短语——启用通过触发短语激活搜索。
4. 正则表达式——提供 JS 风格的正则表达式来匹配用户消息。如果正则匹配，将使用给定查询触发搜索。搜索查询支持 `{{macros}}` 和 $1 语法来引用匹配组。示例：针对搜索查询 `news in $1` 的正则表达式 `/what is happening in (.*)/i` 将匹配包含 `what is happening in New York` 的消息，并触发查询 `news in New York` 的搜索。
5. 触发短语——逐个添加将触发搜索的短语。它可以出现在消息的任意位置，查询从触发词开始，共跨越"最大词数"个词。要将特定消息排除在处理之外，消息必须以句点开头，例如 `.What do you think?`。触发优先级：先按文本框中的顺序，再按用户消息中出现的先后顺序。
6. 最大词数——搜索查询中包含的词数（含触发短语）。Google 每个提示词的限制约为 32 个词。默认为 10 个词。

### 页面抓取

1. 访问链接——从已访问的搜索结果页面提取文本并保存到文件附件中。
2. 访问数量——将被访问和解析文本的链接数量。
3. 访问域名黑名单——要排除访问的网站域名，每行一个。
4. 文件头——文件头模板，插入在文本文件开头，包含额外的 \{\{query\}\} 宏。
5. 块头——链接块模板，与每个链接的解析内容一起插入。使用 \{\{link\}\} 宏表示页面 URL，\{\{text\}\} 表示页面内容。
6. 保存目标——保存抓取结果的位置。可选项：触发消息附件、数据库聊天附件，或仅图像（如果来源支持）。
7. 包含图像——将相关图像附加到聊天中。需要支持图像的来源（见下文）。

## 更多信息

最新查询的搜索结果将持续包含在提示词中，直到找到下一个有效查询。如果你想提出额外问题而不意外触发搜索，请以句点开头发送消息。

!!!info
如果启用了网络搜索函数工具且可用，它将始终覆盖其他触发器。
!!!

触发器优先级（如果启用了多个）：

1. 反引号。
2. 正则表达式。
3. 触发短语。

要从处理中丢弃所有之前的查询，可以用感叹号开头发送用户消息，例如，用户消息 `!Now let's talk about...` 将丢弃该消息及其之上的所有消息。

此扩展还提供了一个 `/websearch` 斜线命令，可在 STscript 中使用。更多信息见：[STscript 语言参考](/For_Contributors/st-script.md#extension-commands)

```stscript
/websearch (links=on|off snippets=on|off [query]) – 执行网络搜索查询。使用命名参数指定返回内容——页面摘要（默认：开启）、完整解析页面（默认：关闭）或两者均返回。

示例：/websearch links=off snippets=on how to make a sandwich
```

### 搜索结果可以包含哪些内容？

**术语表：**

- 答案框：问题的直接答案。
- 知识图谱：关于该主题的百科知识。
- 页面摘要：来自网页的相关摘录。
- 相关问题：类似主题的问题和答案。
- 图像：相关图像。

#### SerpApi

1. 答案框。
2. 知识图谱。
3. 页面摘要（最多 10 条）。
4. 相关问题（最多 10 条）。
5. 图像（最多 10 张）。

#### Selenium 插件和 Extras API

1. Google——答案框、知识图谱、页面摘要。
2. DuckDuckGo——页面摘要。

**Selenium 插件**还可额外提供图像。

#### SearXNG

1. 信息框。
2. 页面摘要。
3. 图像。

#### Tavily AI

1. 答案。
2. 页面内容。
3. 图像（最多 5 张）。

#### KoboldCpp

1. 页面标题。
2. 页面摘要。

#### Serper

1. 答案框。
2. 知识图谱。
3. 页面摘要。
4. 相关问题。
5. 图像。

#### Z.AI

1. 页面标题。
2. 页面摘要。
