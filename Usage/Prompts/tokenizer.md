---
order: 70
route: /usage/prompts/tokenizer/
---

# 分词器

Tokenizer 是将一段文本分解为称为 token 的较小单元的工具。这些 token 可以是单个单词,甚至是单词的一部分,如前缀、后缀或标点符号。一个经验法则是,一个 token 通常对应于 3~4 个文本字符。

SillyTavern 提供了一个 "Best match" 选项,尝试根据使用的 API 提供商使用以下规则匹配 tokenizer。

Text Completion APIs **(可覆盖)**:

1. NovelAI Clio: NerdStash tokenizer。
2. NovelAI Kayra: NerdStash v2 tokenizer。
3. Text Completion: API tokenizer(如果支持)或 Llama tokenizer。
4. KoboldAI Classic / AI Horde: Llama tokenizer。
5. KoboldCpp: model API tokenizer。

如果你得到不准确的结果或希望尝试,可以为 SillyTavern 设置一个 _override tokenizer_ 以在向 AI backend 形成请求时使用:

1. None。每个 token 估计为约 3.3 个字符,向上舍入到最接近的整数。**如果你的 prompt 在高上下文长度上被截断,请尝试此选项。** 这种方法被 KoboldAI Lite 使用。
2. Llama tokenizer。由 Llama 1/2 模型系列使用:Vicuna、Hermes、Airoboros 等。**如果你使用 Llama 1/2 模型,请选择此选项。**
3. Llama 3 tokenizer。由 Llama 3/3.1 模型使用。**如果你使用 Llama 3/3.1 模型,请选择此选项。**
4. NerdStash tokenizer。由 NovelAI 的 Clio 模型使用。**如果你使用 Clio 模型,请选择此选项。**
5. NerdStash v2 tokenizer。由 NovelAI 的 Kayra 模型使用。**如果你使用 Kayra 模型,请选择此选项。**
6. Mistral V1 tokenizer。由较旧的 Mistral 模型系列及其微调版本使用。**如果你使用较旧的 Mistral 模型,请选择此选项。**
7. Mistral Nemo tokenizer。由 Mistral Nemo 模型系列及其微调版本使用。**如果你使用 Mistral Nemo/Pixtral 模型,请选择此选项。**
8. Yi tokenizer。由 Yi 模型使用。**如果你使用 Yi 模型,请选择此选项。**
9. Gemma tokenizer。由 Gemini/Gemma 模型使用。**如果你使用 Gemma 模型,请选择此选项。**
10. DeepSeek tokenizer。由 DeepSeek 模型(如 R1)使用。**如果你使用 DeepSeek 模型,请选择此选项。**
11. API tokenizer。查询生成 API 以直接从模型获取 token 计数。已知支持的 backend:Text Generation WebUI (ooba)、koboldcpp、TabbyAPI、Aphrodite API。**如果你使用支持的 backend,请选择此选项。**

Chat Completion APIs **(不可覆盖)**:

1. OpenAI: 通过 [tiktoken](https://github.com/openai/tiktoken) 的模型相关 tokenizer。
2. Claude: 通过 [WebTokenizers](https://github.com/mlc-ai/tokenizers-cpp) 的模型相关 tokenizer。
3. OpenRouter: 为各自模型提供的 Llama、Mistral、Gemma、Yi tokenizer。
4. Google AI Studio: Gemma tokenizer。
5. AI21 API: Jamba tokenizer(需要一次性下载)。
6. Cohere API: Command-R 或 Command-A tokenizer(需要一次性下载)。
7. MistralAI API: Mistral V1 或 V3 tokenizer(需要一次性下载)。
8. DeepSeek API: DeepSeek tokenizer(需要一次性下载)。
9. Fallback tokenizer: GPT-3.5 turbo tokenizer。

#### Additional Tokenizers

由于体积原因,这些 tokenizer 不包含在默认安装中。首次使用时需要一次性下载。

1. Qwen2 tokenizer。
2. Command-R / Command-A tokenizer。在 Chat Completion 中由 Cohere 源使用。
3. Mistral V3 (Nemo) tokenizer。在 Chat Completion 中由 MistralAI 源使用(Nemo 和 Pixtral 模型)。
4. DeepSeek (deepseek-chat) tokenizer。在 Chat Completion 中由 DeepSeek 源使用。

如果你不想使用互联网下载,config.yaml 中存在选择退出选项:`enableDownloadableTokenizers`。设置为 `false` 以禁用下载。

你还可以从 [SillyTavern-Tokenizers](https://github.com/SillyTavern/SillyTavern-Tokenizers) 仓库手动下载 tokenizer。下载 JSON 文件并将它们放在数据根目录的 `_cache` 子目录中,默认路径为 `./data/_cache`。如果 `_cache` 目录不存在,请创建它。之后,重启 SillyTavern 服务器以重新初始化 tokenizer。

如果所需的 tokenizer 模型未缓存且下载被禁用,将使用后备 tokenizer(Llama 3)进行计数。

### Token Padding

!!! Applies to: Text Completion APIs
SillyTavern 将始终为 Chat Completion 模型使用匹配的 tokenizer,因此不需要 token padding。
!!!

除非 SillyTavern 使用运行模型的远程 backend API 提供的 tokenizer,否则在 prompt 生成期间假设的所有 token 计数都是基于所选 [tokenizer](#tokenizer) 类型的估计值。

由于 tokenization 的结果在接近模型定义的最大上下文大小时可能不准确,prompt 的某些部分可能会被修剪或删除,这可能会对角色定义的连贯性产生负面影响。

为了防止这种情况,SillyTavern 将上下文大小的一部分分配为 padding,以避免添加超过模型可容纳的聊天项目。如果你发现即使选择了最匹配的 tokenizer,prompt 的某些部分仍被修剪,请调整 padding,以便不截断描述。

你可以输入负值以进行反向 padding,这允许分配超过设置的最大 token 数量。
