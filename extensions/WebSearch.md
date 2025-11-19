---
route: /extensions/websearch/
---

# Web Search

将网络搜索结果添加到 LLM prompts 中。

!!! Note
一些 [Chat Completion](/Usage/API_Connections/openai.md) 来源提供内置的网络搜索功能。在这种情况下,此扩展将基本上是多余的。检查 **<i class="fa-solid fa-sliders"></i> AI Response Configuration** 面板中的 "Enable web search" 开关。例如,Claude、Google AI Studio / Vertex AI、xAI 和 OpenRouter 后端都可用此功能。
!!!

## Available sources

### Selenium Plugin

需要安装并启用官方服务器插件。

参见 [SillyTavern-WebSearch-Selenium](https://github.com/SillyTavern/SillyTavern-WebSearch-Selenium) 了解更多详情。

支持 Google 和 DuckDuckGo 引擎。

### Extras API

需要 `websearch` 模块和主机上安装的 Chrome/Firefox 网络浏览器。

支持 Google 和 DuckDuckGo 引擎。

### SerpApi

需要 API key。

在此处获取 key:<https://serpapi.com/dashboard>

### SearXNG

需要 SearXNG 实例 URL(私有或公共)。使用 HTML 格式显示搜索结果。

SearXNG preferences string:从 SearXNG - preferences - COOKIES - Copy preferences hash 获取

了解更多:<https://docs.searxng.org/>

### Tavily AI

需要 API key。

在此处获取 key:<https://app.tavily.com/>

### KoboldCpp

必须在 Text Completion API settings 中提供 KoboldCpp URL。KoboldCpp 版本必须 >= 1.81.1,并且必须在启动时启用 WebSearch 模块:在 GUI launcher 中启用 Network => Enable WebSearch,或在命令行中添加 `--websearch`。

参见:<https://github.com/LostRuins/koboldcpp/releases/tag/v1.81.1>

### Serper

需要 API key。

在此处获取 key:<https://serper.dev/>

## How to use

1. 确保您使用最新版本的 SillyTavern。
2. 通过 SillyTavern 中的 "Download Extensions & Assets" 菜单安装扩展。
3. 打开 "Web Search" 扩展设置,设置您的 API key 或连接到 Extras,并启用扩展。
4. 网络搜索结果将在您聊天时自然地添加到 prompt 中。**只有用户消息会触发搜索。**
5. 要更自然地包含搜索结果,请用单反引号包裹搜索查询:```Tell me about the `latest Ryan Gosling movie`.``` 将产生搜索查询 `latest Ryan Gosling movie`。
6. 可选地,根据您的喜好配置设置。

## Settings

### General

1. Enabled - 打开和关闭扩展。
2. Sources - 设置搜索结果源。
3. Cache Lifetime - 搜索结果为您的 prompt 缓存的时间(以秒为单位)。默认 = 一周。

### Prompt Settings

1. Prompt Budget - 设置插入文本的最大容量(以文本字符为单位,而不是 tokens)。经验法则:1 token ~ 3-4 个字符,根据您的模型上下文限制进行调整。默认 = 1500 个字符。
2. Insertion Template - 结果如何插入到 prompt 中。支持常用 macro + 特殊 macro:\{\{query\}\} 用于搜索查询,\{\{text\}\} 用于搜索结果。
3. Injection Position - 结果在 prompt 中的位置。与 Author's Note 相同的选项:作为 in-chat injection 或在 system prompt 之前/之后。

### Search Activation

1. Use function tool - 使用 [function calling](/For_Contributors/Function-Calling.md) 激活搜索或抓取网页。必须使用支持的 Chat Completion API,并在 AI Response settings 中启用。**启用时禁用所有其他激活方法。**
2. Use Backticks - 启用使用单反引号包裹的单词进行搜索激活。
3. Use Trigger Phrases - 启用使用触发短语进行搜索激活。
4. Regular expressions - 提供 JS 风格的 regex 来匹配用户消息。如果 regex 匹配,将触发具有给定查询的搜索。搜索查询支持 `{{macros}}` 和 $1-syntax 来引用匹配的组。示例:`/what is happening in (.*)/i` regex,搜索查询为 `news in $1`,将匹配包含 `what is happening in New York` 的消息,并触发查询 `news in New York` 的搜索。
5. Trigger Phrases - 逐个添加将触发搜索的短语。它可以在消息的任何位置,查询从触发词开始,总共跨越到 "Max Words"。要从处理中排除特定消息,它必须以句点开头,例如 `.What do you think?`。触发器优先级:首先按文本框中的顺序,然后是用户消息中的第一个。
6. Max Words - 搜索查询中包含多少个单词(包括触发短语)。Google 每个 prompt 的限制约为 32 个单词。默认 = 10 个单词。

### Page Scraping

1. Visit Links - 将从访问的搜索结果页面提取文本并保存到文件附件中。
2. Visit Count - 将访问和解析多少个链接的文本。
3. Visit Domain Blacklist - 要排除访问的网站域名。每行一个。
4. File Header - 文件头模板,插入到文本文件的开头,有一个额外的 \{\{query\}\} macro。
5. Block Header - 链接块模板,与每个链接的解析内容一起插入。使用 \{\{link\}\} macro 表示页面 URL,\{\{text\}\} 表示页面内容。
6. Save Target - 保存抓取结果的位置。可能的选项:触发消息附件、Data Bank 的聊天附件,或仅图像(如果源支持)。
7. Include Images - 将相关图像附加到聊天。需要支持图像的源(见下文)。

## More info

最新查询的搜索结果将保留在 prompt 中,直到找到下一个有效查询。
如果您想问额外的问题而不意外触发搜索,请以句点开始您的消息。

!!!info
如果启用并可用,Web Search function tool 始终会覆盖其他触发器。
!!!

触发器优先级(如果启用多个):

1. Backticks。
2. Regular expressions。
3. Trigger phrases。

要丢弃所有先前查询的处理,请在用户消息开头加上感叹号,例如,用户消息 `!Now let's talk about...` 将丢弃此消息和它上面的每条消息。

此扩展还提供了一个 `/websearch` slash command,用于在 STscript 中使用。更多信息请参见:[STscript Language Reference](/For_Contributors/st-script.md#extension-commands)

```stscript
/websearch (links=on|off snippets=on|off [query]) – 执行网络搜索查询。使用命名参数指定要返回的内容 - 页面片段(默认:on)、完整解析页面(默认:off)或两者。

示例:/websearch links=off snippets=on how to make a sandwich
```

### What can be included in the search result?

**Thesaurus:**

- Answer box:问题的直接答案。
- Knowledge graph:关于主题的百科知识。
- Page snippets:来自网页的相关摘录。
- Relevant questions:类似主题的问题和答案。
- Images:相关图像。

#### SerpApi

1. Answer box。
2. Knowledge graph。
3. Page snippets(最多 10 个)。
4. Relevant questions(最多 10 个)。
5. Images(最多 10 个)。

#### Selenium Plugin and Extras API

1. Google - answer box、knowledge graph、page snippets。
2. DuckDuckGo - page snippets。

**Selenium Plugin** 还可以提供 images。

#### SearXNG

1. Infobox。
2. Page snippets。
3. Images。

#### Tavily AI

1. Answer。
2. Page contents。
3. Images(最多 5 个)。

#### KoboldCpp

1. Page titles。
2. Page snippets。

#### Serper

1. Answer box。
2. Knowledge graph。
3. Page snippets。
4. Relevant questions。
5. Images。
