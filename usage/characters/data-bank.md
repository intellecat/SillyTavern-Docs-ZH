# 数据库（RAG）

检索增强生成（RAG）是一种为 LLM 提供外部知识来源的技术。它通过访问模型训练数据以外的信息，帮助提升 AI 回答的准确性。

SillyTavern 提供了一套工具，可从多种来源构建多用途知识库，并将收集到的数据用于 LLM 提示词中。

## 访问数据库

内置的聊天附件扩展（1.12.0 及更高版本默认包含）在"魔法棒"菜单中新增了一个选项——数据库。这是您管理 SillyTavern 中 RAG 所用文档的中心。

## 关于文档

数据库存储文件附件，也称为文档。文档分为三个可用范围。

1. 全局附件——在每个聊天中均可用，无论是单人聊天还是群组聊天。
2. 角色附件——仅对当前选定的角色可用，包括该角色在群组中发言时。_附件保存在本地，不会随角色卡导出！_
3. 聊天附件——仅在当前打开的聊天中可用。聊天中的每个角色均可从中获取信息。

!!!info 注意
虽然不是数据库的正式组成部分，但您也可以将文件附加到单条消息上。可使用"魔法棒"菜单中的"附加文件"选项，或消息操作栏中的回形针图标。
!!!

什么可以成为文档？几乎任何可以用纯文本形式表示的内容！

示例包括但不限于：

- 本地文件（书籍、科学论文等）
- 网页（维基百科、文章、新闻）
- 视频转录文本

各种扩展和插件也可以提供收集和处理数据的新途径，详见下文。

## 数据来源

要向任意范围添加文档，点击"添加"并选择一个可用来源。

### 记事本

从头创建文本文件，或编辑现有附件。

### 文件

从您计算机的硬盘上传文件。SillyTavern 为常见文件格式提供内置转换器：

- PDF（仅文本）
- HTML
- Markdown
- ePUB
- TXT

您也可以附加带有非标准扩展名的文本文件，如 JSON、YAML、源代码等。如果所选文件类型没有已知转换方式，且文件无法解析为纯文本文档，则文件上传将被拒绝，即原始二进制文件不被允许。

!!!info 注意
导入 Microsoft Office（DOCX、PPTX、XLSX）和 LibreOffice 文档（ODT、ODP、ODS）需要安装并加载[服务器插件](https://github.com/SillyTavern/SillyTavern-Office-Parser)。请参阅插件的 README 页面了解安装说明。
!!!

### 网页

通过 URL 从网页抓取文本。HTML 文档将通过 [Readability](https://github.com/mozilla/readability) 库处理，以提取可用文本。

某些 Web 服务器可能会拒绝抓取请求、受 Cloudflare 保护，或严重依赖 JavaScript 才能正常运行。如果您在抓取某个特定网站时遇到问题，请通过浏览器手动下载该页面，然后使用文件上传器附加。

### YouTube

通过 ID 或 URL 下载 YouTube 视频的转录文本，可以是创作者上传的或由 Google 自动生成的。某些视频可能已禁用转录功能；此外，由于需要登录，年龄限制视频无法解析。

脚本以视频的默认语言加载。您也可以选择指定两个字母的语言代码，尝试获取特定语言的转录。此功能并非始终可用，可能会失败，请谨慎使用。

### 网页搜索

!!!info 注意
此来源需要安装并正确配置[网页搜索](/extensions/WebSearch.md)扩展。请参阅链接页面了解更多详情。
!!!

执行网页搜索并从搜索结果页面下载文本。与网页来源类似，但完全自动化。所选搜索引擎将继承自扩展设置，请提前配置好。

首先，指定搜索查询、要访问的最大链接数，以及输出类型：一个合并文件（按扩展规则格式化）或每个页面一个单独文件。您也可以选择保存页面摘要。

### Fandom

!!!info 注意
此来源需要安装并加载[服务器插件](https://github.com/SillyTavern/SillyTavern-Fandom-Scraper)。请参阅插件的 README 页面了解安装说明。
!!!

通过 ID 或 URL 从 [Fandom](https://www.fandom.com/) wiki 抓取文章。由于部分 wiki 内容量庞大，建议使用过滤正则表达式限制抓取范围，该表达式将与文章标题进行匹配。若未提供过滤器，则所有页面均会被导出。您可以将内容保存为每个页面的单独文件，或合并为一个文档。

### Bronie 解析器扩展（第三方）

!!!warning 注意
此来源来自第三方，与 SillyTavern 团队**无关**。使用此来源需要您安装 Bronya Rand 的 [Bronie 解析器扩展](https://github.com/Bronya-Rand/Bronie-Parser-Extension)，以及需要该解析器的服务器插件。
!!!

Bronya Rand 的 Bronie 解析器扩展允许使用第三方抓取器（如米哈游/HoYoverse 的 [HoYoLab](https://wiki.hoyolab.com)）接入 SillyTavern，与其他数据来源用法类似。

目前，Bronya Rand 的 Bronie 解析器扩展支持以下内容：

- 米哈游/HoYoverse 的 HoYoLab（适用于原神/崩坏：星穹铁道），通过 [HoYoWiki-Scraper-TS](https://github.com/Bronya-Rand/HoYoWiki-Scraper-TS)

首先，按照其[安装指南](https://github.com/Bronya-Rand/Bronie-Parser-Extension?tab=readme-ov-file#installation)安装 Bronya Rand 的 Bronie 解析器扩展，并在 SillyTavern 中安装受支持的服务器插件。重启 SillyTavern 后进入_数据库_菜单，点击 `+ 添加`，您应该能看到最近安装的抓取器已添加到可用来源列表中。

## 向量存储 {#vector-storage}

好的，您已经为特定主题建立了一个不错且全面的信息库。接下来呢？

要将文档用于 RAG，您需要使用兼容的扩展，由其将相关数据插入到 LLM 提示词中。

向量存储（Vector Storage）与 SillyTavern 捆绑提供，是此类扩展的参考实现。它使用嵌入（也称为向量）来搜索与当前聊天相关的文档。

!!!info 有趣的事实

1. 嵌入是由专门的语言模型生成的数字数组，抽象地表示一段文本。文本越相似，其对应向量之间的距离就越短。
2. 向量存储扩展使用 [Vectra](https://github.com/Stevenic/vectra) 库来跟踪文件嵌入。它们以 JSON 文件的形式存储在用户数据目录的 `/vectors` 文件夹中。每个文档在内部由其独立的索引/集合文件表示。
   !!!

由于向量功能默认禁用，您需要打开扩展面板（顶部栏上的"堆叠立方体"图标），导航到"向量存储"部分，并在"文件向量化设置"下勾选"为文件启用"复选框。

向量存储本身不生成任何向量，您需要使用兼容的嵌入提供者。

## 向量提供者

!!!warning 警告
嵌入只有在使用生成它们的同一模型检索时才可用。更换嵌入模型或来源时，需要重新计算向量。
!!!

### 本地来源

这些来源免费且不限量，使用您的 CPU/GPU 计算嵌入。

1. 本地（Transformers）——运行在 Node 服务器上。SillyTavern 将自动从 HuggingFace 下载 ONNX 格式的兼容模型。默认模型：[jina-embeddings-v2-base-en](https://huggingface.co/Cohee/jina-embeddings-v2-base-en)。
2. WebLLM——需要安装扩展，且浏览器需[支持 WebGPU](https://caniuse.com/webgpu)。直接在浏览器中运行，可利用硬件加速。自动从 HuggingFace 下载受支持的模型。扩展安装地址：<https://github.com/SillyTavern/Extension-WebLLM>。
3. Ollama——从 <https://ollama.com/> 获取。在 API 连接菜单中设置 API URL（位于文本补全下，默认：`http://localhost:11434`）。需先下载兼容模型，然后在扩展设置中填写其名称。示例模型：[mxbai-embed-large](https://ollama.com/library/mxbai-embed-large)。可选择勾选"保持模型常驻内存"选项。
4. llama.cpp 服务器——从 [ggerganov/llama.cpp](https://github.com/ggerganov/llama.cpp) 获取，并使用 `--embedding` 标志运行服务器可执行文件。从 HuggingFace 加载兼容的 GGUF 嵌入模型，例如 [nomic-ai/nomic-embed-text-v1.5-GGUF](https://huggingface.co/nomic-ai/nomic-embed-text-v1.5-GGUF)。
5. vLLM——从 [vllm-project/vllm](https://github.com/vllm-project/vllm) 获取。需先在 API 连接菜单中设置 API URL 和 API 密钥。
6. Extras（已弃用）——使用 SentenceTransformers 加载器运行在 [Extras API](https://github.com/SillyTavern/SillyTavern-extras) 下。默认模型：[all-mpnet-base-v2](https://huggingface.co/sentence-transformers/all-mpnet-base-v2)。该来源不再维护，未来将被移除。

### API 来源

以下所有来源均需要相应服务的 API 密钥，通常有使用费用，但计算嵌入的成本一般较低。

1. OpenAI
2. Cohere
3. Google AI Studio
4. Google Vertex AI
5. TogetherAI
6. MistralAI
7. NomicAI
8. OpenRouter
9. Electron Hub
10. Chutes
11. NanoGPT
12. SiliconFlow
13. Cloudflare Workers AI

## 向量化设置

选择嵌入提供者后，请务必配置其他设置，这些设置将定义处理和检索文档的规则。

!!!info 注意
附件的分割、向量化和信息检索需要一定时间。虽然文件的初始摄取可能耗时较长，但 RAG 搜索查询通常足够快速，不会造成明显延迟。
!!!

### 消息附件

这些设置控制直接附加到消息的文件。

适用以下规则：

1. 只有适合 LLM 上下文窗口的消息，其附件才能被检索。
2. 当向量存储扩展被禁用时，文件附件及其附带的消息会被完整插入到提示词中。
3. 当启用文件向量化时，文件将被分割为多个块，只有最相关的部分会被插入，从而节省上下文空间并帮助模型保持专注。

- 大小阈值（KB）——设置分块分割阈值。只有大于指定大小的文件才会被分割。
- 块大小（字符）——设置单个块的目标大小（以文本字符计，而非模型 token！）。
- 块重叠（%）——设置相邻块之间共享的块大小百分比。这有助于块之间的平滑过渡，但也可能引入一些冗余。
- 检索块数——设置要检索的最相关文件块的最大数量。这些块将按原始顺序插入。

### 数据库文件

这些设置控制数据库文档的处理方式。

适用以下规则：

1. 当文件向量化被禁用时，数据库不会被使用。
2. 否则，当前范围内的所有可用文档（见上文）均会被纳入查询考量。只有所有文件中最相关的块会被检索。同一文件的多个块按原始顺序插入。
3. 插入的块会在适配聊天消息之前预留一部分上下文空间。

- 大小阈值（KB）——设置分块分割阈值。只有大于指定大小的文件才会被分割。
- 块大小（字符）——设置单个块的目标大小（以文本字符计，而非模型 token！）。
- 块重叠（%）——设置相邻块之间共享的块大小百分比。这有助于块之间的平滑过渡，但也可能引入一些冗余。
- 检索块数——设置要检索的文件块的最大数量。该限额在所有文件之间共享。
- 注入模板——定义检索到的信息插入提示词的方式。可使用特殊的 \{\{text\}\} 宏指定检索文本的插入位置，以及其他任意宏。
- 注入位置——设置提示词注入的插入位置。适用与作者注释和世界信息相同的规则。

### 共享设置

- 查询消息数——用于查询文档块的最新聊天消息数量。
- 分数阈值——调整此项可根据块的相关性分数（0 = 完全不匹配，1 = 完全匹配）过滤检索结果。较高的值允许更精准的检索，防止完全不相关的信息进入上下文。合理的取值范围在 0.2（较宽松）到 0.5（较精准）之间。
- 块边界——自定义字符串，分割文件时优先在此处断开。若未指定，默认依次按双换行符、单换行符和单词间空格分割。
- 仅在自定义边界处分块——启用后，分块仅在指定的块边界处进行。否则，也会在默认边界处进行分块。
- 处理前将文件翻译为英文——启用后，将使用[聊天翻译](/extensions/Translation.md)扩展中配置的翻译 API，在处理前将文件翻译为英文。在使用仅支持英文的嵌入模型时很有用。
- 包含在世界信息扫描中——如果希望注入的内容触发知识库条目，请勾选此项。
- 向量化全部——强制对所有未处理文件摄取嵌入。
- 清除向量——清除文件嵌入，以便重新计算其向量。

!!!info 注意
有关"聊天向量化"设置，请参阅[聊天向量化](/extensions/Chat-vectorization.md)。
!!!

## 结语

恭喜！您的聊天体验现已借助 RAG 的强大能力得到提升。其可能性仅受您想象力的限制。大胆尝试，勇于探索！
