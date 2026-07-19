---
route: /extensions/captioning/
templating: false
---

# Captioning（图像描述）

图像描述功能允许 SillyTavern 自动为聊天中使用的图像生成文字描述。

当您希望 AI 角色能够"看到"并响应对话中的视觉内容时，请使用图像描述功能。

- 为您上传或粘贴到消息中的图像创建描述
- 为聊天历史中的现有图像添加上下文
- 使用各种生成来源，包括本地模型、云端 API 和众包网络

有些选项无需任何设置、无需花钱、无需 GPU。也有些选项需要其中部分或全部条件。请根据您的需求和资源选择合适的选项。

图像描述扩展内置于 SillyTavern 中，无需单独安装。

## 快速开始

1. 设置：
    - 在 **<i class="fa-solid fa-cubes"></i> 扩展**面板中打开**图像描述**面板
    - 选择一个描述来源（最可能是"本地"或"多模态"）
    - 对于"多模态"，请确保您已在 **<i class="fa-solid fa-plug"></i> API 连接**标签页中设置好连接
2. 生成描述：
    - 从 **<i class="fa-solid fa-magic-wand-sparkles"></i> 扩展**弹出菜单中选择"**生成描述**"
    - 在提示时选择图像文件
    - 等待描述生成
3. 审阅并发送：
    - 带描述的图像将被插入您的消息中
    - 通过图像提示查看描述内容
    - 单击 **<i class="fa-solid fa-paper-plane"></i> 发送**，看看您的角色对图像有何看法！

## 面板控件

### 来源选择

选择图像描述的来源。支持的选项：

| 来源                           | 描述                                                                                                                                                                                                  |
|----------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [多模态](#multimodal-source) | **云端**：OpenAI、Anthropic、Google、MistralAI 等。<br>**本地**：Ollama、llama.cpp、KoboldCpp、Text Generation WebUI 和 vLLM。<br>支持自定义提示词，可以对图像进行提问。 |
| [本地](#local-source)           | 使用在 SillyTavern 服务器内本地运行的 [transformers.js](https://huggingface.co/docs/transformers.js/en/index)。零设置！                                                                     |
| Horde                            | 使用 [AI Horde](https://aihorde.net/) 网络，这是一个众包分布式图像生成模型网络。无需下载、配置或付费。响应时间不定。                       |
| Extras                           | Extras 项目已于 2024 年 4 月停止维护，不再受到维护或支持。                                                                                                                                        |

### 描述配置
- **描述提示词**：为图像描述输入自定义提示词。默认提示词为"What's in this image?"
- **每次询问**：切换为每次图像描述请求自定义提示词

### 消息模板
- **消息模板**：自定义描述消息模板。使用 `{{caption}}` 宏插入生成的描述。默认模板为 `[{{user}} sends {{char}} a picture that contains: {{caption}}]`

### 自动描述
- **自动为图像生成描述**：切换以启用自动为粘贴或附加到消息的图像生成描述
- **保存前编辑描述**：切换以允许在保存前编辑描述

## 为图像生成描述

SillyTavern 中所有为图像生成描述的方式：

* 从 **<i class="fa-solid fa-magic-wand-sparkles"></i> 扩展**弹出菜单中选择"**生成描述**"，并在提示时选择图像文件
* 单击消息中已有图像顶部的 <i class="fa-solid fa-envelope-open-text"></i> **描述**图标
* 在启用[自动描述](#auto-captioning)的情况下，直接将图像粘贴到聊天输入框中
* 使用消息操作中的 <i class="fa-solid fa-paperclip"></i> **嵌入文件或图像**按钮，将图像文件附加到消息中
* 发送带有嵌入图像的消息
* 使用 `/caption` [斜杠命令](#slash-command-caption)

## 自动描述 {#auto-captioning}
自动描述功能允许您在图像添加到聊天时自动生成描述，无需每次手动触发描述过程。

要启用，请在图像描述面板中选中"自动为图像生成描述"复选框。您也可以通过勾选"保存前编辑描述"框，选择在保存前编辑描述。

启用后，自动描述将在以下情况下触发：

- 当图像直接粘贴到聊天输入框时。
- 当图像文件附加到消息时。
- 当发送带有嵌入图像的消息时。

系统将使用您选择的描述来源（本地、Extras、Horde 或多模态）和配置的设置为图像生成描述。

### 保存前编辑描述（精细化模式）

如果您已启用"保存前编辑描述"选项：
1. 添加图像后，将出现一个包含生成描述的弹出窗口。
2. 您可以根据需要审阅和编辑描述。
3. 单击"确定"应用描述，或单击"取消"放弃描述而不保存。

### 描述发送
生成的（可选编辑过的）描述将使用您配置的消息模板自动插入提示词中。默认情况下，它将以以下格式发送：

```
[BaronVonUser sends Seraphina a picture that contains: ...]
```


## 斜杠命令：/caption {#slash-command-caption}
该扩展提供了一个 `/caption` 斜杠命令，可在聊天框或脚本中使用。

### 用法

```
/caption [quiet=true|false]? [mesId=number]? [prompt]
```

- `prompt`（可选）：图像描述模型的自定义提示词。仅多模态来源支持。
- `quiet=true|false`：如果设置为 true，则抑制将带描述的消息发送到聊天。默认为 false。
- `mesId=number`：指定消息 ID，以对现有消息中的图像进行描述，而不是上传新图像。

如果未提供 `mesId`，命令将提示您上传图像。当 `quiet` 为 false（默认）时，包含带描述图像的新消息将被发送到聊天。生成的描述可用作其他命令的输入。

### 示例
使用默认设置为新图像生成描述：

```
/caption
```

使用自定义提示词为新图像生成描述：

```
/caption Describe the main colours and shapes in this image
```

对第 5 条消息中的图像生成描述而不发送新消息：

```
/caption mesId=5 quiet=true
```

对第 10 条消息中的图像生成描述，然后根据描述[生成新图像](/extensions/Stable-Diffusion.md)：

```
/caption mesId=10 Describe this image using comma-separated keywords | /imagine 
```

## 本地来源 {#local-source}

您可以在 [config.yaml](/Administration/config-yaml.md#extensions-configuration) 中更改模型。键名为 `extensions.models.captioning`。输入您想使用的 Hugging Face 模型 ID。默认为 `Xenova/vit-gpt2-image-captioning`。

您可以使用任何支持图像描述的模型（`VisionEncoderDecoderModel` 或"image-to-text"流水线）。该模型需要与 transformers.js 库兼容，即需要 ONNX 权重。请查找带有 `ONNX` 和 `image-to-text` 标签的模型，或者具有包含 `.onnx` 文件的 `onnx` 文件夹的模型。

## 多模态来源 {#multimodal-source}

### 通用配置

- **模型**：选择用于图像描述的模型。选项因所选 API 而异。
- **允许反向代理**：切换以允许在已定义且有效时使用反向代理（OpenAI、Anthropic、Google、Mistral、xAI）

描述来源的 API 密钥和端点 URL 在 [API 连接](/Usage/API_Connections/index.md)面板中管理。请先在"API 连接"中设置连接，然后在"图像描述"中将其选为描述来源。

!!!warning 请先在 API 连接面板中设置
再次提醒：在 **<i class="fa-solid fa-plug"></i> API 连接**中配置 API 密钥/地址/端口，然后在"图像描述"中使用该连接。

您完全可以使用 Claude 进行聊天，同时使用 Google AI Studio 进行图像描述，诸如此类。只需先在"API 连接"标签页中*分别*设置好它们，然后将您的聊天补全来源切换到 Claude，将图像描述来源切换到 Google AI Studio。
!!!

对于大多数本地后端，您需要在模型后端而不是在 SillyTavern 中设置一些选项。如果您的后端一次只能运行一个模型且不支持自动切换，您有以下几种选项来对聊天和图像描述使用不同的模型：

1. **辅助端点：**使用辅助端点功能（参见下方[辅助端点](#secondary-endpoints)部分）连接到不同的 API 服务器进行图像描述
2. **多种连接类型：**在"API 连接"中使用文本补全和聊天补全两种模式连接到您的后端——这样可以对同一后端类型建立两个独立的连接

### 来源

要使用以下某个描述来源，请在"来源"下拉菜单中选择"多模态"。

* "我想要最佳的图像描述效果，不介意付费"：Anthropic
* "我不想付钱也不想运行任何东西"：Google AI Studio 免费层
* "我想在本地为图像生成描述且开箱即用"：Ollama
* "我想保持本地 AI 的梦想"：[KoboldCpp](#koboldcpp)
* "我想在不工作时抱怨"：~~Extras~~

| API 提供商                      | 描述                                                                                                                                                                   |
|-----------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| AI/ML API                         | 云端，付费，多种具有视觉能力的 GPT、Claude 和 Gemini 模型                                                                                                                  |
| Chutes                            | 云端，多种具有视觉能力的模型                                                                                                                                |
| Claude                            | 云端，付费，所有具有视觉能力的 Claude 模型                                                                                                                       |
| Cloudflare Workers AI             | 云端，付费，多种具有视觉能力的模型                                                                                                                          |
| Cohere                            | 云端，付费，Aya Vision 8B / 32B                                                                                                                                              |
| Custom (OpenAI-compatible)        | 用于自定义兼容 OpenAI 的 API，使用在"API 连接"标签页中当前配置的模型                                                                                     |
| Electron Hub                      | 云端，付费，多种具有视觉能力的模型。                                                                                                                         |
| Google AI Studio                  | 云端，免费层后付费，Gemini Flash/Pro                                                                                                                                  |
| Google Vertex AI                  | 云端，免费层，Gemini Flash/Pro                                                                                                                                            |
| Groq                              | 云端，llama-4 scout/maverick                                                                                                                                                 |
| KoboldCpp                         | 本地，需在 KoboldCpp 中配置模型                                                                                                                                      |
| llama.cpp                         | 本地，需在 llama.cpp 中配置模型                                                                                                                                           |
| MistralAI                         | 云端，付费，pixtral-large、pixtral-12B、magistral、mistral-large 等。                                                                                                       |
| Moonshot AI                       | 云端，付费，moonshot-vision                                                                                                                                                  |
| NanoGPT                           | 云端，付费，多种具有视觉能力的 GPT/Claude/Google 模型                                                                                                                        |
| Ollama                            | 本地，可在"图像描述"中切换可用模型并下载[额外的视觉模型](https://ollama.com/search?c=vision)（在"API 连接"中配置后）|
| OpenAI                            | 云端，付费，GPT-4 Vision、4-turbo、4o、4o-mini                                                                                                                               |
| OpenRouter                        | 云端，付费（可能有免费选项），众多模型，在"API 连接"中配置后可在"图像描述"中从可用模型中选择                                              |
| Pollinations                      | 云端，免费                                                                                                                                                                   |
| Text Generation WebUI (oobabooga) | 本地，需在 ooba 中配置模型                                                                                                                                           |
| vLLM                              | 本地                                                                                                                                                                         |
| xAI (Grok)                        | 云端，付费，grok-vision                                                                                                                                                      |
| Z.AI (GLM)                        | 云端，付费，GLM 视觉模型                                                                                                                                                |

### 辅助端点 {#secondary-endpoints}

默认情况下，多模态来源使用在"API 连接"标签页中配置的主端点。
您也可以专门为多模态图像描述设置辅助端点。

- 在 **<i class="fa-solid fa-cubes"></i> 扩展**面板中打开**图像描述**面板。
- 选择"多模态"作为描述来源，并选择首选的 API 提供商。
- 在"辅助描述端点 URL"字段中输入有效的 URL 作为辅助端点。
- 勾选"使用辅助 URL"框以启用辅助端点。

!!!tip
请勿在 URL 末尾添加 `/v1` 或 `/chat/completions`。扩展将自动处理这部分。
!!!

仅以下 API 支持此功能：

- KoboldCpp
- llama.cpp
- Ollama
- Text Generation WebUI (oobabooga)
- vLLM

### 特定来源指南

#### KoboldCpp

有关安装和使用 [KoboldCpp](https://github.com/LostRuins/koboldcpp) 的一般信息，请参阅 [KoboldCpp 文档](https://github.com/LostRuins/koboldcpp/wiki)。

要使用 KoboldCpp 进行多模态图像描述：

* 获取一个多模态兼容模型，该模型经过训练，可以同时处理文本和图像提示词。
* 同时获取该模型的多模态投影权重。这些权重使模型能够理解输入的文本和图像部分之间的关系。
* 在 KoboldCpp 启动 GUI 或命令行界面中加载模型和投影权重。

最初的经典本地多模态模型是 LLaVA。该模型和投影权重的 GGUF 格式文件可从 [Mozilla/llava-v1.5-7b-llamafile](https://huggingface.co/Mozilla/llava-v1.5-7b-llamafile) 获取。要从命令行加载它们，请使用 `--model` 和 `--mmproj` 标志指定模型和投影权重。例如：

```shell
./koboldcpp \
--model="models/llava-v1.5-7b-Q4_K.gguf" \
--mmproj="models/ llava-v1.5-7b-mmproj-Q4_0.gguf" \
... other flags ...
```

您可以尝试的一些 LLaVA 微调版本：[xtuner/llava-llama-3-8b-v1_1-gguf](https://huggingface.co/xtuner/llava-llama-3-8b-v1_1-gguf)、[xtuner/llava-phi-3-mini-gguf](https://huggingface.co/xtuner/llava-phi-3-mini-gguf)。

您可以使用您特定微调版本所基于的基础模型的多模态投影权重。部分常见基础模型的投影权重可从 [koboldcpp/mmproj](https://huggingface.co/koboldcpp/mmproj/tree/main) 获取。
