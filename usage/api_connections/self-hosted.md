# 自托管 AI 模型

!!!warning
本指南基于作者的个人经验和知识，并不代表绝对真理。所有陈述均应保持审慎态度。如果您有任何更正或建议，请在 Discord 上联系我们，或向 [SillyTavern 文档仓库](https://github.com/SillyTavern/SillyTavern-Docs) 提交 PR。
!!!

## 简介

本指南旨在帮助您在自己的电脑上通过 SillyTavern 运行本地 AI（从现在起我们将使用正确的术语，称之为 LLM）。在向他人提问技术支持问题之前，请先阅读本指南。

### 哪些大语言模型最好？

这个问题无法给出答案，因为不存在标准化的"最佳"衡量尺度。社区在 Reddit 和 Discord 上拥有足够的资源与讨论，可以帮助您形成对首选/常用模型的基本判断。您的实际体验可能因人而异。

### 什么是最佳配置？

如果存在某种最佳或显而易见的设置，还需要配置选项吗？最佳配置就是适合您的配置，这是一个反复试验的过程。

## 硬件要求与概览

这是一个复杂的话题，因此我将只介绍要点并进行概括。

* 您可以从互联网上下载数千个免费的 LLM，类似于 Stable Diffusion 拥有大量可用于生成图像的模型。
* 运行未经修改的 LLM 需要配备大量 VRAM（GPU 显存）的顶级 GPU，其需求远超普通用户所能拥有的。
* 通过使用 GPTQ 或 AWQ 等量化技术压缩模型，可以降低 VRAM 需求。这会使模型能力略有下降，但大幅减少了运行所需的 VRAM。正因如此，拥有 3080 等游戏级 GPU 的用户也能运行 13B 模型。尽管效果不如未量化模型，但仍然相当出色。
* 更好的消息是：还存在一种名为 GGUF（前身为 GGML）的模型格式与量化方案，已成为没有顶级 GPU 的普通用户的首选格式。这使您无需任何 GPU 即可运行 LLM，仅使用 CPU 和内存。这比在 GPU 上使用 GPTQ/AWQ 运行 LLM 要慢得多（大约慢 15 倍），尤其是在提示词处理阶段，但模型的能力同样出色。随后，GGUF 的创建者通过添加一个配置选项进一步优化了 GGUF，该选项允许拥有游戏级 GPU 的用户将模型的部分内容卸载到 GPU 上运行，从而以 GPU 速度处理部分模型（注意：这不会降低内存需求，只会提升生成速度）。
* 模型有不同的规模，以训练时使用的参数数量命名，如 7B、13B、30B、70B 等。您可以将其理解为模型的"大脑容量"。来自同一模型系列的 13B 模型将比 7B 更强大：两者使用相同的数据训练，但更大的"大脑"能更好地保留知识并进行更连贯的推理。更大的模型也需要更多 VRAM/RAM。
* 量化有几种不同的程度（8 位、5 位、4 位等）。位数越低，模型性能退化越严重，但硬件要求也越低。因此，即便在性能较差的硬件上，您也可能能够运行所需模型的 4 位版本。甚至还有 3 位和 2 位量化，但那样已经得不偿失了。此外还有更细分的量化子类型，命名为 k_s、k_m、k_l 等。k_m 优于 k_s，但需要更多资源。
* 上下文大小（即对话在模型开始丢弃早期内容之前能达到的长度）同样影响 VRAM/RAM 需求。幸运的是，这是一个可配置的设置，允许您使用较小的上下文以降低 VRAM/RAM 需求。（注意：基于 Llama2 的模型上下文大小为 4k；Mistral 宣传为 8k，但实际上是 4k。）
* 2023 年某时，NVIDIA 更改了 GPU 驱动程序：当所需 VRAM 超出 GPU 容量时，任务不再直接崩溃，而是开始使用普通内存作为备用。这会降低 LLM 的生成速度，但模型仍可正常工作并输出同等质量的内容。幸运的是，此行为[可以禁用](https://nvidia.custhelp.com/app/answers/detail/a_id/5490)。

综上所述，硬件需求和性能表现因模型系列、模型类型、模型大小、量化方法等因素的不同而完全不同。

#### 模型大小计算器

您可以使用 [Nyx 的模型大小计算器](https://huggingface.co/spaces/NyxKrage/LLM-Model-VRAM-Calculator) 来估算所需的 RAM/VRAM 大小。

请记住，您的目标是运行能够放入内存的最大、量化程度最低的模型，即不会导致[磁盘交换](https://serverfault.com/a/48487)的模型。

## 下载 LLM

首先，您需要下载一个 LLM。查找和下载 LLM 最常见的平台是 HuggingFace，上面有数千个可用模型。查找 GGUF 模型的好方法是浏览 bartowski 的账户页面：<https://huggingface.co/bartowski>。如果您不需要 GGUF 格式，他会链接到原始模型页面，在那里您可能会找到该模型的其他格式。

在某个模型的页面上，您会看到一系列文件。

* 您可能不需要所有文件！对于 GGUF，您只需要 .gguf 模型文件（通常为 4-11GB）。如果您看到多个大型文件，通常是同一模型的不同量化版本，您只需选择其中一个。
* 对于 .safetensors 文件（可以是 GPTQ、AWQ、HF 量化或未量化格式），如果文件名中包含类似 model-00001-of-00003.safetensors 的数字序列，则需要下载全部 3 个 .safetensors 文件，以及仓库中的所有其他文件（分词器、配置文件等），才能获得完整的模型。
* 截至 2024 年 1 月，Mixtral MOE 8x7B 被广泛认为是本地 LLM 的最佳选择。如果您有 32GB RAM 可以运行它，强烈建议尝试。如果 RAM 不足 32GB，则推荐使用 Kunoichi-DPO-v2-7B，这款模型虽然体积较小，但开箱即用的表现相当出色。

### 下载 Kunoichi-DPO-v2-7B 的步骤

本指南后续部分将使用 Kunoichi-DPO-v2-7B 模型。它是一款基于 Mistral 7B 的出色模型，仅需 7GB RAM，表现远超其规模所限。注意：Kunoichi 使用 Alpaca 提示格式。

* 前往 <https://huggingface.co/brittlewis12/Kunoichi-DPO-v2-7B-GGUF>
* 点击"Files and versions"，您将看到若干文件的列表。这些都是同一模型的不同量化版本。点击文件"kunoichi-dpo-v2-7b.Q6_K.gguf"，这是一个 6 位量化版本。
* 点击"download"按钮，下载即可开始。

### 如何识别模型类型

像 TheBloke 这样优秀的模型上传者会提供描述性的名称。如果没有的话：

* 文件名以 .gguf 结尾：GGUF CPU 模型（显而易见）
* 文件名以 .safetensors 结尾：可能是未量化、HF 量化、GPTQ 或 AWQ 格式
* 文件名为 pytorch-***.bin：与上一条相同，但这是一种较旧的模型文件格式，会在加载模型时允许执行任意 Python 脚本，被认为存在安全风险。如果您信任模型创建者，或别无选择，仍可使用，但有选择的话请优先选择 .safetensors。
* 存在 config.json？查看其中是否有 quant_method 字段。
* q4 表示 4 位量化，q5 表示 5 位量化，以此类推
* 文件名中出现类似 -16k 的数字？那是增加的上下文大小（即对话在模型忘记早期内容之前可以达到的长度）！注意，更高的上下文大小需要更多 VRAM。

## 安装 LLM 服务器：Oobabooga 或 KoboldAI

LLM 现在已经在您的电脑上了，接下来我们需要下载一个工具，充当 SillyTavern 与模型之间的中间层：它将加载模型，并将其功能以本地 HTTP Web API 的形式对外暴露，供 SillyTavern 使用——就像 SillyTavern 与 OpenAI GPT 或 Claude 等付费网络服务通信的方式一样。您使用的工具应为 KoboldAI 或 Oobabooga（或其他兼容工具）。

本指南涵盖两种选项，您只需选择其中一个。

!!!warning
如果您在 Docker 上托管 SillyTavern，请使用 **http://host.docker.internal:\<port\>** 代替 **http://127.0.0.1:\<port\>**。这是因为 SillyTavern 从运行在 Docker 容器中的服务器连接到 API 端点。Docker 的网络栈与宿主机是隔离的，回环接口并不共享。
!!!

### 下载和使用 KoboldCpp（无需安装，适用于 GGUF 模型）

1. 访问 https://koboldai.org/cpp，您将看到最新版本及各种可下载的文件。
撰写本文时，他们列出的最新 CUDA 版本为 cu12，在现代 Nvidia GPU 上表现最佳。如果您使用的是较旧的 GPU 或其他品牌，可以使用常规版 koboldcpp.exe。如果您的 CPU 较旧，KoboldCpp 在尝试加载模型时可能会崩溃，这种情况下请尝试 _oldcpu 版本，看看是否能解决问题。
2. KoboldCpp 无需安装，启动后即可通过 Model 字段旁边的 Browse 按钮直接选择您的 GGUF 模型（如上面链接的模型）。
3. 默认情况下，KoboldCpp 的最大上下文为 4K，即使您在 SillyTavern 中设置了更高的值。如果希望以更大的上下文运行模型，请在启动模型之前调整此界面上的上下文滑块。请注意，上下文越大，对（视频）内存的需求也越高。如果设置过高，或加载的模型超出系统承载能力，KoboldCpp 将自动将无法放入 GPU 的层改由 CPU 处理，速度会慢很多。
4. 点击 Launch，如果一切正常，将打开一个新网页显示 KoboldAI Lite，您可以在那里测试是否正常工作。
5. 打开 SillyTavern，点击 API 连接（顶部工具栏中的第二个按钮）
6. 将 API 设置为"文本补全"，API 类型设置为 KoboldCpp。
7. 将服务器 URL 设置为 <http://127.0.0.1:5001/>，或在 KoboldCpp 不在同一系统上运行时使用其提供的链接（您可以激活 KoboldCpp 的远程隧道模式，获取一个可从任何地方访问的链接）。
8. 点击连接。应成功连接，并检测到 kunoichi-dpo-v2-7b.Q6_K.gguf 作为当前模型。
9. 与某个角色聊天，测试是否正常工作。

### 优化 KoboldCpp 速度的技巧

1. Flash Attention 有助于降低内存需求，根据系统不同，可能会更快或更慢，同时能让您在 GPU 上放置比默认更多的层。
2. KoboldCpp 在估算层数时会为其他软件预留一些空间以防出错。如果您打开的程序较少，且无法将模型完全放入 GPU，可以尝试手动增加几层。
3. 如果模型的上下文大小占用了过多内存，可以通过量化 KV 缓存来降低需求。这会略微降低输出质量，但可以帮助您在 GPU 上放置更多层。操作方法：进入 KoboldCpp 的 Tokens 标签，禁用 Context Shifting 并启用 Flash Attention，此时将解锁量化 KV 缓存滑块，数值越低意味着占用内存越少，但模型智能也相应降低。
4. 在较慢的系统上运行 KoboldCpp，提示词处理耗时较长？当您避免使用知识库、随机化或其他动态改变输入的功能时，Context Shifting 效果最佳。保持启用 Context Shifting，KoboldCpp 将帮助您避免漫长的重新处理等待。

### 安装 Oobabooga

!!!note
根据您安装 Oobabooga 的方式，文件路径可能略有不同：通过 git clone 安装时为 `/text-generation-webui/user_data`，通过 .zip 方式安装时为 `/text-generation-webui-main/user_data`。
!!!

以下是一个更规范/更适合新手的安装流程：

1. git clone <https://github.com/oobabooga/text-generation-webui>（或在浏览器中将仓库下载为 .zip 文件后解压）
2. 运行 `start_windows.bat` 或您操作系统对应的启动脚本
3. 当提示选择 GPU 类型时，即使您打算使用 GGUF/CPU，如果您的 GPU 在列表中，请现在就选择它——这样以后可以使用一个名为 GPU 分片的速度优化选项（无需从头重新安装）。如果您没有游戏级独立 GPU（NVIDIA、AMD），请选择 None。
4. 等待安装完成
5. 将 kunoichi-dpo-v2-7b.Q6_K.gguf 放入 `text-generation-webui/user_data/models` 目录
6. 打开 `text-generation-webui/user_data/CMD_FLAGS.txt`，删除其中的所有内容，写入：`--api`
7. 重启 Oobabooga
8. 访问 <http://127.0.0.1:5000/docs>，是否加载了 FastAPI 页面？如果没有，说明某个步骤出了问题。

### 在 Oobabooga 中加载模型

1. 在浏览器中打开 <http://127.0.0.1:7860/>
2. 点击 Model 标签
3. 在下拉菜单中选择我们的 Kunoichi DPO v2 模型，应该已自动选择 llama.cpp 加载器。
4. （可选）我们之前多次提到"GPU 卸载"：即本页面上的 n-gpu-layers 设置。如需使用，请在加载模型前设置一个值。基本参考：对于 13B 及以下的模型，设为 30 大约使用不到 6GB VRAM。（具体数值因模型架构和大小而异）
5. 点击 Load

### 配置 SillyTavern 与 Oobabooga 通信

1. 点击 API 连接（顶部工具栏中的第二个按钮）
2. 将 API 设置为"文本补全"
3. 将 API 类型设置为"默认（Oobabooga）"
4. 将服务器 URL 设置为 <http://127.0.0.1:5000/>
5. 点击连接。应成功连接，并检测到 kunoichi-dpo-v2-7b.Q6_K.gguf 作为当前模型。
6. 与某个角色聊天，测试是否正常工作

## 结语

恭喜，您现在应该已经拥有一个正常运行的本地 LLM 了。
