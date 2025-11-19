---
order: 60
route: /usage/prompts/cfg/
---

# CFG

Page written by: kingbri

Contributors: kingbri, Guillaume "Vermeille" Sanchez, AliCat

## What is it?

CFG,即 classifier-free guidance,是一种用于帮助使 prompt 的部分内容变得更不显著或更显著的方法。

### Supported Backend APIs

目前,支持的 backend 是 oobabooga 的 textgen WebUI、NovelAI 和 TabbyAPI。
NovelAI 有自己的 [CFG 文档](https://web.archive.org/web/20240917150051/https://docs.novelai.net/text/cfg.html)。

警告:CFG 会增加 vram 使用量,因为需要摄取超过 1 个 prompt!如果在启用 CFG 的情况下生成 prompt 时 GPU 内存耗尽,请考虑减小上下文大小、使用参数较少的模型或完全关闭 CFG。

---

## Configuration

访问 CFG 设置的方式与访问 Author's note 相同:

![CFGhamburgermenupng](/static/cfg-hamburger.png)

以下是 CFG 面板的外观:

![CFGchatpanelpng](/static/cfg-panel.png)

CFG 面板中有四个下拉菜单:

- Chat CFG

  - 将 CFG scale 和 prompt 的范围限定为仅此聊天
- Character CFG

  - 将 CFG scale 和 prompt 的范围限定为指定的角色
- Global CFG

  - 全局覆盖 CFG scale 和 prompt(也覆盖模型预设!)
- CFG Advanced Settings(以前称为 CFG Prompt Cascading)

  - 一个组合前 3 个下拉菜单中的 prompt 并设置插入深度的地方。

注意:如果 guidance scale 设置为 1,则不会发送任何内容,因为这是 CFG 处于"关闭"状态时的值。

#### Group Chats

在群组聊天中,CFG scale 面板如下所示:

![CFGpanelgcpng](/static/cfg-groups.png)

主要变化是 character CFG 被移除,并且在 chat CFG 下拉菜单中出现了一个名为 `Use Character CFG Scales` 的复选框。这允许使用当前角色的 guidance scale 而不是 chat CFG scale 设置的值。

此功能的主要用途是根据每个角色的个体需求改变 scale。

此外,在 prompt cascading 中选中 `Character Negatives` 框将附加独立的角色负面 prompt 以及聊天 prompt(如果启用)。

---

## Concepts

### Isn't this in Stable Diffusion?

是也不是。LLM 的 CFG 与人们在 Stable Diffusion 中习惯的工作方式不同。基于 LLM 的 CFG 基于"prompt 混合"的原理工作。CFG 公式采用正面和负面 prompt,然后混合它们之间的 *差异*。从那里,发送组合的 prompt 并生成响应!

这是一个帮助可视化这个概念的插图。红色代表负面 prompt,蓝色代表中性 prompt,紫色代表被解释的混合结果。所有白色空间在所有 3 个 prompt 中都是相同的,因此这些不用于 CFG 混合。

![stcfgdiagrampng](/static/cfg-diagram.png)

如果你想了解更多关于 CFG 和 LLM 的信息,Vermifuge 的原始论文位于此处。我建议阅读/收听:

- Paper - [[2306.17806] Stay on topic with Classifier-Free Guidance (arxiv.org)](https://arxiv.org/abs//2306.17806)

- Audio version - [https://www.youtube.com/watch?v=MGY00YFcyco](https://www.youtube.com/watch?v=MGY00YFcyco)


### Do I need CFG prompts?

不需要!CFG prompt 是完全可选的。仅将 guidance scale 调整为高于 `1` 也会对响应产生影响,这可以增强聊天和角色互动。

### What makes a good CFG prompt?

那么,我们已经确定 CFG prompting 与 Stable Diffusion 的负面标签和嵌入不同。我们如何制作 prompt?

警告:这假设你已使用 PLists 和 Ali:Chat 创建了一个角色。如果你还没有,请随意尝试各种 prompting 技术。

假设我有一个名为"John"的角色。John 应该从他的示例对话中一直感到快乐和兴奋。然而,在与 John 聊天时,他有时会悲伤和沮丧。

为了消除这一点,CFG 来拯救!只需将负面 prompt 设为 `[John's feelings: sad, depressed]` 即可帮助消除悲伤部分。你可以选择将正面 prompt 设为 `[John's feelings: happy, joyful]` 以进一步突出 John 的快乐部分。

### Positive Prompts

我在上一节中讨论了这个,但我想更多地谈谈这个。Positive prompt 用于进一步突出角色的某些部分。让我们再次使用 John 作为例子。通过使用正面 prompt `[John's feelings: happy, joyful]` 使他更快乐,John 应该开始输出比不包含正面 prompt 时更快乐的对话。

### But...

这些只是一种特定角色格式的经验中的 **宽松指南**。有许多其他方法可以创建你应该尝试的 prompt。随时与其他用户分享你的想法!

### Guidance Scale

这是一个经验法则。guidance scale 为 `1` 意味着 CFG 被禁用。实际上,如果 guidance scale 为 1,SillyTavern 不会向你的 backend 发送任何内容。guidance scale `>1` 将在不同程度上提供其他部分中显示的结果。

然而,guidance scale `<1` 将产生 *相反的* 效果,因为负面 prompt 在这里被用作主要 prompt。

让我们再次使用 John 的例子。负面 prompt 是 `[John's feelings: sad, depressed]`,正面 prompt 是 `[John's feelings: happy, joyful]`,guidance scale 为 `0.8`。

这将反过来更多地突出 *负面* prompt,你会看到 John 开始表现得比正常情况更悲伤而不是更快乐。

简而言之:使用 guidance scale `1.5` 并根据你的输出从那里向上和向下调整。

### Prompt Cascading

Negative 和 positive 可以在 CFG 类型之间级联(类型为 per-chat、per-character 和 global overrides)。有关更多信息,请参阅 Configuration 标题。

### Insertion Depth

遵循基本规则:某物在 prompt 中的位置越低,它对响应的影响就越大。对于聊天,我建议使用默认深度 `1`,因为它与 SillyTavern 的其他组件非常灵活。

然而,如果你想尝试,插入深度 `0` 是开放的。然而,这些可能会极大地改变你的响应的外观,**不建议** 在这里使用 prompt cascading!
