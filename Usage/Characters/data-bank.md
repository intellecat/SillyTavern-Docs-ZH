---
order: character-50
route: /usage/core-concepts/data-bank
tags:
    [
        vector storage,
        RAG,
        retrieval-augmented generation,
        vectors,
        documents,
        files,
        attachments,
    ]
---

# 数据库（RAG）

检索增强生成（RAG）是一种为 LLM 提供外部知识来源的技术。它通过访问模型训练数据之外的信息来帮助提高 AI 回答的准确性。

SillyTavern 提供了一套工具，用于从多种来源构建多用途知识库，以及在 LLM 提示词中使用收集的数据。

## 访问数据库

内置的聊天附件扩展（在 1.12.0 及更高版本中默认包含）在"魔法棒"菜单中添加了一个新选项 - 数据库。这是您管理 SillyTavern 中用于 RAG 的文档的中心。

## 关于文档

数据库存储文件附件，也称为文档。文档分为三个可用范围。

1. 全局附件 - 在每个聊天中都可用，无论是单人还是群组。
2. 角色附件 - 仅对当前选择的角色可用，包括他们在群组中回复时。_附件保存在本地，不会随角色卡片导出！_
3. 聊天附件 - 仅在当前打开的聊天中可用。聊天中的每个角色都可以从中获取信息。

!!! info 注意
虽然不是数据库的正式组成部分，但您甚至可以将文件附加到单个消息。使用"魔法棒"菜单中的附加文件选项，或消息操作行中的回形针图标。
!!!

什么可以成为文档？实际上任何可以用纯文本形式表示的内容！

示例包括但不限于：

-   本地文件（书籍、科学论文等）
-   网页（维基百科、文章、新闻）
-   视频转录

各种扩展和插件也可以提供新的方式来收集和处理数据，下面会详细介绍。

## 数据来源

要将文档添加到任何范围，点击"添加"并选择一个可用的来源。

### 记事本

从头创建文本文件，或编辑现有附件。

### 文件

从您的计算机硬盘上传文件。SillyTavern 为常见文件格式提供内置转换器：

-   PDF（仅文本）
-   HTML
-   Markdown
-   ePUB
-   TXT

您也可以附加任何带有非标准扩展名的文本文件，如 JSON、YAML、源代码等。如果所选文件类型没有已知的转换方式，并且文件无法解析为纯文本文档，文件上传将被拒绝，这意味着不允许原始二进制文件。

!!! info 注意
导入 Microsoft Office（DOCX、PPTX、XLSX）和 LibreOffice 文档（ODT、ODP、ODS）需要安装并加载[服务器插件](https://github.com/SillyTavern/SillyTavern-Office-Parser)。有关安装说明，请参阅插件的 README 页面。
!!!

### 网页

通过 URL 从网页抓取文本。然后通过 [Readability](https://github.com/mozilla/readability) 库处理 HTML 文档以提取可用文本。

某些 Web 服务器可能会拒绝获取请求，受 Cloudflare 保护，或严重依赖 JavaScript 来运行。如果您遇到任何特定网站的问题，请通过网络浏览器手动下载页面并使用文件上传器附加它。

### YouTube

通过 ID 或 URL 下载 YouTube 视频的转录文本，可以是创作者上传的或由 Google 自动生成的。某些视频可能已禁用转录，同时由于需要登录，无法解析年龄限制视频。

脚本以视频的默认语言加载。您也可以选择指定两个字母的语言代码，尝试获取特定语言的转录。此功能并非总是可用，可能会失败，因此请谨慎使用。

### 网页搜索

!!! info 注意
此来源需要安装并正确配置[网页搜索](/extensions/WebSearch.md)扩展。有关详细信息，请参阅链接页面。
!!!

执行网页搜索并从搜索结果页面下载文本。这类似于网页来源但完全自动化。将从扩展设置中继承所选搜索引擎，因此请提前设置。

首先，指定搜索查询、要访问的最大链接数，以及输出类型：一个组合文件（根据扩展规则格式化）或每个页面一个单独的文件。您也可以选择保存页面摘要。

### Fandom

!!! info 注意
此来源需要安装并加载[服务器插件](https://github.com/SillyTavern/SillyTavern-Fandom-Scraper)。有关安装说明，请参阅插件的 README 页面。
!!!

通过 ID 或 URL 从 [Fandom](https://www.fandom.com/) wiki 抓取文章。由于某些 wiki 非常大，使用过滤正则表达式限制范围可能会有帮助，它将针对文章标题进行测试。如果没有提供过滤器，则所有页面都将被导出。您可以将它们保存为每个页面的单独文件，或合并为一个文档。

### Bronie 解析器扩展（第三方）

!!! warning 注意
此来源来自第三方，与 SillyTavern 团队**无关**。此来源需要您安装 Bronya Rand 的 [Bronie 解析器扩展](https://github.com/Bronya-Rand/Bronie-Parser-Extension)以及需要解析器工作的服务器插件。
!!!

Bronya Rand 的 Bronie 解析器扩展允许使用第三方抓取器，如米哈游/HoYoverse 的 [HoYoLab](https://wiki.hoyolab.com) 到 SillyTavern，类似于其他数据来源。

目前，Bronya Rand 的 Bronie 解析器扩展支持以下内容：

-   米哈游/HoYoverse 的 HoYoLab（用于原神/崩坏：星穹铁道）通过 [HoYoWiki-Scraper-TS](https://github.com/Bronya-Rand/HoYoWiki-Scraper-TS)

首先，按照其[安装指南](https://github.com/Bronya-Rand/Bronie-Parser-Extension?tab=readme-ov-file#installation)安装 Bronya Rand 的 Bronie 解析器扩展，并在 SillyTavern 中安装支持的服务器插件。重启 SillyTavern 并进入_数据库_菜单。点击`+ 添加`，您应该会看到您最近安装的抓取器已添加到可能的信息来源列表中。

## 向量存储

好的，您已经为您的特定主题建立了一个不错且全面的信息库。接下来呢？

要将文档用于 RAG，您需要使用兼容的扩展，它将相关数据插入到 LLM 提示词中。

向量存储，它与 SillyTavern 捆绑在一起，是这种扩展的参考实现。它使用嵌入（也称为向量）来搜索与您正在进行的聊天相关的文档。

!!! info 有趣的事实

1. 嵌入是抽象表示一段文本的数字数组，由专门的语言模型生成。更相似的文本在它们各自的向量之间有更短的距离。
2. 向量存储扩展使用 [Vectra](https://github.com/Stevenic/vectra) 库来跟踪文件嵌入。它们存储在用户数据目录的 `/vectors` 文件夹中的 JSON 文件中。每个文档在内部由其自己的索引/集合文件表示。
   !!!

由于向量功能默认是禁用的，您需要打开扩展面板（顶部栏上的"堆叠立方体"图标），然后导航到"向量存储"部分，并在"文件向量化设置"下勾选"为文件启用"复选框。

向量存储本身不生成任何向量，您需要使用兼容的嵌入提供者。

## 向量提供者

### 本地

这些来源是免费且无限制的，使用您的 CPU/GPU 计算嵌入。

1. 本地（Transformers）- 在 Node 服务器上运行。SillyTavern 将自动从 HuggingFace 下载兼容的 ONNX 格式模型。默认模型：[jina-embeddings-v2-base-en](https://huggingface.co/Cohee/jina-embeddings-v2-base-en)。
2. Extras - 使用 SentenceTransformers 加载器在 [Extras API](https://github.com/SillyTavern/SillyTavern-extras) 下运行。默认模型：[all-mpnet-base-v2](https://huggingface.co/sentence-transformers/all-mpnet-base-v2)。
3. Ollama - 从 <https://ollama.com/> 获取。在 API 连接菜单中设置 API URL（在文本补全下，默认：`http://localhost:11434`）。必须先下载兼容的模型，然后在扩展设置中设置其名称。示例模型：[mxbai-embed-large](https://ollama.com/library/mxbai-embed-large)。可选择勾选选项以将模型保持在内存中。
4. llama.cpp 服务器 - 从 [ggerganov/llama.cpp](https://github.com/ggerganov/llama.cpp) 获取并使用 `--embedding` 标志运行服务器可执行文件。从 HuggingFace 加载兼容的 GGUF 嵌入模型，例如 [nomic-ai/nomic-embed-text-v1.5-GGUF](https://huggingface.co/nomic-ai/nomic-embed-text-v1.5-GGUF)。
5. vLLM - 从 [vllm-project/vllm](https://github.com/vllm-project/vllm) 获取。首先在 API 连接菜单中设置 API URL 和 API 密钥。

### API 来源

所有这些来源都需要相应服务的 API 密钥，通常有使用成本，但一般来说计算嵌入的成本相当低。

1. OpenAI
2. Cohere
3. Google MakerSuite
4. TogetherAI
5. MistralAI
6. NomicAI

!!! warning 警告
嵌入只有在使用生成它们的相同模型检索时才可用。更改嵌入模型或来源时，建议清除并重新计算文件向量。
!!!

## 向量化设置

在选择了嵌入提供者之后，不要忘记配置其他设置，这些设置将定义处理和检索文档的规则。

!!! info 注意
附件的分割、向量化和信息检索需要一些时间。虽然文件的初始摄取可能需要一段时间，但 RAG 搜索查询通常足够快，不会造成明显的延迟。
!!!

### 消息附件

这些设置控制直接附加到消息的文件。

适用以下规则：

1. 只有适合 LLM 上下文窗口的消息才能检索其附件。
2. 当向量存储扩展被禁用时，文件附件及其附带的消息将完全插入到提示词中。
3. 当启用文件向量化时，文件将被分割成块，只有最相关的部分会被插入，节省上下文空间并让模型保持专注。

-   大小阈值（KB）- 设置分块分割阈值。只有大于指定大小的文件才会被分割。
-   块大小（字符）- 设置单个块的目标大小（以文本字符为单位，而不是模型 token！）。
-   块重叠（%）- 设置相邻块之间共享的块大小百分比。这允许块之间更平滑的过渡，但也可能引入一些冗余。
-   检索块数 - 设置要检索的最相关文件块的最大数量。它们将按原始顺序插入。

### 数据库文件

这些设置控制数据库文档的处理方式。

适用以下规则：

1. 当文件向量化被禁用时，数据库不会被使用。
2. 否则，当前范围内的所有可用文档（见上文）都会被考虑用于查询。只有所有文件中最相关的块会被检索。同一文件的多个块按原始顺序插入。
3. 插入的块将在适应聊天消息之前保留部分上下文。

-   大小阈值（KB）- 设置分块分割阈值。只有大于指定大小的文件才会被分割。
-   块大小（字符）- 设置单个块的目标大小（以文本字符为单位，而不是模型 token！）。
-   块重叠（%）- 设置相邻块之间共享的块大小百分比。这允许块之间更平滑的过渡，但也可能引入一些冗余。
-   检索块数 - 设置要检索的文件块的最大数量。这个限额在所有文件之间共享。
-   注入模板 - 定义如何将检索到的信息插入到提示词中。您可以使用特殊的 \{\{text\}\} 宏来指定检索文本的位置，以及任何其他宏。
-   注入位置 - 设置在哪里插入提示词注入。适用与作者注释和世界信息相同的规则。

### 共享设置

-   查询消息 - 用于查询文档块的最新聊天消息数量。
-   分数阈值 - 调整以允许根据块的相关性分数（0 - 完全不匹配，1 - 完全匹配）筛选块的检索。较高的值允许更准确的检索并防止完全随机的信息进入上下文。合理的值范围在 0.2（更宽松）到 0.5（更专注）之间。
-   包含在世界信息扫描中 - 如果您希望注入的内容激活知识库条目，请勾选。
-   向量化全部 - 强制摄取所有未处理文件的嵌入。
-   清除向量 - 清除文件嵌入，允许重新计算它们的向量。

!!! info 注意
有关"聊天向量化"设置，请参阅[聊天向量化](/extensions/Chat-vectorization.md)。
!!!

## 结论

恭喜！借助 RAG 技术，您的聊天体验已得到显著提升。其潜力无限，全凭您的创意发挥。大胆尝试，勇于探索，让您的聊天体验更上一层楼！
