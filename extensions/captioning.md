---
route: /extensions/captioning/
templating: false
---

# 图像描述

图像描述允许 SillyTavern 自动为聊天中使用的图像生成文本描述。

当您希望 AI 角色能够"看到"并响应对话中的视觉内容时,请使用图像描述。

- 为您上传或粘贴到消息中的图像创建描述
- 为聊天历史记录中的现有图像添加上下文
- 使用各种来源进行生成,包括本地模型、云 API 和众包网络

有些选项不需要设置、不需要付费、也不需要 GPU。还有一些选项需要其中的一些或全部。选择适合您需求和资源的选项。

图像描述扩展内置在 SillyTavern 中,无需单独安装。

## 快速开始

1. 设置:
    - 在 **<i class="fa-solid fa-cubes"></i> 扩展**面板中打开**图像描述**面板
    - 选择一个描述来源(最有可能是"本地"或"多模态")
    - 对于"多模态",请确保您已在 **<i class="fa-solid fa-plug"></i> API 连接**选项卡中设置了连接
2. 生成描述:
    - 从 **<i class="fa-solid fa-magic-wand-sparkles"></i> 扩展**弹出菜单中选择"**生成描述**"
    - 在提示时选择一个图像文件
    - 等待生成描述
3. 审查和发送:
    - 带描述的图像将插入到您的消息中
    - 使用图像工具提示查看描述
    - 单击 **<i class="fa-solid fa-paper-plane"></i> 发送**,看看您的角色对图像的看法!

## 面板控制

### 来源选择

选择图像描述的来源。支持的选项:

| 来源                           | 描述                                                                                                                                                                                  |
|----------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [多模态](#multimodal-source) | **云端**: OpenAI、Anthropic、Google、MistralAI 等。 <br>**本地**: Ollama、llama.cpp、KoboldCpp、Text Generation WebUI 和 vLLM。 <br>支持自定义提示,因此您可以向图像提问。 |
| [本地](#local-source)           | 使用在 SillyTavern 服务器内本地运行的 [transformers.js](https://huggingface.co/docs/transformers.js/en/index)。零设置!                                                                     |
| Horde                            | 使用 [AI Horde](https://aihorde.net/) 网络,这是一个众包的分布式图像生成模型网络。无需下载、配置或付费。响应时间可变。                       |
| Extras                           | Extras 项目于 2024 年 4 月停止,不再维护或支持。                                                                                                                        |

### 描述配置
- **描述提示**: 输入自定义描述提示。默认提示是"这张图片里有什么?"
- **每次都询问**: 切换以请求每个图像描述的自定义提示

### 消息模板
- **消息模板**: 自定义描述消息模板。使用 `{{caption}}` 宏插入生成的描述。默认模板是 `[{{user}} 向 {{char}} 发送了一张包含以下内容的图片: {{caption}}]`

### 自动描述
- **自动描述图像**: 切换以启用自动描述粘贴或附加到消息的图像
- **保存前编辑描述**: 切换以允许在保存描述之前编辑描述

## 描述图像

在 SillyTavern 中描述图像的所有方法:

* 从 **<i class="fa-solid fa-magic-wand-sparkles"></i> 扩展**弹出菜单中选择"**生成描述**",并在提示时选择一个图像文件
* 单击消息中图像顶部的 <i class="fa-solid fa-envelope-open-text"></i> **描述**图标
* 在启用[自动描述](#auto-captioning)的情况下,直接将图像粘贴到聊天输入中
* 使用消息操作中的 <i class="fa-solid fa-paperclip"></i> **嵌入文件或图像**按钮将图像文件附加到消息
* 发送带有嵌入图像的消息
* 使用 `/caption` [斜杠命令](#slash-command-caption)

## 自动描述
自动描述功能允许您在将图像添加到聊天时自动为图像生成描述,而无需每次手动触发描述过程。

要启用,请选中图像描述面板中的"自动描述图像"复选框。您还可以通过选中"保存前编辑描述"框来选择在保存描述之前编辑描述。

启用后,自动描述将在以下场景中触发:

- 当图像直接粘贴到聊天输入中时。
- 当图像文件附加到消息时。
- 当发送带有嵌入图像的消息时。

系统将使用您选择的描述来源(本地、Extras、Horde 或多模态)和配置的设置为图像生成描述。

### 保存前编辑描述(精炼模式)

如果您启用了"保存前编辑描述"选项:
1. 添加图像后,将出现一个带有生成描述的弹出窗口。
2. 您可以根据需要审查和编辑描述。
3. 单击"确定"以应用描述,或单击"取消"以丢弃描述而不保存。

### 描述发送
生成的(并且可选地编辑的)描述将使用您配置的消息模板自动插入到提示中。默认情况下,它将以这种格式发送:

```
[BaronVonUser 向 Seraphina 发送了一张包含以下内容的图片: ...]
```


## 斜杠命令: /caption
该扩展提供了一个 `/caption` 斜杠命令,可在聊天框或脚本中使用。

### 用法

```
/caption [quiet=true|false]? [mesId=number]? [prompt]
```

- `prompt`(可选): 描述模型的自定义提示。仅多模态来源支持。
- `quiet=true|false`: 如果设置为 true,则禁止向聊天发送带描述的消息。默认值为 false。
- `mesId=number`: 指定消息 ID 以从现有消息中描述图像,而不是上传新图像。

如果未提供 `mesId`,该命令将提示您上传图像。当 `quiet` 为 false(默认值)时,将向聊天发送带有描述图像的新消息。生成的描述可用作其他命令的输入。

### 示例
使用默认设置描述新图像:

```
/caption
```

使用自定义提示描述新图像:

```
/caption 描述这张图片中的主要颜色和形状
```

从消息 #5 描述图像而不发送新消息:

```
/caption mesId=5 quiet=true
```

从消息 #10 使用自定义提示描述图像,然后基于描述[生成新图像](/extensions/Stable-Diffusion.md):

```
/caption mesId=10 使用逗号分隔的关键字描述这张图片 | /imagine
```

## 本地来源

您可以在 [config.yaml](/Administration/config-yaml.md#extensions-configuration) 中更改模型。键名为 `extensions.models.captioning`。输入您想使用的 Hugging Face 模型 ID。默认值是 `Xenova/vit-gpt2-image-captioning`。

您可以使用任何支持图像描述的模型(`VisionEncoderDecoderModel` 或 "image-to-text" 管道)。该模型需要与 transformers.js 库兼容。也就是说,它需要 ONNX 权重。查找带有 `ONNX` 和 `image-to-text` 标签的模型,或具有名为 `onnx` 的文件夹,其中包含 `.onnx` 文件。

## 多模态来源

### 通用配置

- **模型**: 选择用于图像描述的模型。选项根据所选 API 而有所不同。
- **允许反向代理**: 切换以允许使用反向代理(如果已定义且有效)(OpenAI、Anthropic、Google、Mistral、xAI)

描述来源的 API 密钥和端点 URL 在 [API 连接](/Usage/API_Connections/index.md)面板中管理。首先在 API 连接中设置连接,然后在描述中将其选择为您的描述来源。

!!!warning 首先在 API 连接面板中设置
再说一次:在 **<i class="fa-solid fa-plug"></i> API 连接**中配置 API 密钥/地址/端口,并在描述中使用连接。

您仍然可以将 Claude 用于聊天,将 Google AI Studio 用于图像描述,或者其他任何组合。只需首先在"API 连接"选项卡中设置它们*两个*。然后将您的聊天完成来源切换为 Claude,将您的描述来源切换为 Google AI Studio。
!!!

对于大多数本地后端,您需要在模型后端中而不是在 SillyTavern 中设置一些选项。如果您的后端一次只能运行一个模型并且不支持自动切换,您有几个选项可以为聊天和描述使用不同的模型:

1. **辅助端点:** 使用辅助端点功能(请参阅下面的[辅助端点](#secondary-endpoints)部分)连接到不同的 API 服务器以进行描述
2. **多种连接类型:** 在 API 连接中使用文本完成和聊天完成模式连接到您的后端 - 这为您提供了两个到同一后端类型的单独连接

### 来源

要使用这些描述来源之一,请在来源下拉菜单中选择多模态。

* "我想要最好的描述,并且不介意付费": Anthropic
* "我不想付费或运行任何东西": Google AI Studio 免费层
* "我想在本地描述图像并让它正常工作": Ollama
* "我想保持本地 AI 的梦想": [KoboldCpp](#koboldcpp)
* "我想在它不工作时抱怨": ~~Extras~~

| API 提供商                      | 描述                                                                                                                                                                   |
|-----------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| AI/ML API                         | 云端、付费、各种具有视觉功能的 GPT、Claude 和 Gemini 模型                                                                                                  |
| Claude                            | 云端、付费、所有具有视觉功能的 Claude 模型                                                                                                                       |
| Cohere                            | 云端、付费、Aya Vision 8B / 32B                                                                                                                                              |
| Custom (OpenAI-compatible)        | 用于自定义 OpenAI 兼容 API,使用 API 连接选项卡中当前配置的模型                                                                                     |
| Electron Hub                      | 云端、付费、各种具有视觉功能的模型。                                                                                                                         |
| Google AI Studio                  | 云端、免费层然后付费、Gemini Flash/Pro                                                                                                                                  |
| Google Vertex AI                  | 云端、免费层、Gemini Flash/Pro                                                                                                                                            |
| Groq                              | 云端、llama-4 scout/maverick                                                                                                                                 |
| KoboldCpp                         | 本地、必须在 KoboldCpp 中配置模型                                                                                                                                      |
| llama.cpp                         | 本地、必须在 llama.cpp 中配置模型                                                                                                                                      |
| MistralAI                         | 云端、付费、pixtral-large、pixtral-12B、magistral、mistral-large 等。                                                                                                       |
| Moonshot AI                       | 云端、付费、moonshot-vision                                                                                                                                                  |
| NanoGPT                           | 云端、付费、各种具有视觉功能的 GPT/Claude/Google 模型                                                                                                        |
| Ollama                            | 本地、可以在 API 连接中配置后在描述中切换可用模型并下载[其他视觉模型](https://ollama.com/search?c=vision) |
| OpenAI                            | 云端、付费、GPT-4 Vision、4-turbo、4o、4o-mini                                                                                                                               |
| OpenRouter                        | 云端、付费(可能有免费选项)、许多模型,在 API 连接中配置后从描述中可用的模型中选择                                              |
| Pollinations                      | 云端、免费                                                                                                                                                                   |
| Text Generation WebUI (oobabooga) | 本地、必须在 ooba 中配置模型                                                                                                                                           |
| vLLM                              | 本地                                                                                                                                                         |
| xAI (Grok)                        | 云端、付费、grok-vision                                                                                                                                                      |

### 辅助端点

默认情况下,多模态来源使用 API 连接选项卡中配置的主端点。
您还可以专门为多模态描述设置辅助端点。

- 在 **<i class="fa-solid fa-cubes"></i> 扩展**面板中打开**图像描述**面板。
- 选择"多模态"作为描述来源和首选 API 提供商。
- 在"辅助描述端点 URL"字段中输入辅助端点的有效 URL。
- 选中"使用辅助 URL"框以启用辅助端点。

!!!tip
不要在 URL 末尾附加 `/v1` 或 `/chat/completions`。扩展将自动处理。
!!!

仅以下 API 支持此功能:

- KoboldCpp
- llama.cpp
- Ollama
- Text Generation WebUI (oobabooga)
- vLLM

### 来源特定指南

#### KoboldCpp

有关安装和使用 [KoboldCpp](https://github.com/LostRuins/koboldcpp) 的一般信息,请参阅 [KoboldCpp 文档](https://github.com/LostRuins/koboldcpp/wiki)。

要将 KoboldCpp 用于多模态描述:

* 获取一个支持多模态的模型,该模型经过训练可以同时处理文本和图像提示。
* 还要获取模型的多模态投影。这些权重允许模型理解输入的文本和图像部分如何相互关联。
* 在 KoboldCpp 启动 GUI 或命令行界面中加载模型和投影。

原始和经典的本地多模态模型是 LLaVA。GGUF 格式的模型和投影文件可从 [Mozilla/llava-v1.5-7b-llamafile](https://huggingface.co/Mozilla/llava-v1.5-7b-llamafile) 获得。要从命令行加载它们,请使用 `--model` 和 `--mmproj` 标志设置模型和投影。例如:

```shell
./koboldcpp \
--model="models/llava-v1.5-7b-Q4_K.gguf" \
--mmproj="models/ llava-v1.5-7b-mmproj-Q4_0.gguf" \
... 其他标志 ...
```

您可以尝试一些 LLaVA 微调: [xtuner/llava-llama-3-8b-v1_1-gguf](https://huggingface.co/xtuner/llava-llama-3-8b-v1_1-gguf)、[xtuner/llava-phi-3-mini-gguf](https://huggingface.co/xtuner/llava-phi-3-mini-gguf)。

您可以为您的特定微调所基于的基础模型使用多模态投影。一些常见基础模型的投影可从 [koboldcpp/mmproj](https://huggingface.co/koboldcpp/mmproj/tree/main) 获得。
