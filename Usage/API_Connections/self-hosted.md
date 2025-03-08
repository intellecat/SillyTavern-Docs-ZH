---
order: 90
icon: desktop-download
route: /usage/how-to-use-a-self-hosted-model/
---

# 自托管 AI 模型

!!! warning
本指南基于作者的个人经验和知识，并不是绝对真理。所有陈述都应该谨慎对待。如果您有任何更正或建议，请在 Discord 上联系我们或向 [SillyTavern 文档仓库](https://github.com/SillyTavern/SillyTavern-Docs)发送 PR。
!!!

## 简介

本指南旨在帮助您在自己的电脑上使用 SillyTavern 运行本地 AI（从现在开始，我们将使用正确的术语，称之为 LLM）。在询问技术支持问题之前，请先阅读本指南。

### 哪些大语言模型最好？

这个问题无法回答，因为没有标准化的"最佳"衡量标准。社区在 Reddit 和 Discord 上有足够的资源和讨论，可以形成对首选/常用模型的一些看法。您的体验可能会有所不同。

### 什么是最佳配置？

如果存在最佳或显而易见的设置，还需要配置吗？最佳配置是适合您的配置。这是一个反复试验的过程。

## 硬件要求和方向

这是一个复杂的主题，所以我会坚持基本要点并进行概括。

* 您可以从互联网上下载数千个免费的 LLM，类似于 Stable Diffusion 有大量可用于生成图像的模型。
* 运行未经修改的 LLM 需要具有大量 VRAM（GPU 内存）的强大 GPU。比您能拥有的还要多。
* 可以通过使用 GPTQ 或 AWQ 等量化技术来压缩模型，从而减少 VRAM 要求。这使模型的能力略有下降，但大大减少了运行它所需的 VRAM。突然间，这让拥有 3080 等游戏 GPU 的人也能运行 13B 模型。虽然它不如未量化的模型好，但仍然不错。
* 更好的是：还存在一种称为 GGUF（以前称为 GGML）的模型格式和量化方法，它已成为没有强大 GPU 的普通用户的首选格式。这允许您完全不使用 GPU 就能运行 LLM。它只会使用 CPU 和 RAM。这比在 GPU 上使用 GPTQ/AWQ 运行 LLM 要慢得多（可能慢 15 倍），特别是在提示词处理期间，但模型的能力同样出色。然后 GGUF 创建者通过添加一个配置选项进一步优化了 GGUF，该选项允许拥有游戏级 GPU 的用户将模型的部分内容卸载到 GPU，使他们能够以 GPU 速度运行部分模型（注意，这不会减少 RAM 要求，它只会提高您的生成速度）。
* 有不同大小的模型，根据它们训练时使用的参数数量命名。您会看到像 7B、13B、30B、70B 等名称。您可以将这些视为模型的大脑大小。来自同一模型系列的 13B 模型将比 7B 更强大：它们在相同的数据上训练，但更大的大脑可以更好地保留知识并思考得更连贯。更大的模型也需要更多的 VRAM/RAM。
* 有几种量化程度（8 位、5 位、4 位等）。位数越低，模型退化越严重，但硬件要求也越低。因此，即使在性能较差的硬件上，您也可能能够运行所需模型的 4 位版本。甚至还有 3 位和 2 位量化，但在这一点上，您已经在浪费时间了。还有进一步的量化子类型，命名为 k_s、k_m、k_l 等。k_m 比 k_s 好，但需要更多资源。
* 上下文大小（您的对话在模型丢弃部分内容之前可以变得多长）也会影响 VRAM/RAM 要求。幸运的是，这是一个可配置的设置，允许您使用较小的上下文来减少 VRAM/RAM 要求。（注意：基于 Llama2 的模型的上下文大小是 4k。Mistral 宣传为 8k，但实际上是 4k。）
* 2023 年的某个时候，NVIDIA 更改了他们的 GPU 驱动程序，如果您需要的 VRAM 超过 GPU 所拥有的，任务不会崩溃，而是会开始使用常规 RAM 作为备用。这会降低 LLM 的写入速度，但模型仍然可以工作并提供相同质量的输出。幸运的是，这种行为[可以禁用](https://nvidia.custhelp.com/app/answers/detail/a_id/5490)。

考虑到以上所有因素，硬件要求和性能完全取决于模型系列、模型类型、模型大小、量化方法等。

#### 模型大小计算器
您可以使用 [Nyx 的模型大小计算器](https://huggingface.co/spaces/NyxKrage/LLM-Model-VRAM-Calculator)来确定需要多少 RAM/VRAM。

记住，您想要运行可以适应您内存的最大、最少量化的模型，即不会导致[磁盘交换](https://serverfault.com/a/48487)。

## 下载 LLM

要开始，您需要下载一个 LLM。找到和下载 LLM 最常见的地方是 HuggingFace。那里有数千个可用的模型。查找 GGUF 模型的好方法是查看 bartowski 的账户页面：<https://huggingface.co/bartowski>。如果您不想要 GGUF，他会链接到原始模型页面，在那里您可能会找到该模型的其他格式。

在给定模型的页面上，您会找到一堆文件。

* 您可能不需要所有这些文件！对于 GGUF，您只需要 .gguf 模型文件（通常 4-11GB）。如果您找到多个大文件，通常都是同一模型的不同量化版本，您只需要选择一个。
* 对于 .safetensors 文件（可以是 GPTQ 或 AWQ 或 HF 量化或未量化），如果您在文件名中看到像 model-00001-of-00003.safetensors 这样的数字序列，那么您需要所有 3 个 .safetensors 文件 + 仓库中的所有其他文件（分词器、配置等）才能获得完整的模型。
* 截至 2024 年 1 月，Mixtral MOE 8x7B 被广泛认为是本地 LLM 的最新技术。如果您有 32GB 的 RAM 来运行它，一定要尝试一下。如果您的 RAM 少于 32GB，那么使用 Kunoichi-DPO-v2-7B，尽管它的大小较小，但一开始就表现出色。

### 下载 Kunoichi-DPO-v2-7B 的步骤

我们将在本指南的其余部分使用 Kunoichi-DPO-v2-7B 模型。它是一个基于 Mistral 7B 的出色模型，只需要 7GB RAM，并且表现远超其规模。注意：Kunoichi 使用 Alpaca 提示。

* 前往 <https://huggingface.co/brittlewis12/Kunoichi-DPO-v2-7B-GGUF>
* 点击"Files and versions"。您会看到几个文件的列表。这些都是同一个模型，但提供了不同的量化选项。点击文件"kunoichi-dpo-v2-7b.Q6_K.gguf"，这给我们一个 6 位量化。
* 点击"download"按钮。您的下载应该开始了。

### 如何识别模型类型

像 TheBloke 这样优秀的模型上传者会给出描述性的名称。但如果他们没有：

* 文件名以 .gguf 结尾：GGUF CPU 模型（显然）
* 文件名以 .safetensors 结尾：可以是未量化的，或 HF 量化的，或 GPTQ，或 AWQ
* 文件名是 pytorch-***.bin：与上面相同，但这是一个较旧的模型文件格式，允许在加载模型时执行任意 Python 脚本，被认为是不安全的。如果您信任模型创建者，或者别无选择，您仍然可以使用它，但如果有选择，请选择 .safetensors。
* 存在 config.json？查看它是否有 quant_method。
* q4 表示 4 位量化，q5 是 5 位量化，等等
* 您看到像 -16k 这样的数字？那是增加的上下文大小（即您的对话在模型忘记聊天开始之前可以变得多长）！注意，更高的上下文大小需要更多的 VRAM。

## 安装 LLM 服务器：Oobabooga 或 KoboldAI

现在 LLM 在您的电脑上了，我们需要下载一个工具，作为 SillyTavern 和模型之间的中间人：它将加载模型，并将其功能作为本地 HTTP Web API 暴露出来，供 SillyTavern 使用，就像 SillyTavern 与 OpenAI GPT 或 Claude 等付费网络服务通信一样。您使用的工具应该是 KoboldAI 或 Oobabooga（或其他兼容工具）。

本指南涵盖了这两个选项，您只需要一个。

!!! warning
如果您在 Docker 上托管 SillyTavern，请使用 **http://host.docker.internal:<port>** 而不是 **http://127.0.0.1:<port>**。这是因为 SillyTavern 从运行在 Docker 容器中的服务器连接到 API 端点。Docker 的网络栈与主机的网络栈是分开的，因此回环接口不共享。
!!!

### 下载和使用 KoboldCpp（无需安装，GGUF 模型）
1. 访问 https://koboldai.org/cpp，您将看到最新版本和各种可下载的文件。
在撰写本文时，他们列出的最新 CUDA 版本是 cu12，这将在现代 Nvidia GPU 上运行最好，如果您有较旧的 GPU 或不同品牌，您可以使用常规的 koboldcpp.exe。如果您有旧 CPU，KoboldCpp 可能会在您尝试加载模型时崩溃，在这种情况下，尝试 _oldcpu 版本看看是否能解决您的问题。
2. KoboldCpp 不需要安装，一旦您启动 KoboldCpp，您就可以使用"Model"字段旁边的"Browse"按钮立即选择您的 GGUF 模型，如上面链接的模型。
3. 默认情况下，KoboldCpp 的最大上下文为 4K，即使您在 SillyTavern 中设置更高，如果您希望以更高的上下文运行模型，请确保在启动模型之前调整此屏幕上的上下文滑块。请记住，更多的上下文大小意味着更高的（视频）内存要求，如果您设置得太高或加载的模型对您的系统来说太大，KoboldCpp 将自动开始使用您的 CPU 来处理无法放入 GPU 的层，这将慢得多。
4. 点击"Launch"，如果一切顺利，将打开一个新的网页，显示 KoboldAI Lite，您可以在那里测试一切是否正常工作。
5. 打开 SillyTavern 并点击 API 连接（顶部栏中的第二个按钮）
6. 将 API 设置为"文本补全"，API 类型设置为 KoboldCpp。
7. 将服务器 URL 设置为 <http://127.0.0.1:5001/> 或 KoboldCpp 给您的链接（如果它不在同一系统上运行）（您可以激活 KoboldCpp 的远程隧道模式以获取可从任何地方访问的链接）。
8. 点击连接。它应该成功连接并检测到 kunoichi-dpo-v2-7b.Q6_K.gguf 作为模型。
9. 与角色聊天以测试它是否工作。

### 优化 KoboldCpp 速度的提示
1. Flash Attention 将帮助减少内存要求，根据您的系统，它可能更快或更慢，并且将允许您在 GPU 上放置比默认更多的层。
2. KoboldCpp 会为其他软件留出一些空间，当它猜测层以防止问题时，如果您打开的程序很少，并且无法将模型完全放入 GPU，您可能可以添加一些额外的层。
3. 如果模型对上下文大小使用了太多内存，您可以通过量化 KV 来减少这个问题。这将降低输出质量，但可以帮助您在 GPU 上放置更多层。要做到这一点，您需要转到 KoboldCpp 中的 Tokens 标签，然后禁用 Context Shifting 并启用 Flash Attention。这将解锁量化 KV 缓存滑块，数字越低意味着模型的内存/智能越少。
4. 在较慢的系统上运行 KoboldCpp，处理提示词需要很长时间？当您避免使用知识库、随机化或其他动态更改输入的功能时，Context Shifting 效果最好。保持启用 context shifting，KoboldCpp 将帮助您避免长时间的重新处理。

### 安装 Oobabooga

这是一个更正确/傻瓜式的安装程序：

1. git clone <https://github.com/oobabooga/text-generation-webui>（或在浏览器中将他们的仓库下载为 .zip，然后解压）
2. 运行 start_windows.bat 或您的操作系统对应的文件
3. 当被问及时，选择您的 GPU 类型。即使您打算使用 GGUF/CPU，如果您的 GPU 在列表中，现在就选择它，因为它将给您一个称为 GPU 分片的速度优化选项（无需从头重新安装）。如果您没有游戏级 dGPU（NVIDIA、AMD），选择 None。
4. 等待安装完成
5. 将 kunoichi-dpo-v2-7b.Q6_K.gguf 放在 text-generation-webui/models 中
6. 打开 text-generation-webui/CMD_FLAGS.txt，删除里面的所有内容并写入：--api
7. 重启 Oobabooga
8. 访问 <http://127.0.0.1:5000/docs>。它是否加载了一个 FastAPI 页面？如果没有，您在某个地方出错了。

### 在 Oobabooga 中加载我们的模型

1. 在浏览器中打开 <http://127.0.0.1:7860/>
2. 点击 Model 标签
3. 在下拉菜单中，选择我们的 Kunoichi DPO v2 模型。它应该已经自动选择了 llama.cpp 加载器。
4. （可选）我们之前多次提到"GPU 卸载"：那就是这个页面上的 n-gpu-layers 设置。如果您想使用它，在加载模型之前设置一个值。作为基本参考，将其设置为 30 对于 13B 及以下的模型使用不到 6GB VRAM。（它随模型架构和大小而变化）
5. 点击 Load

### 配置 SillyTavern 与 Oobabooga 通信

1. 点击 API 连接（顶部栏中的第二个按钮）
2. 将 API 设置为"文本补全"
3. 将 API 类型设置为"默认（Oobabooga）"
4. 将服务器 URL 设置为 <http://127.0.0.1:5000/>
5. 点击连接。它应该成功连接并检测到 kunoichi-dpo-v2-7b.Q6_K.gguf 作为模型。
6. 与角色聊天以测试它是否工作

## 结论

恭喜，您现在应该有一个正常工作的本地 LLM 了。
