---
order: 30
route: /usage/core-concepts/data-bank/
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

# Data Bank (RAG)

Retrieval-augmented generation (RAG) 是一种为 LLM 提供外部知识来源的技术。它通过访问模型训练数据之外的信息来帮助提高 AI 答案的准确性。

SillyTavern 提供了一套工具，用于从多种来源构建多用途知识库，以及在 LLM prompts 中使用收集的数据。

## 访问 Data Bank

内置的 Chat Attachments extension（默认包含在 >= 1.12.0 的发行版本中）在 "Magic Wand" 菜单中添加了一个新选项 - Data Bank。这是您在 SillyTavern 中管理可用于 RAG 的文档的中心。

## 关于 Documents

Data Bank 存储文件附件，也称为 documents。这些 documents 分为三个可用范围。

1. Global attachments - 在每个聊天中都可用，无论是单人还是群聊。
2. Character attachments - 仅对当前选择的角色可用，包括当他们在群聊中回复时。_Attachments 保存在本地，不会随 character card 导出！_
3. Chat attachments - 仅在当前打开的聊天中可用。聊天中的每个角色都可以从中提取。

!!!info 注意
虽然不正式属于 data bank 的一部分，您甚至可以将文件附加到单个消息。使用 "Wand" 菜单中的 Attach File 选项，或消息操作行中的回形针图标。
!!!

什么可以成为 document？实际上任何可以用纯文本形式表示的东西！

示例包括但不限于：

- 本地文件（书籍、科学论文等）
- 网页（Wikipedia、文章、新闻）
- 视频字幕

各种 extensions 和 plugins 也可以提供收集和处理数据的新方法，下面将详细介绍。

## Data Sources

要将 document 添加到任何范围，点击 "Add" 并选择可用来源之一。

### Notepad

从头开始创建文本文件，或编辑现有附件。

### File

从计算机硬盘上传文件。SillyTavern 为流行的文件格式提供内置转换器：

- PDF（仅文本）
- HTML
- Markdown
- ePUB
- TXT

您还可以附加任何非标准扩展名的文本文件，如 JSON、YAML、源代码等。如果所选文件类型没有已知的转换，并且文件无法解析为纯文本文档，则文件上传将被拒绝，这意味着不允许原始二进制文件。

!!!info 注意
导入 Microsoft Office（DOCX、PPTX、XLSX）和 LibreOffice 文档（ODT、ODP、ODS）需要安装并加载 [Server Plugin](https://github.com/SillyTavern/SillyTavern-Office-Parser)。有关安装说明，请参阅 plugin 的 README 页面。
!!!

### Web

通过 URL 从网页抓取文本。然后通过 [Readability](https://github.com/mozilla/readability) library 处理 HTML 文档以仅提取可用文本。

某些 web servers 可能会拒绝 fetch 请求、受 Cloudflare 保护或严重依赖 JavaScript 运行。如果您在任何特定网站上遇到问题，请通过 web browser 手动下载页面，并使用 file uploader 附加它。

### YouTube

通过 ID 或 URL 下载 YouTube 视频的字幕，可以是创作者上传的，也可以是 Google 自动生成的。某些视频可能禁用了字幕，也无法解析有年龄限制的视频，因为需要登录。

字幕以视频的默认语言加载。可选地，您可以指定两个字母的语言代码来尝试获取特定语言的字幕。此功能并非总是可用，可能会失败，因此请谨慎使用。

### Web Search

!!!info 注意
此来源需要安装并正确配置 [Web Search](/extensions/WebSearch.md) extension。有关更多详细信息，请参阅链接的页面。
!!!

执行 web search 并从搜索结果页面下载文本。这类似于 Web 来源，但完全自动化。所选的 search engine 将继承自 extension 设置，因此请提前设置。

首先，指定搜索查询、要访问的最大链接数以及输出类型：一个组合文件（根据 extension 规则格式化）或每个单独页面的单独文件。您可以选择保存页面片段。

### Fandom

!!!info 注意
此来源需要安装并加载 [Server Plugin](https://github.com/SillyTavern/SillyTavern-Fandom-Scraper)。有关安装说明，请参阅 plugin 的 README 页面。
!!!

通过 ID 或 URL 从 [Fandom](https://www.fandom.com/) wiki 抓取文章。由于某些 wikis 非常大，使用 filter regular expression 限制范围可能是有益的，它将针对文章标题进行测试。如果未提供 filter，则所有页面都将被导出。您可以将它们保存为每个页面的单独文件，或合并为单个文档。

### Bronie Parser Extension（第三方）

!!!warning 注意
此来源来自第三方，与 SillyTavern 团队**无关**。此来源需要您安装 Bronya Rand 的 [Bronie Parser Extension](https://github.com/Bronya-Rand/Bronie-Parser-Extension)，以及需要 parser 才能工作的 Server Plugins。
!!!

Bronya Rand 的 Bronie Parser Extension 允许使用第三方 scrapers，例如 miHoYo/HoYoverse 的 [HoYoLab](https://wiki.hoyolab.com) 进入 SillyTavern，类似于其他 data sources。

目前，Bronya Rand 的 Bronie Parser Extension 支持以下内容：

- miHoYo/HoYoverse 的 HoYoLab（用于 Genshin Impact/Honkai: Star Rail）通过 [HoYoWiki-Scraper-TS](https://github.com/Bronya-Rand/HoYoWiki-Scraper-TS)

首先，按照其[安装指南](https://github.com/Bronya-Rand/Bronie-Parser-Extension?tab=readme-ov-file#installation)安装 Bronya Rand 的 Bronie Parser Extension，并将支持的 Server Plugin 安装到 SillyTavern 中。重启 SillyTavern 并进入 _Data Bank_ 菜单。点击 `+ Add`，您应该会看到您最近安装的 scrapers 已添加到可能的来源列表中以获取信息。

## Vector Storage

所以，您已经为自己建立了一个关于特定主题的良好而全面的信息库。下一步是什么？

要将 documents 用于 RAG，您需要使用兼容的 extension，它将把相关数据插入到 LLM prompt 中。

Vector Storage 与 SillyTavern 捆绑在一起，是此类 extension 的参考实现。它使用 embeddings（也称为 vectors）来搜索与您正在进行的聊天相关的 documents。

!!!info 有趣的事实

1. Embeddings 是抽象表示一段文本的数字数组，由专门的语言模型生成。更相似的文本在其各自 vectors 之间的距离更短。
2. Vector Storage extension 使用 [Vectra](https://github.com/Stevenic/vectra) library 来跟踪文件 embeddings。它们存储在用户数据目录的 `/vectors` 文件夹中的 JSON 文件中。每个 document 内部由其自己的 index/collection 文件表示。
   !!!

由于 Vectors 功能默认禁用，您需要打开 extensions 面板（顶部栏上的 "Stacked Cubes" 图标），然后导航到 "Vector Storage" 部分，并勾选 "File vectorization settings" 下的 "Enabled for files" checkbox。

Vector Storage 本身不生成任何 vectors，您需要使用兼容的 embedding provider。

## Vector Providers

!!!warning 警告
Embeddings 仅在使用生成它们的相同模型检索时才可用。更改 embedding 模型或来源时，需要重新计算 vectors。
!!!

### Local

这些来源是免费和无限的，并使用您的 CPU/GPU 来计算 embeddings。

1. Local (Transformers) - 在 Node server 上运行。SillyTavern 将自动从 HuggingFace 下载 ONNX 格式的兼容模型。默认模型：[jina-embeddings-v2-base-en](https://huggingface.co/Cohee/jina-embeddings-v2-base-en)。
2. WebLLM - 需要安装 extension 和[支持 WebGPU](https://caniuse.com/webgpu) 的 web browser。直接在您的 browser 中运行，可以使用硬件加速。自动从 HuggingFace 下载支持的模型。从这里安装 extension：<https://github.com/SillyTavern/Extension-WebLLM>。
3. Ollama - 从 <https://ollama.com/> 获取。在 API connection 菜单中设置 API URL（在 Text Completion 下，默认：`http://localhost:11434`）。必须先下载兼容模型，然后在 extension 设置中设置其名称。示例模型：[mxbai-embed-large](https://ollama.com/library/mxbai-embed-large)。可选地，勾选一个选项以将模型保持加载在内存中。
4. llama.cpp server - 从 [ggerganov/llama.cpp](https://github.com/ggerganov/llama.cpp) 获取，并使用 `--embedding` flag 运行 server 可执行文件。从 HuggingFace 加载兼容的 GGUF embedding 模型，例如 [nomic-ai/nomic-embed-text-v1.5-GGUF](https://huggingface.co/nomic-ai/nomic-embed-text-v1.5-GGUF)。
5. vLLM - 从 [vllm-project/vllm](https://github.com/vllm-project/vllm) 获取。首先在 API connection 菜单中设置 API URL 和 API key。
6. Extras（已弃用）- 使用 SentenceTransformers loader 在 [Extras API](https://github.com/SillyTavern/SillyTavern-extras) 下运行。默认模型：[all-mpnet-base-v2](https://huggingface.co/sentence-transformers/all-mpnet-base-v2)。此来源未维护，将来最终会被删除。

### API sources

所有这些来源都需要相应服务的 API key，通常有使用成本，但通常计算 embeddings 相当便宜。

1. OpenAI
2. Cohere
3. Google AI Studio
4. Google Vertex AI
5. TogetherAI
6. MistralAI
7. NomicAI

## Vectorization Settings

选择 embedding provider 后，不要忘记配置其他设置，这些设置将定义处理和检索 documents 的规则。

!!!info 注意
拆分、vectorization 和从 attachments 检索信息需要一些时间。虽然文件的初始摄取可能需要一段时间，但 RAG 搜索查询通常足够快，不会产生明显的延迟。
!!!

### Message attachments

这些设置控制直接附加到消息的文件。

适用以下规则：

1. 只有适合 LLM context window 的消息才能检索其 attachments。
2. 当 vector storage extension 被禁用时，文件 attachments 及其附带的消息将完全插入到 prompt 中。
3. 当文件 vectorization 被启用时，文件将被拆分成块，只有最相关的部分将被插入，节省 context 空间并允许模型保持专注。

- Size threshold (KB) - 设置分块拆分阈值。只有大于指定大小的文件才会被拆分。
- Chunk size (chars) - 设置单个块的目标大小（以文本字符为单位，而不是模型 tokens！）。
- Chunk overlap (%) - 设置将在相邻块之间共享的块大小的百分比。这允许块之间更平滑的过渡，但也可能引入一些冗余。
- Retrieve chunks - 设置要检索的最相关文件块的最大数量。它们将按原始顺序插入。

### Data Bank files

这些设置控制 Data Bank documents 的处理方式。

适用以下规则：

1. 当文件 vectorization 被禁用时，不使用 Data Bank。
2. 否则，来自当前范围（见上文）的所有可用 documents 都会被考虑用于查询。只检索所有文件中最相关的块。同一文件的多个块按原始顺序插入。
3. 插入的块将在适配聊天消息之前保留部分 context。

- Size threshold (KB) - 设置分块拆分阈值。只有大于指定大小的文件才会被拆分。
- Chunk size (chars) - 设置单个块的目标大小（以文本字符为单位，而不是模型 tokens！）。
- Chunk overlap (%) - 设置将在相邻块之间共享的块大小的百分比。这允许块之间更平滑的过渡，但也可能引入一些冗余。
- Retrieve chunks - 设置要检索的文件块的最大数量。此限额在所有文件之间共享。
- Injection Template - 定义检索到的信息将如何插入到 prompt 中。您可以使用特殊的 \{\{text\}\} macro 来指定检索文本的位置，以及任何其他 macros。
- Injection Position - 设置插入 prompt injection 的位置。与 Author's Note 和 World Info 相同的规则适用。

### Shared settings

- Query messages - 将使用多少最新聊天消息来查询 document chunks。
- Score threshold - 调整以允许根据相关性分数剔除块的检索（0 - 完全不匹配，1 - 完美匹配）。较高的值允许更准确的检索，并防止完全随机的信息进入 context。合理的值在 0.2（更宽松）到 0.5（更集中）之间。
- Chunk boundary - 拆分文件为块时将优先考虑的自定义字符串。如果未指定，默认情况下按（按顺序）双换行符、单换行符和单词之间的空格拆分。
- Only chunk on custom boundary - 如果启用，分块将仅在指定的 chunk boundary 上进行。否则，分块也将在默认边界上进行。
- Translate files into English before processing - 如果启用，将使用 [Chat Translation](/extensions/Translation.md) extension 中配置的 translation API 在处理文件之前将文件翻译成英语。当使用仅支持英语文本的 embedding 模型时，这很有用。
- Include in World Info Scanning - 如果希望注入的内容激活 lore book entries，请勾选。
- Vectorize All - 强制为所有未处理的文件摄取 embeddings。
- Purge Vectors - 清除文件 embeddings，允许重新计算其 vectors。

!!!info 注意
有关 "Chat vectorization" 设置，请参阅 [Chat Vectorization](/extensions/Chat-vectorization.md)。
!!!

## 结论

恭喜！您的聊天体验现在通过 RAG 的力量得到增强。它的功能仅受您想象力的限制。一如既往，不要害怕尝试！