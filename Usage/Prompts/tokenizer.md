---
order: 70
route: /usage/prompts/tokenizer/
---

# 分词器

分词器是一种将文本拆分为更小单元（称为 token）的工具。这些 token 可以是单个词，也可以是词的一部分，例如前缀、后缀或标点符号。一个经验法则是：一个 token 通常对应约 3~4 个字符的文本。

SillyTavern 提供「最佳匹配」选项，根据所用 API 提供商，按以下规则自动匹配分词器。

文本补全 API **（可覆盖）**：

1. NovelAI Clio：NerdStash 分词器。
2. NovelAI Kayra：NerdStash v2 分词器。
3. 文本补全：API 分词器（如支持）或 Llama 分词器。
4. KoboldAI Classic / AI Horde：Llama 分词器。
5. KoboldCpp：模型 API 分词器。

如果结果不够准确，或希望进行实验，可以为 SillyTavern 在向 AI 后端构建请求时设置一个_覆盖分词器_：

1. 无。每个 token 估算为约 3.3 个字符，向上取整至最近整数。**如果提示词在高上下文长度时被截断，请尝试此选项。** KoboldAI Lite 采用此方式。
2. Llama 分词器。适用于 Llama 1/2 模型系列：Vicuna、Hermes、Airoboros 等。**使用 Llama 1/2 模型时请选择此项。**
3. Llama 3 分词器。适用于 Llama 3/3.1 模型。**使用 Llama 3/3.1 模型时请选择此项。**
4. NerdStash 分词器。适用于 NovelAI 的 Clio 模型。**使用 Clio 模型时请选择此项。**
5. NerdStash v2 分词器。适用于 NovelAI 的 Kayra 模型。**使用 Kayra 模型时请选择此项。**
6. Mistral V1 分词器。适用于较旧的 Mistral 模型系列及其微调版本。**使用较旧 Mistral 模型时请选择此项。**
7. Mistral Nemo 分词器。适用于 Mistral Nemo 模型系列及其微调版本。**使用 Mistral Nemo/Pixtral 模型时请选择此项。**
8. Yi 分词器。适用于 Yi 模型。**使用 Yi 模型时请选择此项。**
9. Gemma 分词器。适用于 Gemini/Gemma 模型。**使用 Gemma 模型时请选择此项。**
10. DeepSeek 分词器。适用于 DeepSeek 模型（如 R1）。**使用 DeepSeek 模型时请选择此项。**
11. API 分词器。直接向生成 API 查询以从模型获取 token 数量。已知支持的后端：Text Generation WebUI (ooba)、koboldcpp、TabbyAPI、Aphrodite API。**使用受支持后端时请选择此项。**

聊天补全 API **（不可覆盖）**：

1. OpenAI：通过 [tiktoken](https://github.com/openai/tiktoken) 使用模型对应的分词器。
2. Claude：通过 [WebTokenizers](https://github.com/mlc-ai/tokenizers-cpp) 使用模型对应的分词器。
3. OpenRouter：对应模型使用 Llama、Mistral、Gemma、Yi 分词器。
4. Google AI Studio：Gemma 分词器。
5. AI21 API：Jamba 分词器（需要一次性下载）。
6. Cohere API：Command-R 或 Command-A 分词器（需要一次性下载）。
7. MistralAI API：Mistral V1 或 V3 分词器（需要一次性下载）。
8. DeepSeek API：DeepSeek 分词器（需要一次性下载）。
9. 兜底分词器：GPT-3.5 turbo 分词器。

#### 附加分词器

以下分词器由于文件体积较大，默认安装中不包含，首次使用时需要进行一次性下载。

1. Qwen2 分词器。
2. Command-R / Command-A 分词器。适用于聊天补全中的 Cohere 源。
3. Mistral V3（Nemo）分词器。适用于聊天补全中的 MistralAI 源（Nemo 和 Pixtral 模型）。
4. DeepSeek（deepseek-chat）分词器。适用于聊天补全中的 DeepSeek 源。

如果不希望从互联网下载，可以在 config.yaml 中设置退出选项：`enableDownloadableTokenizers`，将其设为 `false` 即可禁用下载。

也可以从 [SillyTavern-Tokenizers](https://github.com/SillyTavern/SillyTavern-Tokenizers) 仓库手动下载分词器。下载 JSON 文件并放置在数据根目录的 `_cache` 子目录中，默认路径为 `./data/_cache`。若 `_cache` 目录不存在，请先创建。之后重启 SillyTavern 服务器以重新初始化分词器。

如果所需分词器模型未缓存且下载已禁用，将使用兜底分词器（Llama 3）进行计数。

### Token 填充

!!! 适用于：文本补全 API
SillyTavern 会始终为聊天补全模型使用匹配的分词器，因此无需 token 填充。
!!!

除非 SillyTavern 使用由运行模型的远程后端 API 提供的分词器，否则提示词生成过程中的所有 token 数量估算均基于所选[分词器](#分词器)类型。

由于在接近模型定义的最大上下文大小时，分词结果可能不够精确，提示词的某些部分可能会被裁剪或丢弃，从而对角色定义的连贯性产生负面影响。

为防止这种情况，SillyTavern 会将上下文大小的一部分预留为填充空间，以避免加入超过模型容纳能力的聊天内容。如果即使选择了最匹配的分词器，提示词的某些部分仍被截断，请调整填充量以确保描述不被截断。

可以输入负值进行反向填充，即允许分配超过设定上限的 token 数量。
