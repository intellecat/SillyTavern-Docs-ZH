# KoboldCpp
KoboldCpp 是一个用于 GGML 和 GGUF 模型的独立 API。

Nyx 的这个 [VRAM 计算器](https://huggingface.co/spaces/NyxKrage/LLM-Model-VRAM-Calculator)将告诉您模型大约需要多少 RAM/VRAM。

## Nvidia GPU 快速入门
本指南假设您使用的是 Windows。
* 下载最新版本：https://github.com/LostRuins/koboldcpp/releases
* 启动 KoboldCpp。您可能会看到 Microsoft Defender 的弹出窗口，点击"仍要运行"。
* 截至 1.58 版本，KoboldCpp 应该是这样的：

![KoboldCpp 1.58](/static/koboldcpp.png)

* 在"快速启动"标签下，选择模型和您首选的"上下文大小"。
* 选择"使用 CuBLAS"并确保"GPU ID"旁边的黄色文本与您的 GPU 匹配。
* 即使您的 VRAM 较低，也不要勾选"低 VRAM"。
* 除非您使用的是 Nvidia 10 系列或更旧的 GPU，否则取消勾选"使用 QuantMatMul (mmq)"。
* 当您加载模型时，"GPU 层数"应该已经填充。暂时保持不变。
* 在"硬件"标签下，勾选"高优先级"。
* 点击"保存"，这样您就不必在每次启动时都配置 KoboldCpp。
* 点击"启动"并等待模型加载。

您应该看到类似这样的内容：
```
Load Model OK: True
Embedded Kobold Lite loaded.
Starting Kobold API on port 5001 at http://localhost:5001/api/
Starting OpenAI Compatible API on port 5001 at http://localhost:5001/v1/
======
Please connect to custom endpoint at http://localhost:5001
```
现在您可以在 SillyTavern 中使用 `http://localhost:5001` 作为 API URL 连接到 KoboldCpp 并开始聊天。

**恭喜！您完成了！**

差不多吧。

### GPU 层数
KoboldCpp 正在工作，但您可以通过确保尽可能多的层卸载到 GPU 来提高性能。您应该在终端中看到类似这样的内容：
```
llm_load_tensors: offloading 9 repeating layers to GPU
llm_load_tensors: offloaded 9/33 layers to GPU
llm_load_tensors:        CPU buffer size = 25215.88 MiB
llm_load_tensors:      CUDA0 buffer size =  7043.34 MiB
....................................................................................................
llama_kv_cache_init:  CUDA_Host KV buffer size =  1479.19 MiB
llama_kv_cache_init:      CUDA0 KV buffer size =   578.81 MiB
```

不要害怕这些数字；这部分比看起来更容易。`CPU buffer size` 指的是正在使用的系统 RAM 数量。忽略它。`CUDA0 buffer size` 指的是正在使用的 GPU VRAM 数量。`CUDA_Host KV buffer size` 和 `CUDA0 KV buffer size` 指的是为模型的上下文专门分配的 GPU VRAM 数量。在这种情况下，KoboldCpp 使用了大约 9 GB 的 VRAM。

我有 12 GB 的 VRAM，只有 2 GB 的 VRAM 用于上下文，所以我还剩下大约 10 GB 的 VRAM 来加载模型。因为 9 层使用了大约 7 GB 的 VRAM，而 `7000 / 9 = 777.77`，所以我们可以假设每层使用大约 `777.77 MIB` 的 VRAM。`10,000 MIB / 777.77 = 12.8`，所以我会向下取整，从现在开始用这个模型加载 12 层。

现在使用您系统的模型、上下文大小和 VRAM 进行自己的计算，并重启 KoboldCpp：
* 如果您很聪明，之前点击了"保存"，现在您可以用"加载"加载您之前的配置。否则，选择您之前选择的相同设置。
* 将"GPU 层数"更改为您新的、VRAM 优化的数字（在我的情况下是 12 层）。
* 点击"保存"以保存您更新的配置。

您现在应该看到类似这样的内容：
```
llm_load_tensors: offloading 12 repeating layers to GPU
llm_load_tensors: offloaded 12/33 layers to GPU
llm_load_tensors:        CPU buffer size = 25215.88 MiB
llm_load_tensors:      CUDA0 buffer size =  9391.12 MiB
....................................................................................................
llama_kv_cache_init:  CUDA_Host KV buffer size =  1286.25 MiB
llama_kv_cache_init:      CUDA0 KV buffer size =   771.75 MiB
```

KoboldCpp 使用了我 12 GB VRAM 中的大约 11.5 GB。这应该比 KoboldCpp 自动生成的设置表现得更好。

**恭喜！您（真的）完成了！**

要更深入地了解 KoboldCpp 设置，请查看 Kalmomaze 的 [Simple Llama + SillyTavern 设置指南](https://rentry.org/llama_v2_sillytavern)。
