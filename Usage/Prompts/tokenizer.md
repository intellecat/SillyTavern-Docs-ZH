---
order: prompts-40
---

# 分词器

分词器是一个将文本分解成更小单位（称为 token）的工具。这些 token 可以是单个词，甚至是词的部分，如前缀、后缀或标点符号。一个经验法则是，一个 token 通常对应 3~4 个字符的文本。

SillyTavern 提供了一个"最佳匹配"选项，根据使用的 API 提供商使用以下规则尝试匹配分词器。

Text Completion API **（可覆盖）**：

1. NovelAI Clio：NerdStash 分词器。
2. NovelAI Kayra：NerdStash v2 分词器。
3. Text Completion：API 分词器（如果支持）或 Llama 分词器。
4. KoboldAI Classic / AI Horde：Llama 分词器。
5. KoboldCpp：模型 API 分词器。

如果您得到不准确的结果或想要实验，您可以设置一个_覆盖分词器_供 SillyTavern 在向 AI 后端发送请求时使用：

1. 无。每个 token 估计为 ~3.3 个字符，向上取整到最接近的整数。**如果您的提示词在高上下文长度时被截断，请尝试这个。** 这种方法被 KoboldAI Lite 使用。
2. Llama 分词器。由 Llama 1/2 模型家族使用：Vicuna、Hermes、Airoboros 等。**如果您使用 Llama 1/2 模型，请选择这个。**
3. Llama 3 分词器。由 Llama 3/3.1 模型使用。**如果您使用 Llama 3/3.1 模型，请选择这个。**
4. NerdStash 分词器。由 NovelAI 的 Clio 模型使用。**如果您使用 Clio 模型，请选择这个。**
5. NerdStash v2 分词器。由 NovelAI 的 Kayra 模型使用。**如果您使用 Kayra 模型，请选择这个。**
6. Mistral V1 分词器。由较旧的 Mistral 模型家族及其微调版本使用。**如果您使用较旧的 Mistral 模型，请选择这个。**
7. Mistral Nemo 分词器。由 Mistral Nemo 模型家族及其微调版本使用。**如果您使用 Mistral Nemo/Pixtral 模型，请选择这个。**
8. Yi 分词器。由 Yi 模型使用。**如果您使用 Yi 模型，请选择这个。**
9. Gemma 分词器。由 Gemini/Gemma 模型使用。**如果您使用 Gemma 模型，请选择这个。**
10. DeepSeek 分词器。由 DeepSeek 模型（如 R1）使用。**如果您使用 DeepSeek 模型，请选择这个。**
11. API 分词器。直接从模型查询生成 API 以获取 token 计数。已知支持的后端：Text Generation WebUI (ooba)、koboldcpp、TabbyAPI、Aphrodite API。**如果您使用支持的后端，请选择这个。**

Chat Completion API **（不可覆盖）**：

1. OpenAI：通过 [tiktoken](https://github.com/openai/tiktoken) 的模型相关分词器。
2. Claude：通过 [WebTokenizers](https://github.com/mlc-ai/tokenizers-cpp) 的模型相关分词器。
3. OpenRouter：对应模型的 Llama、Mistral、Gemma、Yi 分词器。
4. Google AI Studio：Gemma 分词器。
5. Scale API：GPT-4 分词器。
6. AI21 API：Jamba 分词器（需要一次性下载）。
7. Cohere API：Command-R 分词器（需要一次性下载）。
8. MistralAI API：Mistral V1 或 V3 分词器（需要一次性下载）。
9. DeepSeek API：DeepSeek 分词器（需要一次性下载）。
10. 后备分词器：GPT-3.5 turbo 分词器。

#### 额外分词器

由于大小原因，这些分词器不包含在默认安装中。首次使用时需要一次性下载。

1. Qwen2 分词器。
2. Command-R 分词器。由 Chat Completion 中的 Cohere 源使用。
3. Mistral V3 (Nemo) 分词器。由 Chat Completion 中的 MistralAI 源使用（Nemo 和 Pixtral 模型）。
4. DeepSeek (deepseek-chat) 分词器。由 Chat Completion 中的 DeepSeek 源使用。

如果您不想使用互联网下载，可以在 config.yaml 中选择退出：`enableDownloadableTokenizers`。设置为 `false` 以禁用下载。

您也可以从 [SillyTavern-Tokenizers](https://github.com/SillyTavern/SillyTavern-Tokenizers) 仓库手动下载分词器。下载 JSON 文件并将它们放在数据根目录的 `_cache` 子目录中，默认路径是 `./data/_cache`。如果 `_cache` 目录不存在，请创建它。之后，重启 SillyTavern 服务器以重新初始化分词器。

如果所需的分词器模型未缓存且下载被禁用，将使用后备分词器（Llama 3）进行计数。

### Token 填充

!!! 适用于：Text Completion API
SillyTavern 将始终为 Chat Completion 模型使用匹配的分词器，因此不需要 token 填充。
!!!

除非 SillyTavern 使用由运行模型的远程后端 API 提供的分词器，否则在提示词生成期间假定的所有 token 计数都是基于所选[分词器](#分词器)类型的估计。

由于在接近模型定义的最大值的上下文大小上，分词结果可能不准确，提示词的某些部分可能会被修剪或删除，这可能会对角色定义的连贯性产生负面影响。

为了防止这种情况，SillyTavern 将上下文大小的一部分分配为填充，以避免添加超过模型容纳能力的聊天项。如果您发现即使选择了最匹配的分词器，提示词的某些部分仍被截断，请调整填充以确保描述不被截断。

您可以输入负值进行反向填充，这允许分配超过设定的最大 token 数量。
