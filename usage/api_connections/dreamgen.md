# DreamGen

DreamGen 是一个由 AI 驱动的角色扮演和故事创作应用与 API。它提供免费套餐，以及允许每月无限制访问其专门为可控 AI 角色扮演和故事创作而设计的高质量内部文本生成模型的付费订阅。注册账户即可开始使用：<https://dreamgen.com/>

（免费）积分在每个自然月月初重置。请查看[定价页面](https://dreamgen.com/pricing)了解每个模型的积分消耗，以及[用量页面](https://dreamgen.com/account/usage)查看剩余积分。

## 连接到 DreamGen

### 获取 API Key

前往 [DreamGen API keys](https://dreamgen.com/account/api-keys) 页面，点击「New API Key」按钮。确保将 API Key 复制到剪贴板。

![Create New DreamGen API key](/static/dreamgen/dreamgen_api_keys_new.jpg)
![Copy DreamGen API key](/static/dreamgen/dreamgen_api_keys_copy.jpg)

### 连接

1. 前往 SillyTavern 连接设置。
2. 选择 API：Text Completion
3. 选择 API 类型：DreamGen
4. 输入 API key
5. （可选）选择一个模型

![Connecting to DreamGen](/static/dreamgen/dreamgen_st_connection.png)

## 模型

DreamGen API 提供多个不同规模的模型。

- Lucid Max（API 中称为 `lucid-v1-max` 或 `lucid-v1-extra-large`）
- Lucid Base（API 中称为 `lucid-v1-base` 或 `lucid-v1-medium`）—— 对应权重公开的 [Lucid V1 Nemo](https://dreamgen.com/docs/models/lucid-v1/huggingface)。

Lucid Base 消耗积分更少、速度更快，而 Lucid Max 创意性更强，能处理更复杂的指令和叙事。

## 设置

Lucid V1 DreamGen 模型使用经过角色扮演和写作优化的 Llama 3 聊天模板扩展，配合特定的系统提示词效果最佳。

强烈建议从以下主预设之一开始：

- 确保启用了 instruct 模式，并在导入时勾选所有复选框。
- [DreamGen Lucid V1 角色扮演预设](https://dreamgen.com/docs/models/lucid-v1/sillytavern/master-preset/role-play)
- [DreamGen Lucid V1 故事预设](https://dreamgen.com/docs/models/lucid-v1/sillytavern/master-preset/story)

这些预设内置了对 `/sys` 的支持，可向模型发送指令，用于引导剧情走向或控制角色行为。

![DreamGen preset selected](/static/dreamgen/dreamgen_st_preset.png)

其他资源：

- [**DreamGen + SillyTavern 详细使用指南**](https://dreamgen.com/docs/models/lucid-v1/sillytavern)
- [Lucid V1 提示词格式详细文档](https://dreamgen.com/docs/models/lucid-v1)
- [DreamGen + SillyTavern 角色扮演演示](https://imgur.com/a/dreamgen-lucid-sillytavern-roleplay-demo-bhzQpto)
- [DreamGen + SillyTavern 故事写作演示](https://imgur.com/a/dreamgen-lucid-sillytavern-writing-demo-JLv5iO3)
- [创建自定义场景的技巧](https://v2.dreamgen.com/docs/scenario-editor)

## 常见问题

### 如何让回复更长或更短？

可以在格式化预设中设置 `Last Assistant Prefix`。

长回复示例：

```txt
<|start_header_id|>user<|end_header_id|>

The next message is from  and is at least 100 words long<|eot_id|><|start_header_id|>writer character <|end_header_id|>

```

短回复示例：

```txt
<|start_header_id|>user<|end_header_id|>

The next message is from  and is at most 50 words long<|eot_id|><|start_header_id|>writer character <|end_header_id|>

```

注意保留所有换行符，包括末尾的两个空行。

![Long Message Prefix](/static/dreamgen/dreamgen_st_long_response_prefix.png)

也可以在角色卡或系统提示词中加入写作风格描述，例如：

```txt
## Style

<your description>
```

详见 [「Style」文档](https://v2.dreamgen.com/docs/scenario-editor#style)，其中有更多说明和示例。

### 如何引导角色扮演/故事走向？

使用 `/sys` 选项向模型发送指令。以下是一些示例：

> The innkeeper offers Daria and the others a pint of ale.

> The next message is from Draco and should be at least 200 words, focusing on his inner conflict about the decision.

[查看实际效果。](https://imgur.com/a/dreamgen-lucid-sillytavern-roleplay-demo-bhzQpto)
