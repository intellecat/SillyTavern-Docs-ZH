# API 连接

SillyTavern 可连接多种大语言模型 API。
以下是对各类 API 的优缺点与适用场景的介绍。

## 通俗解释：聊天补全 vs 文本补全

首次进入 ST 的「API 连接」页面时，你会看到一个下拉选项，可在「聊天补全」和「文本补全」等选项之间切换。理解这两者的区别很有帮助。

常见误解：很多人会认为「文本补全」对应本地模型、「聊天补全」对应云端 LLM，但实际上并非如此。同样地，「NovelAI」或「Kobold」也不是完全独立的模型类型，尽管它们在 ST 的 API 下拉菜单中是单独的选项。通过适当的后端配置，可以强制模型使用不同的 API 结构，但这不是本节的重点。

当你用 ST 发送消息时，聊天内容、角色描述以及知识书、作者注释等其他提示词会被组合成一个「提示词」发送给模型。你所使用的模型的 API「类型」决定了这个提示词的具体构建方式（ST 会在后台自动处理——你可以打开 ST 终端查看发送给 AI 的完整提示词内容）。

### 聊天补全

顾名思义，聊天补全模型会将提示词构建为用户（你）与助手（AI）或系统（中立）之间的一系列消息。针对聊天补全训练的模型有助于营造「聊天」的感觉，让 AI「回应」最后一条消息。当你使用 ChatGPT 网站时，背后用的就是聊天补全 API。

### 文本补全（又称「补全」）

文本补全则是将提示词转换为一个长字符串，模型只是尝试续写这段文字（可以想象成：你的所有文字、数百条消息、所有格式和换行符等，全部被压缩成一个超长的句子）。

如果你在 ST 中的消息恰好以「YourPersona:」和「Character:」的格式交替排列，文本补全模型会尝试延续这一模式，ST 会将其渲染为新的聊天消息——但实际上模型只是在续写文本。比如输入「太阳从」，文本补全模型很可能续写为「东方升起」。

大多数文本补全模型都有推荐的「Instruct 模板」（通常在模型文档或下载页面中注明），帮助模型像聊天补全模型那样「回应」消息和指令。ST 通常在「高级格式设置」页面中提供了大多数（甚至全部）的 Instruct 模板供选择。

## 本地 API

- 这些 LLM API 可以在你的电脑上运行。
- 免费使用，无内容过滤。
- 安装过程可能较为复杂（**SillyTavern 开发团队不提供此方面的支持**）。
- 需要从 [HuggingFace](https://huggingface.co/models?other=LLM) 单独下载 LLM 模型，每个模型大小为 5~50GB 不等。
- 大多数模型的能力不及云端 LLM API。

### KoboldCpp

- 易用的 API，支持 CPU 卸载（对显存不足的用户很有帮助）和流式传输
- 在 Windows、Mac 和 Linux 上可以从单个二进制文件运行
- 支持 GGUF 模型
- 速度慢于 AutoGPTQ、Exllama/v2 等纯 GPU 加载器
- [GitHub](https://github.com/LostRuins/koboldcpp)，[配置说明](/Usage/API_Connections/koboldcpp.md)

### llama.cpp

- KoboldCpp 和 Ollama 的原始上游项目
- 提供预编译二进制文件及从源码编译的选项
- 支持 GGUF 模型
- 轻量级 CLI 接口 llama-server
- [GitHub](https://github.com/ggml-org/llama.cpp)

### Ollama

- 所有基于 llama.cpp 的 API 中最易配置和使用的
- 提供实用的模型[目录](https://ollama.com/library)，支持一键下载
- 支持以 Ollama 专有格式封装的 GGUF 模型
- [GitHub](https://github.com/ollama/ollama)，[官网](https://ollama.com/)

### Oobabooga TextGeneration WebUI

- 带流式传输的一体化 Gradio UI
- 对量化模型（AWQ、Exl2、GGML、GGUF、GPTQ）和 FP16 模型的支持最为全面
- 提供一键安装程序
- 定期更新，偶尔可能与 SillyTavern 存在兼容性问题
- [GitHub](https://github.com/oobabooga/text-generation-webui#one-click-installers)

**将 SillyTavern 连接到 Ooba 新版 OpenAI API 的正确方式：**

1. 确保使用的是 2023 年 11 月 14 日之后的 Oobabooga TextGen 最新版本。
2. 编辑 CMD_FLAGS.txt 文件，添加 `--api` 标志，然后重启 Ooba 服务器。
3. 将 ST 连接到 `http://localhost:5000/`（默认地址），无需勾选「Legacy API」选项。可删除 Ooba 控制台提供的 URL 中的 `/v1` 后缀。

*可以使用 `--api-port 5001` 标志修改 API 监听端口，其中 5001 为自定义端口号。*

### TabbyAPI

- 基于 [Exllamav2](https://github.com/turboderp/exllamav2) 的轻量级 API，支持流式传输
- 支持 Exl2、GPTQ 和 FP16 模型
- [官方扩展](https://github.com/theroyallab/ST-tabbyAPI-loader)支持直接从 SillyTavern 加载/卸载模型
- 不推荐显存不足的用户使用（无 CPU 卸载）
- [GitHub](https://github.com/theroyallab/tabbyAPI)，[配置说明](/Usage/API_Connections/tabbyapi.md)

### KoboldAI Classic（已弃用，已停止维护）

- 在本地运行，完全私密，可用模型种类丰富
- 对 AI 生成参数的控制最为直接
- 需要 GPU 提供大量显存（6~24GB，具体取决于 LLM 模型）
- 模型上下文限制为 2k
- 不支持流式传输
- 常见 KoboldAI 版本：
  - [Henky's United](https://github.com/henky717/KoboldAI)
  - [0cc4m's 4bit-supporting United](https://github.com/0cc4m/KoboldAI)

## 云端 LLM API

- 这些 LLM API 以云服务方式运行，无需占用本地资源
- 能力通常强于大多数本地 LLM
- 但都有不同程度的内容过滤，大多数需要付费

### AI Horde

- SillyTavern 开箱即用，无需额外配置即可访问
- 使用个人志愿者（Horde Worker）的 GPU 处理聊天请求
- 生成等待时间、AI 参数和可用模型均取决于 Worker 的配置
- [官网](https://aihorde.net/)，[配置说明](./horde.md)

### OpenAI (ChatGPT)

- 易于配置，API 密钥获取方便
- 需要预充值积分，按提示词计费
- 逻辑性强，但创意写作风格可能较为刻板和可预测
- 较新的模型（gpt-4-turbo、gpt-4o）支持多模态
- [官网](https://platform.openai.com/)，[配置说明](/Usage/API_Connections/openai.md#openai)

### Claude（Anthropic 出品）

- 推荐给希望 AI 拥有创意、独特写作风格的用户
- 需要预充值积分，按提示词计费
- 最新模型（Claude 3）支持多模态
- 需要特定的提示词风格，并善用 [prefill](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/prefill-claudes-response) 引导回复
- [官网](https://console.anthropic.com/)，[配置说明](/Usage/API_Connections/openai.md#claude)

### Google AI Studio 和 Vertex AI

- 提供带速率限制的免费层（Gemini Flash），可能需要绑定付款信息
- [AI Studio](https://aistudio.google.com/) 通常具有最新的模型和功能
- [Vertex AI](https://console.cloud.google.com/vertex-ai/studio) 配置较复杂，但更稳定
- [配置说明](/Usage/API_Connections/google.md)

### Mistral（Mistral AI 出品）

- 提供各种规模和用途的高效模型，可在[其平台](https://console.mistral.ai/api-keys/)创建账号和 API 密钥。
- 通用用途上下文长度为 32k~128k，代码用途为 32k~256k。
- 提供有速率限制的免费层。
- 内容审核较宽松，Mistral 的核心理念是保持中立、赋能用户，详情见[此处](https://mistral.ai/terms/)。
- [官网](https://console.mistral.ai/)，[配置说明](/Usage/API_Connections/openai.md#mistral-ai)

### OpenRouter

- 提供统一 API 访问市面上所有主流 LLM
- 按 token 付费的积分系统，以及每日请求有限的免费模型
- 不强制内容审核，除非 LLM 供应商有要求
- [官网](https://openrouter.ai)，[配置说明](/Usage/API_Connections/OpenRouter.md)

### DeepSeek

- 提供访问热门 DeepSeek V3（`deepseek-chat`）和 DeepSeek R1（`deepseek-reasoner`）最新版本的能力
- 需要购买积分（最低 2 美元），但模型质价比相当高
- API 层面无内容审核，但模型可能会拒绝某些提示词
- [官网](https://platform.deepseek.com/)，[配置说明](/Usage/API_Connections/openai.md#deepseek)

### AI21

- 提供 Jamba 系列开放模型的访问
- 提供免费试用（三个月 10 美元），之后按月按 token 付费
- [官网](https://ai21.com/)，[配置说明](/Usage/API_Connections/openai.md#ai21)

### Cohere

- 提供 Cohere 最新模型（command-r、command-a、c4ai-aya 等）的访问
- 提供免费层（试用密钥），速率限制对于日常使用足够
- [官网](https://cohere.com/)，[配置说明](/Usage/API_Connections/openai.md#cohere)

### Perplexity

- 通过 API 提供独特的 Perplexity Sonar 联网模型
- 需要配置账单并购买积分
- [官网](https://perplexity.ai/)，[配置说明](/Usage/API_Connections/openai.md#perplexity)

### Mancer AI

- 托管多个系列无约束模型的服务
- 使用「积分」为各模型的 token 付费
- 默认不记录提示词，但可以开启以换取 token 积分折扣
- 使用类似 `Oobabooga TextGeneration WebUI` 的 API，详见 [Mancer 文档](https://mancer.tech/docs/clients/#sampling-parameters)
- [官网](https://mancer.tech/)，[配置说明](/Usage/API_Connections/mancer.md)

### DreamGen

- 专为可控创意写作调优的无审查模型
- 提供每月免费积分及付费订阅
- 模型规模从 7B 到 70B
- [配置说明](DreamGen.md)

### Pollinations

- 无需配置，开箱即用
- 免费提供多种模型的访问
- 输出内容偶尔可能包含带第三方服务链接的广告

### NovelAI

- 无内容过滤，最新模型基于 Llama 3
- 需要付费订阅，订阅套餐决定最大上下文长度
- [官网](https://novelai.net/)，[配置说明](/Usage/API_Connections/novelai.md)

### Electron Hub

- 一个 API 密钥即可解锁多家供应商（OpenAI、Anthropic、DeepSeek 等）的文本和图像生成模型
- 每天提供 0.25 美元免费积分，付费计划可用
- [官网](https://www.electronhub.ai/)，[配置说明](/Usage/API_Connections/openai.md#electron-hub)

### AI/ML API

- 统一 API，支持 300+ 个模型，涵盖 Claude、GPT-4o、Gemini、LLaMA 3、Mistral 等
- 提供有速率限制的免费层、订阅计划及按需付费选项
- [官网](https://aimlapi.com)，[文档](https://docs.aimlapi.com)，[模型列表](https://aimlapi.com/models)
