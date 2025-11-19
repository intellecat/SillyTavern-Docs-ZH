---
route: /extensions/stable-diffusion/
templating: false
---

# Image Generation

使用本地或基于云的 Stable Diffusion、FLUX 或 DALL-E API 生成图像。

自动生成图像作为您消息的回复以获得完全沉浸感,从 wand 菜单或 slash commands 使用聊天历史和角色信息生成图像,或在聊天输入栏中使用 `/sd (anything_here)` 命令使用您自己的 prompt 生成图像。

大多数常用的 Stable Diffusion 生成设置都可以在 SillyTavern UI 中自定义。

- 支持[多种图像生成源](#supported-sources),包括本地和基于云的
- 各种[生成模式](#generation-modes)用于角色、场景和自定义 prompts
- [Slash commands](#how-to-generate-an-image) 用于在聊天中轻松生成图像
- [Interactive mode](#use-interactive-mode) 可根据自然语言请求触发图像生成
- 可自定义的 prompt 模板和[前缀](#common-prompt-prefix)以保持一致的风格和质量
- [角色特定 prompt 前缀](#character-specific-prompt-prefix)用于定制角色图像
- [风格预设](#styles)可快速切换不同的图像生成设置
- 灵活的[可见性选项](#chat-message-visibility)用于聊天中生成的图像
- 高级 [ComfyUI 集成](#comfyui-configuration)用于高度可自定义的工作流程
- 能够[查看所有生成的图像](#view-all-generated-images)在角色画廊中
- [图像 swipes](#image-swipes) 功能可以重新生成图像同时保持相同的 prompt
- 选项可以[在生成前编辑 prompts](#edit-prompts-before-generation) 和[扩展 free-mode prompts](#extend-free-mode-prompts)
- 与 AI [function calling](#use-function-tool) 集成以自动检测图像生成意图

## 支持的来源

| Source                                                                                            | 备注                                                                                         |
|:--------------------------------------------------------------------------------------------------|:------------------------------------------------------------------------------------------------|
| [AI.ML API](https://aimlapi.com/)                                                                 | 云端,付费                                                                                     |
| [Black Forest Labs](https://bfl.ai/)                                                              | 云端,付费                                                                                     |
| [ComfyUI](https://github.com/comfyanonymous/ComfyUI)                                              | 本地,开源 (GPL3),免费,参见 [ComfyUI Configuration](#comfyui-configuration)。 |
| [Draw Things](https://drawthings.ai/)                                                             | 本地,Mac/iOS,免费                                                                  |
| [Electron Hub](https://electronhub.ai/)                                                           | 云端,付费                                                                                     |
| [FAL.AI](https://fal.ai/)                                                                         | 云端,付费                                                                                     |
| [Google AI Studio](https://aistudio.google.com/) / [Google Vertex AI](https://cloud.google.com/vertex-ai) | 云端,付费。Imagen 模型系列。AI Studio 仅支持 Imagen 3.0 002 模型。         |
| [HuggingFace Serverless](https://huggingface.co/docs/api-inference/index)                         | 云端,免费                                                                           |
| [NanoGPT](https://nano-gpt.com/)                                                                  | 云端,付费                                                                                     |
| [NovelAI Diffusion](https://novelai.net/)                                                         | 云端,需要有效订阅                                                          |
| [OpenAI](https://platform.openai.com/)                                                            | 云端,付费                                                                                     |
| [Pollinations](https://pollinations.ai/)                                                          | 云端,开源 (MIT),免费                                                        |
| [SD.Next / vladmandic](https://github.com/vladmandic/automatic)                                   | 本地,开源 (AGPL3),免费                                                      |
| [SillyTavern Extras](https://github.com/SillyTavern/SillyTavern-Extras)                           | 已弃用,不推荐                                                                     |
| [Stability AI](https://platform.stability.ai/)                                                    | 云端,付费                                                                     |
| [Stable Diffusion WebUI / AUTOMATIC1111](https://github.com/AUTOMATIC1111/stable-diffusion-webui) | 本地,开源 (AGPL3),免费                                                      |
| [Stable Horde](https://stablehorde.net/)                                                          | 云端,开源 (AGPL3),免费                                                      |
| [TogetherAI](https://docs.together.ai/docs/serverless-models#image-models)                        | 云端                                                                           |
| [x.AI](https://x.ai/)                                                                             | 云端,付费                                                                                     |

## 生成模式

| Wand 菜单项     | Slash command 参数 | 描述                                    | 备注                               |
|:-------------------|:-----------------------|:-----------------------------------------------|:--------------------------------------|
| "Yourself"         | `you`                  | 当前角色的全身肖像。 | -                                     |
| "Your Face"        | `face`                 | 当前角色的特写肖像。  | 强制使用肖像纵横比。       |
| "Me"               | `me`                   | 用户角色的肖像。                | -                                     |
| "The Whole Story"  | `scene`                | 聊天事件的视觉回顾。             | -                                     |
| "The Last Message" | `last`                 | 最后一条聊天消息的视觉回顾。       | -                                     |
| "Raw Last Message" | `raw_last`             | 直接使用最后一条消息作为 prompt。        | -                                     |
| "Background"       | `background`           | 基于故事背景的聊天背景。      | 强制使用宽景观纵横比。 |

## 如何生成图像

1. 使用 extensions context menu (wand) 中的 "Image Generation" 项。
2. 输入带有 Generation modes 表中参数的 `/sd (argument)` slash command。其他任何内容都会触发 "free mode" 让 SD 生成您提示的任何内容。例如:`/sd apple tree` 会生成一棵苹果树的图片。
3. 查找聊天消息 context actions 中的画笔图标。这将为所选消息强制使用 "Raw Message" mode。

除了 raw message 和 free mode 之外的每种生成模式都会使用您当前选择的主要生成 API 触发 prompt 生成,将聊天上下文转换为 SD prompt。
您可以使用 extensions 面板中的 "SD Prompt Templates" settings drawer 为每种生成模式配置指令模板。

### Tips and tricks for the `/sd` command usage

#### View all generated images

要查看角色的所有保存图像(包括其他聊天),请从角色信息面板的 "More..." 下拉菜单打开画廊,或使用 `/show-gallery` slash command。

#### Specify a negative prompt

在 prompt 之前使用 `negative` 命名参数来强制使用特定的 negative prompt 进行生成。

```stscript
/sd negative="fries" cute tater farmer holding a tayto in a spud-field
```

#### Include a character-specific prefix

在 free-prompt mode 下使用特殊的 `{{charPrefix}}` macro 来包含当前角色的 positive 和 negative prompt 前缀(如果已定义)。

```stscript
/sd {{charPrefix}}, riding a bike
```

#### Suppress a chat message

您可以通过传递 `quiet=true` 命名参数来避免将生成的图像发布到聊天中。图像仍会添加到角色画廊中,命令将生成图像的相对 URL,可供其他命令使用。

下面的示例将使用 Markdown 作为用户角色发送生成的图像。

```stscript
/sd quiet=true me | /send Here's a picture of me: ![my portrait]({{pipe}})
```

### Image swipes

Images swipes 允许在保持相同 prompt 的同时重新生成图像。如果设置了固定 seed,它将为下一次生成随机化。

要在图像之间循环,将鼠标光标悬停(移动端点击)在生成的图像上以显示箭头按钮和 swipes 计数器。点击最新图像上的右箭头将生成新图像。

*'Swipes' 只是一个名称,不要尝试实际的滑动手势,因为这会重新生成消息本身,而不是附加的图像。*

## 选项

### Edit prompts before generation

允许在将自动生成的 prompts 发送到 Stable Diffusion API 之前手动编辑它们。

### Use function tool

使用 [function calling](/extensions/Stable-Diffusion.md) 自动检测生成图像的意图。

**要求:**

1. 必须配置支持的 source 的图像生成。
2. 必须使用支持的 Chat Completion API 模型,并在 AI Response settings 中启用 function tool calling。
3. 必须在 Image Generation settings 中启用 "Use function tool" 选项。
4. 用户应该在聊天消息中表达生成图像的意图,例如 "Send me a picture of a cat"。

!!!warning
启用 function tool 时不会触发 interactive mode。
!!!

### Use interactive mode

允许触发图像生成而不是文本作为对遵循特殊模式的用户消息的回复:

1. 包含以下动词之一:send、mail、imagine、generate、make、create、draw、paint、render
2. 后跟以下名词之一(不超过 10 个字符):pic、picture、image、drawing、painting、photo、photograph
3. 后跟图像生成的目标主题,可以选择性地在前面加上 "of a" 或 "of this" 等短语。

有效请求和捕获主题的示例:

* `Can you please send me a picture of a cat` => `cat`
* `Generate a picture of the Eiffel tower` => `Eiffel tower`
* `Let's draw a painting of Mona Lisa` => `Mona Lisa`

一些特殊主题会触发预定义的生成模式:

* 'you, 'yourself' => "Yourself"
* 'your face', 'your portrait', 'your selfie' => "Your Face"
* 'me', 'myself' => "Me"
* 'story', 'scenario', 'whole story' => "The Whole Story"
* 'last message' => "The Last Message"
* 'background', 'scene background', 'scene', 'scenery', 'surroundings', 'environment' => "Background"

### Extend free-mode prompts

当使用 interactive mode 或 slash command 时,通过提示您的主 API 自动扩展 free-mode 生成主题描述。

### Snap auto-adjusted resolutions

将具有强制纵横比(肖像、背景)的图像生成请求调整到最接近的已知分辨率,同时尝试保留绝对像素数。请参阅 "Resolution" 下拉菜单以获取可能选项的列表。

**推荐用于 SDXL 模型**。

## 通用 prompt 前缀

!!!tip Pro Tip
使用 `{prompt}` macro 来指定生成的 prompt 将插入的确切位置。
!!!

添加在每个生成或 free-mode prompt 之前。通常用于设置图片的整体风格。

示例:`best quality, anime lineart`。

## Negative prompt

您不希望在输出中出现的图像特征。

示例:`bad quality, watermark`。

## 角色特定 prompt 前缀

!!!tip Pro Tip
如果生成 source 支持,您也可以在这里使用 LoRAs/embeddings,例如:`<lora:DonaldDuck:1>`。
!!!

描述当前选定角色的任何特征。将添加在 common prefix 之后。

示例:`female, green eyes, brown hair, pink shirt`。

您还可以为任何不需要的内容指定 negative prompt 前缀。它将与一般 negative prompt 结合。

限制:
1. 仅在 1 对 1 聊天中有效。不会在群组中使用。
2. 不会用于 backgrounds 和 free mode 生成。

!!! Note
要强制将角色前缀包含到 free mode prompt 中,请在 prompt 中的任何位置使用 `{{charPrefix}}` macro。
!!!

如果您想与他人分享前缀,请勾选 "Shareable" 复选框。这会将它们与角色数据一起保存,而不是您的本地设置。

## 风格

使用此功能快速保存和恢复您最喜欢的风格/质量预设,以便稍后使用或在模型之间切换时使用。Style preset 中包含以下内容:

1. Common Prompt Prefix
2. Negative Prompt

您还可以使用 `/imagine-style` 命令(或 `/sd-style` 或 `/img-style`)在 styles 之间切换。

## 聊天消息可见性

插入聊天中的生成图像在主 API prompts 中默认隐藏,但可以根据每个生成启动器("Magic wand" 图标、slash command、interactive mode)单独覆盖。这可以用于通过让角色"确认"图像来使体验更加身临其境。如果启用了 "Send inline images",Chat Completions API 中的多模态模型也可能"看到"图像。

可以通过更改 Image Prompt Templates 下的 "Chat Message Template" 来自定义文本消息。所有常规 macros 都可以在此模板中使用,还有一个特殊的 `{{prompt}}` macro 来指定图像 prompt 将添加的位置。

## ComfyUI 配置

[ComfyUI](https://github.com/comfyanonymous/ComfyUI) 是一个快速且非常灵活的图像生成选项。

如果您熟悉 ComfyUI,简而言之:在 ComfyUI 中制作工作流程,以 **API 格式**下载它,然后将其粘贴到 SillyTavern ComfyUI Workflow Editor 中。ST 将向 ComfyUI 的 API 提交您的工作流程,您将在聊天中获得图像。但能力越大,责任越大,主要责任是在工作流程 JSON 中插入占位符,以便您可以从 SillyTavern 更改设置。

如果您不熟悉 ComfyUI,您仍然可以使用默认工作流程在 SillyTavern 中生成图像。稍后,当您需要强大功能时,您可以学习如何使用 ComfyUI...

### Controls

此面板允许您配置和管理 ComfyUI 与 SillyTavern 的集成。

在 **ComfyUI URL** 输入字段中输入 ComfyUI 服务器的 URL。默认值为 `http://127.0.0.1:8188`。
如果您使用 [SwarmUI](https://github.com/mcmonkeyprojects/SwarmUI),
[managed ComfyUI server](https://github.com/mcmonkeyprojects/SwarmUI/blob/master/src/BuiltinExtensions/ComfyUIBackend/README.md) 的默认端口为 `7821`,
比 SwarmUI 的默认端口高 20 个端口。

输入 URL 后,选择 <i class="fa-solid fa-check"></i> **Connect** 以验证和建立连接。ComfyUI 服务器必须可从 SillyTavern 主机访问。

### Workflow Management

从下拉菜单中选择 ComfyUI workflow。提供两个默认 workflows:

- Default_Comfy_Workflow.json:支持最常见图像生成设置的基本 text-to-image workflow。
- Char_Avatar_Comfy_Workflow.json:使用角色头像和 prompt 生成图像的示例 image-to-image workflow。

使用以下按钮管理您的 workflows:

- <i class="fa-solid fa-pen-to-square"></i> **Open workflow editor** 查看和修改所选 workflow。
- <i class="fa-solid fa-plus"></i> **Create new workflow** 使用自定义名称创建新 workflow。
- <i class="fa-solid fa-trash-can"></i> **Delete workflow** 删除所选 workflow。

### Workflow Editor

ComfyUI Workflow Editor 允许您查看和修改用于 SillyTavern 的 ComfyUI workflows。

编辑器的主要组件是一个大型文本区域,您可以在其中以 JSON 格式插入或编辑 ComfyUI workflow。

要向编辑器添加 ComfyUI workflow,请按照以下步骤操作:

1. 在 ComfyUI settings 中启用 'Dev Mode'。
2. 使用 ComfyUI 中的 'Save (API Format)' 选项下载 JSON 数据。
3. 在 SillyTavern 中创建新 workflow 并打开编辑器。
4. 将下载的 JSON 数据粘贴到文本区域中。
5. 根据您的用例需要,将特定值替换为占位符。

!!!tip Tips
您可以直接将 API 格式的 JSON 文件添加到 SillyTavern 安装中的 `data/default-user/user/workflows` 目录。这将省去步骤 3 和 4。

保留原始 JSON 文件。如果您需要再次在 ComfyUI 中打开 workflow 进行更改,编辑原始文件比编辑所有占位符的文件要方便得多。
!!!

### Placeholders

编辑器提供了可在 workflow JSON 中使用的预定义占位符列表。这些占位符在 SillyTavern 中执行 workflow 时会被动态值替换。

标记为 ✅ 的占位符存在于您的 workflow JSON 中。标记为 ❌ 的占位符不存在于您的 workflow JSON 中。您可以根据需要将这些占位符添加到 workflow JSON 中。您不需要添加所有占位符,只需添加 workflow 使用并且您想要动态替换的占位符。

#### Prompts

`%prompt%` 和 `%negative_prompt%` 占位符用于将图像生成 prompts 插入 workflow 中。这些包含 SillyTavern 生成的最终 prompts,包括为您选择的 `/sd` 模式生成的 prompt、common prompt prefix、negative prompt 和角色特定 prompt 前缀。

例如,您可能已经在 ComfyUI 中使用 "forest elf" 之类的 prompt 测试了 workflow。要在 SillyTavern 中使用此 workflow,您可以将 "forest elf" prompt 替换为 `%prompt%` 占位符:

+++ JSON with placeholder
```json
{
    "class_type": "CLIPTextEncode",
    "inputs": {
        "clip": ["4", 1],
        "text": "%prompt%"
    }
}
```
+++ Original JSON
```json
{
    "class_type": "CLIPTextEncode",
    "inputs": {
        "clip": ["4", 1],
        "text": "forest elf"
    }
}
```
+++

请注意,占位符用双引号包裹。这对于 JSON 格式很重要,并且是 SillyTavern 的占位符替换系统所要求的。即使对于数字,您也必须在模板 JSON 中使用双引号。

有时 prompt(或其他值)不会出现在您预期的位置。如果节点对于 workflow 在 API 模式下运行不是必需的,ComfyUI 将从 workflow 的 API 版本中删除节点。

例如,此 workflow 使用带有 prompt primitive 的 [LoRA tag loader node](https://github.com/badjeff/comfyui_lora_tag_loader),以便在 UI 模式下 workflow 更清晰:

![Prompt primitive and LoRA loader](/static/extensions/sd-comfy-prompt-primitive.png)

Prompt primitive node 将从 workflow 的 API 版本中删除,因此您在 LoraTagLoader node 中插入占位符。在 workflow 中找到文本 "apple tree" 并将其替换为 `%prompt%` 占位符:

+++ JSON with placeholder
```json
{
    "inputs": {
      "text": "%prompt%",
      "model": ["112", 0],
      "clip": ["112", 1]
    },
    "class_type": "LoraTagLoader",
    "_meta": {"title": "Load LoRA Tag"}
}
```
+++ Original JSON
```json
{
    "inputs": {
      "text": "apple tree",
      "model": ["112", 0],
      "clip": ["112", 1]
    },
    "class_type": "LoraTagLoader",
    "_meta": {"title": "Load LoRA Tag"}
}
```
+++

在某些情况下,您可能需要在 workflow JSON 中进行多次替换,即使 prompt 在 UI 中只出现一次。

#### Model

`%model%` 占位符将插入 image generation settings 中所选模型的值。

来自默认 text-to-image workflow 的示例:

+++ JSON with placeholder
```json
{
    "class_type": "CheckpointLoaderSimple",
    "inputs": {
        "ckpt_name": "%model%"
    }
}
```
+++ Original JSON
```json
{
    "class_type": "CheckpointLoaderSimple",
    "inputs": {
        "ckpt_name": "sd15.safetensors"
    }
}
```
+++

要加载 GGUF 量化的 UNets,请在 workflow 中使用 [UNet Loader (GGUF)](https://github.com/city96/ComfyUI-GGUF) node,
在 SillyTavern model 下拉菜单中选择 `GGUF` 模型,并像这样在 node 的设置中使用 `%model%` 占位符:

+++ JSON with placeholder
```json
{
    "inputs": {
      "unet_name": "%model%"
    },
    "class_type": "UnetLoaderGGUF",
    "_meta": {
      "title": "Unet Loader (GGUF)"
    }
}
```
+++ Original JSON
```json
{
    "inputs": {
      "unet_name": "flux1-dev-Q4_0.gguf"
    },
    "class_type": "UnetLoaderGGUF",
    "_meta": {
      "title": "Unet Loader (GGUF)"
    }
}
```
+++

!!!info 如果您在 ComfyUI 中有常规 SD checkpoints 以外的模型类型
Stable Diffusion checkpoints、SD UNets 和 GGUF 量化的 UNets 都出现在 Model 下拉菜单中。
一种类型的模型不能与期望另一种类型的 workflows/loader nodes 一起使用。
如果您在 ST 中选择不兼容的模型类型,ComfyUI 将报告 loader node 存在问题。
!!!

#### Avatar images

使用 `%user_avatar%` 和 `%char_avatar%` 占位符将用户和角色头像包含在 workflow 中。这些占位符在执行 workflow 时会被头像的 PNG 数据替换。图像数据以 base64 格式编码,因此您必须在 workflow 中对其进行解码。此任务的常见选择是 [Load image (Base64)](https://github.com/Acly/comfyui-tooling-nodes) node。

在此示例中,使用 `Load Image (Base64)` node 加载角色头像。它还使用 Image Resize node 将图像调整为 image generation settings 中指定的任何大小:

![Load image from base64 string and resize](/static/extensions/sd-comfy-load-b64.png)

将 `%char_avatar%`、`%width%` 和 `%height%` 占位符插入 Load Image (Base64) 和 Image Resize nodes 的 JSON 中:

```json
{
    "97": {
        "inputs": {
            "image": "%char_avatar%"
        },
        "class_type": "ETN_LoadImageBase64",
        "_meta": {"title": "Load Image (Base64)"}
    },
    "98": {
        "inputs": {
            "mode": "resize",
            "resize_width": "%width%",
            "resize_height": "%height%",
            "image": ["97", 0]
        },
        "class_type": "Image Resize",
        "_meta": {"title": "Resize image"}
    }
}
```

要获取 base64 编码的图像字符串以在 ComfyUI 中测试 workflow,请使用任何将图像转换为 base64 字符串的在线工具。
这是一个可用于初始测试的示例字符串:[sd-comfy-base64-test-string.txt](/static/extensions/sd-comfy-base64-test-string.txt)。

#### Other placeholders

大多数其他占位符使用 image generation settings 中相应控件的值,或您使用 `/sd` 命令指定的值:

- `%vae%`,但大多数 SD 模型包含 VAE,因此默认 workflows 不使用此占位符。使用它来加载 VAE 与 UNet 一起使用、覆盖默认 VAE 等。
- `%sampler%`
- `%scheduler%`
- `%steps%`
- `%scale%`
- `%width%`
- `%height%`
- `%denoise%`:对于示例 image-to-image workflow,在约 0.5(对源图像几乎不明显的更改)和 1.0(完全不同的图像,就像没有使用源图像一样)之间改变 denoise 量。默认 text-to-image workflow 不使用,因为对于 text-to-image 使用 1.0 以外的值没有意义。
- `%clip_skip%`:默认 workflows 不使用,但可用于自定义 workflows。

如果您指定了 seed 值,`%seed%` 占位符将插入控件中的 seed 值。如果您将 seed 设置为 `-1`,SillyTavern 将在 `%seed%` 中为每个图像生成新的随机 seed。

#### Custom placeholders

您可以向 workflow 添加自定义占位符:

1. 查找预定义占位符下方的 "Custom" 部分。
2. 点击 "+" 按钮添加新的自定义占位符。
3. 在 `find` 字段中输入占位符的名称。
4. 在 `replace` 字段中输入要替换占位符的值。

自定义占位符将出现在预定义占位符下方的单独列表中。

例如,您可以使用自定义占位符替换默认 workflow 中保存图像文件名的 "SillyTavern" 前缀。添加一个新的自定义占位符,将 `find` 设置为 `filename_prefix`,将 `replace` 设置为 `ServiceTesnor`。将新的 `%filename_prefix%` 占位符插入 workflow JSON 中。现在,您可以通过更改自定义占位符的值将文件名前缀从 SillyTavern 更改为 ServiceTesnor。

+++ JSON with placeholder
```json
{
    "class_type": "SaveImage",
    "inputs": {
        "filename_prefix": "%filename_prefix%",
        "images": ["8", 0]
    }
}
```
+++ Original JSON
```json
{
    "class_type": "SaveImage",
    "inputs": {
        "filename_prefix": "SillyTavern",
        "images": ["8", 0]
    }
}
```
+++

### Comfy tricks

阅读此页面上的所有一般信息,以便您熟悉图像生成选项。可切换 styles 和 common prompt 前缀等选项,结合 ComfyUI workflows 的完全灵活性,允许您创建各种图像生成设置。

#### Loading LoRAs

使用 LoRA tag loader node(例如 [Load LoRA Tag](https://github.com/badjeff/comfyui_lora_tag_loader))加载 prompt 中指定的任何 LoRAs。
现在,您可以使用 `<lora:CroissantStyle:0.8>` 等标签向 prompt 添加任意数量的 LoRAs,它们将被加载到 workflow 中。
这也将使在 [character-specific prompt prefixes](#character-specific-prompt-prefix) 中使用 LoRAs 的 "pro-tip" 与 ComfyUI 一起使用。

#### Setting workflow values from styles or slash-commands

您可以在自定义占位符值中使用 macros。作为一个实际示例,
假设您有时想要生成没有背景的图像,并且您希望可以使用
slash-command 或 image style 进行切换。以下是您可以这样做的方法:

1. 制作一个 ComfyUI workflow,根据输入的值删除图像背景或不删除
2. 使用自定义占位符设置该输入的值,但使用 `{{getvar::remove_background}}` 作为替换值
3. 现在您可以在生成图像之前使用 `/setvar key=remove_background true` 或 `/setvar key=remove_background false` 设置 `remove_background` 的值
4. Workflow 将使用您设置的值来确定是否删除背景
5. 制作一个图像 style "No background",common prompt prefix 为 `{{setvar::remove_background::true}}`
6. 使用 style 控件或 `/imagine-style No background` 在生成图像之前将 `remove_background` 的值设置为 `true`
