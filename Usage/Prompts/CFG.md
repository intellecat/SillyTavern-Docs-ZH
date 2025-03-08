---
order: prompts-50
---

# CFG（无分类器引导）

页面作者：kingbri

贡献者：kingbri, Guillaume "Vermeille" Sanchez, AliCat

## 这是什么？

CFG（classifier-free guidance，无分类器引导）是一种用于帮助使提示词的某些部分变得更不突出或更突出的方法。

### 支持的后端 API

目前支持的后端包括 oobabooga 的 textgen WebUI、NovelAI 和 TabbyAPI。
NovelAI 有其自己的 [CFG 文档](https://web.archive.org/web/20240917150051/https://docs.novelai.net/text/cfg.html)。

警告：由于需要处理多个提示词，CFG 会增加显存使用量！如果在使用 CFG 生成提示词时 GPU 内存耗尽，请考虑减小上下文大小、使用参数较少的模型或完全关闭 CFG。

---

## 配置

访问 CFG 设置的方式与访问作者注释相同：

![CFGhamburgermenupng](/static/cfg-hamburger.png)

以下是 CFG 面板的样子：

![CFGchatpanelpng](/static/cfg-panel.png)

CFG 面板中有四个下拉菜单：

- 聊天 CFG
  
  - 将 CFG 比例和提示词的范围限定在当前聊天中
- 角色 CFG
  
  - 将 CFG 比例和提示词的范围限定在指定角色
- 全局 CFG
  
  - 全局覆盖 CFG 比例和提示词（也会覆盖模型预设！）
- CFG 高级设置（以前称为 CFG 提示词级联）
  
  - 用于组合前三个下拉菜单中的提示词并设置插入深度的位置。

注意：如果引导比例设置为 1，由于这是 CFG 的"关闭"状态，因此不会发送任何内容。

#### 群聊

在群聊中，CFG 比例面板看起来是这样的：

![CFGpanelgcpng](/static/cfg-groups.png)

主要变化是移除了角色 CFG，并在聊天 CFG 下拉菜单中添加了一个名为"使用角色 CFG 比例"的复选框。这允许使用当前角色的引导比例，而不是聊天 CFG 比例设置的值。

此功能的主要用途是根据每个角色的个别需求来调整比例。

此外，在提示词级联中勾选"角色负面提示词"框将会在聊天提示词（如果启用）的基础上附加独立的角色负面提示词。

---

## 概念

### 这不是在 Stable Diffusion 中使用的吗？

是也不是。LLM 中的 CFG 的工作方式与 Stable Diffusion 中的不同。基于 LLM 的 CFG 基于"提示词混合"原理工作。CFG 公式接受一个正面提示词和一个负面提示词，然后混合它们之间的*差异*。之后，发送组合的提示词并生成响应！

这里有一个插图帮助理解这个概念。红色代表负面提示词，蓝色代表中性提示词，紫色代表被解释的混合结果。所有三个提示词中相同的白色空间不会用于 CFG 混合。

![stcfgdiagrampng](/static/cfg-diagram.png)

如果您想了解更多关于 CFG 和 LLM 的信息，Vermifuge 的原始论文在这里。建议您阅读/收听：

- 论文 - [[2306.17806] Stay on topic with Classifier-Free Guidance (arxiv.org)](https://arxiv.org/abs//2306.17806)
  
- 音频版本 - [https://www.youtube.com/watch?v=MGY00YFcyco](https://www.youtube.com/watch?v=MGY00YFcyco)
  

### 我需要 CFG 提示词吗？

不需要！CFG 提示词完全是可选的。仅仅将引导比例调整到 `1` 以上也会对响应产生影响，这可以突出聊天和角色互动。

### 什么是好的 CFG 提示词？

我们已经确定 CFG 提示词与 Stable Diffusion 的负面标签和嵌入不同。那么我们如何制作提示词呢？

警告：这假设您使用 PLists 和 Ali:Chat 创建了一个角色。如果没有，请随意尝试各种提示词技巧。

假设我有一个名叫"John"的角色。从他的示例对话中可以看出，John 应该一直感到快乐和兴奋。然而，在与 John 聊天时，他有时会表现得悲伤和沮丧。

为了消除这种情况，CFG 来救场！只需将负面提示词设为 `[John's feelings: sad, depressed]` 来帮助消除悲伤部分。您可以选择将正面提示词设为 `[John's feelings: happy, joyful]` 来进一步突出 John 的快乐部分。

### 正面提示词

我在上一节中提到了这一点，但我想再详细说明一下。正面提示词用于进一步突出角色的某些部分。让我们再次以 John 为例。通过使用正面提示词 `[John's feelings: happy, joyful]` 使他更快乐，John 应该会开始输出比不包含正面提示词时更快乐的对话。

### 但是...

这些只是基于特定角色格式经验的**宽松指南**。还有许多其他方法可以创建提示词，您应该进行实验。请随意与其他用户分享您的想法！

### 引导比例

这里有一个经验法则。引导比例为 `1` 意味着 CFG 被禁用。实际上，如果引导比例为 1，SillyTavern 不会向后端发送任何内容。引导比例 `>1` 将在不同程度上产生其他部分所示的结果。

然而，引导比例 `<1` 将产生*相反*的效果，因为这里负面提示词被用作主要提示词。

让我们再次以 John 为例。负面提示词是 `[John's feelings: sad, depressed]`，正面提示词是 `[John's feelings: happy, joyful]`，引导比例为 `0.8`。

这将反过来更多地突出*负面*提示词，您会看到 John 开始表现得比正常情况更悲伤，而不是更快乐。

简而言之：使用引导比例 `1.5`，然后根据您的输出向上或向下调整。

### 提示词级联

负面和正面提示词可以在 CFG 类型之间级联（类型包括每次聊天、每个角色和全局覆盖）。有关更多信息，请参见配置标题。

### 插入深度

遵循基本规则：在提示词中位置越低，对响应的影响就越大。对于聊天，我建议使用默认深度 `1`，因为它与 SillyTavern 的其他组件非常灵活。

然而，如果您想要实验，插入深度 `0` 是开放的。但是，这些可能会显著改变您的响应的外观，并且不建议在这里使用提示词级联！
