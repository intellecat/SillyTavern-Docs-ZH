---
route: /extensions/captioning/
templating: false
---


# Image Captioning (图像描述生成)

图像描述生成允许 SillyTavern 自动为聊天中使用的图像生成文本描述。

当您希望您的 AI 角色能够"看到"并回应对话中的视觉内容时，请使用图像描述生成功能。

- 为您上传或粘贴到消息中的图像创建描述
- 为聊天历史中的现有图像添加上下文
- 使用各种生成源，包括本地模型、云 API 和众包网络

有些选项不需要设置、不需要费用，也不需要 GPU。也有一些选项需要其中的一些或全部条件。选择适合您需求和资源的选项。

图像描述生成扩展已内置于 SillyTavern 中，无需单独安装。

## 快速开始

1. 设置：
    - 在 **<i class="fa-solid fa-cubes"></i> Extensions** 面板中打开 **Image Captioning** 面板
    - 选择描述生成源（最可能是"Local"或"Multimodal"）
    - 对于"Multimodal"，确保您已在 **<i class="fa-solid fa-plug"></i> API Connections** 标签页中设置了连接
2. 生成描述：
    - 从 **<i class="fa-solid fa-magic-wand-sparkles"></i> Extensions** 弹出菜单中选择"**Generate Caption**"
    - 在提示时选择图像文件
    - 等待生成描述
3. 审查并发送：
    - 带描述的图像将被插入到您的消息中
    - 使用图像工具提示查看描述
    - 点击 **<i class="fa-solid fa-paper-plane"></i> Send** 看看您的角色对图像的看法！

## 面板控制

### 源选择

选择图像描述生成的源。支持的选项：

| 源                               | 描述                                                                                                                                                                                                  |
|----------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [多模态模型](#多模态源) | **云端**：OpenAI、Anthropic、Google、MistralAI 等。<br>**本地**：Ollama、llama.cpp、KoboldCpp、Text Generation WebUI 和 vLLM。<br>支持自定义提示，因此您可以向图像提问。 |
| [Local](#本地源)           | 使用在您的 SillyTavern 服务器内本地运行的 [transformers.js](https://huggingface.co/docs/transformers.js/en/index)。零设置！                                                                     |
| Horde                            | 使用 [AI Horde](https://aihorde.net/) 网络，这是一个众包分布式图像生成模型网络。无需下载、配置或付费。响应时间不固定。                       |
| Extras                           | Extras 项目已于 2024 年 4 月停止维护，不再提供维护或支持。                                                                                                                        |

### 描述配置
- **Caption Prompt**：输入自定义描述提示。默认提示是"What's in this image?"
- **Ask every time**：切换是否为每个图像描述请求自定义提示

### 消息模板
- **Message Template**：自定义描述消息模板。使用 `{{caption}}` 宏插入生成的描述。默认模板是 `[{{user}} sends {{char}} a picture that contains: {{caption}}]`

### 自动描述生成
- **Automatically caption images**：切换是否为粘贴或附加到消息中的图像自动生成描述
- **Edit captions before saving**：切换是否允许在保存前编辑描述

## 为图像生成描述

在 SillyTavern 中生成图像描述的所有方法：

* 从 **<i class="fa-solid fa-magic-wand-sparkles"></i> Extensions** 弹出菜单中选择"**Generate Caption**"，并在提示时选择图像文件
* 点击消息中已有图像顶部的 <i class="fa-solid fa-envelope-open-text"></i> **Caption** 图标
* 在启用[自动描述生成](#自动描述生成)的情况下直接将图像粘贴到聊天输入框中
* 使用消息操作中的 <i class="fa-solid fa-paperclip"></i> **Embed File or Image** 按钮将图像文件附加到消息中
* 发送带有嵌入图像的消息
* 使用 `/caption` [斜杠命令](#斜杠命令caption)

## 自动描述生成
自动描述生成功能允许您在将图像添加到聊天中时自动生成描述，无需每次手动触发描述生成过程。

要启用，请在图像描述生成面板中选中"Automatically caption images"复选框。您还可以通过选中"Edit captions before saving"框来选择在保存前编辑描述。

启用后，自动描述生成将在以下情况下触发：

- 当图像直接粘贴到聊天输入框中时。
- 当图像文件附加到消息中时。
- 当发送带有嵌入图像的消息时。

系统将使用您选择的描述生成源（Local、Extras、Horde 或 Multimodal）和配置的设置为图像生成描述。

### 保存前编辑描述（优化模式）

如果您启用了"Edit captions before saving"选项：
1. 添加图像后，将出现一个带有生成描述的弹出窗口。
2. 您可以根据需要查看和编辑描述。
3. 点击"OK"应用描述，或点击"Cancel"放弃保存描述。

### 描述发送
生成的（和可选编辑的）描述将使用您配置的消息模板自动插入到提示中。默认情况下，它将以以下格式发送：

```
[BaronVonUser sends Seraphina a picture that contains: ...]
```


## 斜杠命令：/caption
该扩展提供了一个 `/caption` 斜杠命令，可在聊天框或脚本中使用。

### 用法

```
/caption [quiet=true|false]? [mesId=number]? [prompt]
```

- `prompt`（可选）：描述模型的自定义提示。仅 multimodal 源支持。
- `quiet=true|false`：如果设置为 true，则禁止向聊天发送带描述的消息。默认为 false。
- `mesId=number`：指定从现有消息生成图像描述的消息 ID，而不是上传新图像。

如果未提供 `mesId`，命令将提示您上传图像。当 `quiet` 为 false（默认）时，将向聊天发送一条带有描述图像的新消息。生成的描述可以用作其他命令的输入。

### 示例
使用默认设置为新图像生成描述：

```
/caption
```

使用自定义提示为新图像生成描述：

```
/caption Describe the main colours and shapes in this image
```

为消息 #5 中的图像生成描述而不发送新消息：

```
/caption mesId=5 quiet=true
```

为消息 #10 中的图像使用自定义提示生成描述，然后[基于描述生成新图像](/extensions/Stable-Diffusion.md)：

```
/caption mesId=10 Describe this image using comma-separated keywords | /imagine 
```

## 本地源

您可以在 [config.yaml](/Administration/config-yaml.md#扩展配置) 中更改模型。键名为 `extras.captioningModel`（因为某些原因）。输入您想使用的 Hugging Face 模型 ID。默认值是 `Xenova/vit-gpt2-image-captioning`。

您可以使用任何支持图像描述生成的模型（`VisionEncoderDecoderModel` 或"image-to-text"管道）。该模型需要与 transformers.js 库兼容。也就是说，它需要 ONNX 权重。寻找带有 `ONNX` 和 `image-to-text` 标签的模型，或者有一个名为 `onnx` 的文件夹，里面装满了 `.onnx` 文件。

## 多模态源

### 一般配置

- **Model**：选择用于图像描述生成的模型。选项根据所选 API 而变化。
- **Allow reverse proxy**：如果定义并有效，切换是否允许使用反向代理（OpenAI、Anthropic、Google、Mistral）

描述生成源的 API 密钥和端点 URL 在 [API Connections](/Usage/API_Connections/index.md) 面板中管理。首先在 API Connections 中设置连接，然后在 Captioning 中选择它作为您的描述源。

!!! warning 首先在 API Connections 面板中设置
最后一次提醒：在 **<i class="fa-solid fa-plug"></i> API Connections** 中配置 API 密钥/地址/端口，并在 Captioning 中使用该连接。

您仍然可以使用 Claude 进行聊天，使用 Google AI Studio 进行图像描述生成，或者其他组合。只需要先在"API Connections"标签页中设置好它们*两个*。然后将您的 Chat Completion 源切换到 Claude，将您的 Captioning 源切换到 Google AI Studio。
!!!

对于大多数本地后端，您需要在模型后端而不是在 SillyTavern 中设置一些选项。如果您的后端一次只能运行一个模型，并且不支持自动切换，那么使用同一个后端进行聊天和描述生成时使用不同的模型将会很困难。

即使您在不同端口上运行两个后端实例，API Connections 也只允许每种后端类型有一个活动配置。但是如果我告诉您...您可能可以在 Text Completion 和 Chat Completion 模式下都连接到您的后端？现在您可以有两个到同一个后端类型的连接了。

### 源

要使用这些描述源之一，请在源下拉菜单中选择 Multimodal。

* "我想要最好的描述生成，不介意付费"：Anthropic
* "我不想付费或运行任何东西"：Google AI Studio 免费层
* "我想在本地生成图像描述并让它正常工作"：Ollama
* "我想保持本地 AI 的梦想"：[KoboldCpp](#koboldcpp)
* "我想在它不工作时抱怨"：~~Extras~~

| API 提供商                        | 描述                                                                                                                                                                   |
|-----------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 01.AI (Yi)                        | 云端，付费，yi-vision                                                                                                                                                        |
| Anthropic                         | 云端，付费，具有视觉能力的 Claude 模型：claude-3-5-sonnet/haiku、claude-3-opus/sonnet                                                                                        |
| Custom (OpenAI-compatible)        | 用于自定义 OpenAI 兼容 API，使用 API Connections 标签页中当前配置的模型                                                                                                     |
| Google AI Studio                  | 云端，免费层然后付费，Gemini Flash/Pro                                                                                                                                      |
| Groq                              | 云端，llama-3.2-vision 11B/90B，LLaVA                                                                                                                                     |
| KoboldCpp                         | 本地，必须在 KoboldCpp 中配置模型                                                                                                                                      |
| llama.cpp                         | 本地，必须在 llama.cpp 中配置模型                                                                                                                                      |
| MistralAI                         | 云端，付费，pixtral-large、pixtral-12B                                                                                                                                       |
| Ollama                            | 本地，在 API Connections 中配置后可以在 Captioning 中切换可用模型并下载[额外的视觉模型](https://ollama.com/search?c=vision)                                                |
| OpenAI                            | 云端，付费，GPT-4 Vision、4-turbo、4o、4o-mini                                                                                                                               |
| OpenRouter                        | 云端，付费（可能有免费选项），多个模型，在 API Connections 中配置后从 Captioning 中可用的模型中选择                                                                          |
| Text Generation WebUI (oobabooga) | 本地，必须在 ooba 中配置模型                                                                                                                                           |
| vLLM                              | 本地                                                                                                                                                                         |

### KoboldCpp

有关安装和使用 [KoboldCpp](https://github.com/LostRuins/koboldcpp) 的一般信息，请参阅 [KoboldCpp 文档](https://github.com/LostRuins/koboldcpp/wiki)。

要使用 KoboldCpp 进行多模态描述生成：

* 获取一个多模态模型，该模型经过训练可以同时处理文本和图像提示。
* 还要获取该模型的多模态投影。这些权重允许模型理解输入的文本和图像部分之间的关系。
* 在 KoboldCpp 启动 GUI 或命令行界面中加载模型和投影。

原始和经典的本地多模态模型是 LLaVA。可以从 [Mozilla/llava-v1.5-7b-llamafile](https://huggingface.co/Mozilla/llava-v1.5-7b-llamafile) 获取 GGUF 格式的模型和投影文件。要从命令行加载它们，请使用 `--model` 和 `--mmproj` 标志设置模型和投影。例如：

```shell
./koboldcpp \
--model="models/llava-v1.5-7b-Q4_K.gguf" \
--mmproj="models/ llava-v1.5-7b-mmproj-Q4_0.gguf" \
... other flags ...
```

您可以尝试一些 LLaVA 微调版本：[xtuner/llava-llama-3-8b-v1_1-gguf](https://huggingface.co/xtuner/llava-llama-3-8b-v1_1-gguf)、[xtuner/llava-phi-3-mini-gguf](https://huggingface.co/xtuner/llava-phi-3-mini-gguf)。

您可以使用您特定微调版本所基于的基础模型的多模态投影。一些常见基础模型的投影可以从 [koboldcpp/mmproj](https://huggingface.co/koboldcpp/mmproj/tree/main) 获取。
