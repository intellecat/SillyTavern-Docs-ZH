---
order: 160
icon: sliders
route: /usage/common-settings/
---

# 常用设置

这些设置控制使用语言模型生成文本时的采样过程。这些设置的含义对所有支持的后端都是通用的。

## 上下文设置

### 回应长度（tokens）

API 将生成的最大 token 数量。

- 回应长度越高，生成回应所需的时间就越长。
- 如果 API 支持，您可以启用 `流式传输` 来逐步显示正在生成的回应。
- 当 `流式传输` 关闭时，回应将在完成后一次性显示。

### 上下文长度（tokens）

SillyTavern 将作为提示词发送给 API 的最大 token 数量，减去回应长度。

- 上下文包括角色信息、系统提示词、聊天历史等。
- 消息之间的虚线表示聊天的上下文范围。该线以上的消息不会发送给 AI。
- 要在生成消息后查看上下文的组成，请点击 `提示词项目化` 消息选项（展开 `...` 菜单并点击带线条的方形图标）。

## 采样参数

### Temperature（温度）

Temperature 控制 token 选择的随机性：

- 低温度（<1.0）会产生更可预测的文本，倾向于选择概率更高的 token
- 高温度（>1.0）通过给予低概率 token 更好的机会，增加输出的创造性和多样性。

设置为 1 以保持原始概率。

### Repetition Penalty（重复惩罚）

尝试通过根据 token 在上下文中出现的频率来惩罚它们以抑制重复。

将值设置为 1 以禁用其效果。

#### Repetition Penalty Range（重复惩罚范围）

从最后生成的 token 开始，将考虑多少个 token 进行重复惩罚。如果设置得太高，可能会破坏回应，因为常用词如"的、了、和"等将受到最严重的惩罚。

将值设置为 0 以禁用其效果。

#### Repetition Penalty Slope（重复惩罚斜率）

如果这个值和 `重复惩罚范围` 都大于 0，重复惩罚在提示词的末尾将有更大的效果。值越高，效果越强。

将值设置为 0 以禁用其效果。

### Top K

Top K 设置可以选择的最大顶级 token 数量。例如，如果 Top K 是 20，这意味着只会保留排名最高的 20 个 token（无论它们的概率是多样的还是有限的）。

设置为 0（或 -1，取决于您的后端）以禁用。

### Top P

Top P（又称核采样）将所需的顶级 token 相加以达到目标百分比。如果前 2 个 token 都是 25%，而 Top P 是 0.50，则只考虑前 2 个 token。

将值设置为 1 以禁用其效果。

### Typical P

Typical P 采样基于 token 与集合平均熵的偏差来确定优先级。它保留累积概率接近预定阈值（例如 0.5）的 token，强调那些具有平均信息内容的 token。

将值设置为 1 以禁用其效果。

### Min P

通过相对于最高 token 切断低概率 token 来限制 token 池。产生更连贯的回应，但如果设置得太高也可能加剧重复。

- 在低值如 `0.1-0.01` 时效果最好，但在高 `Temperature` 时可以设置更高。例如：`Temperature: 5, Min P: 0.5`

将值设置为 0 以禁用其效果。

### Top A

Top A 基于最高 token 概率的平方设置 token 选择的阈值。例如，如果 Top-A 值为 0.2，最高 token 的概率为 50%，则概率低于 5%（0.2 * 0.5^2）的 token 将被排除。

将值设置为 0 以禁用其效果。

### Tail Free Sampling（尾部自由采样）

Tail-Free Sampling (TFS) 通过使用导数分析 token 概率的变化率来搜索分布中的低概率 token 尾部。它根据归一化的二阶导数保留阈值（例如 0.3）以内的 token。越接近 0，丢弃的 token 越多。

将值设置为 1 以禁用其效果。

### Smoothing Factor（平滑因子）

使用二次变换增加高概率 token 的可能性，同时降低低概率 token 的可能性。旨在无论 `Temperature` 如何都产生更有创意的回应。

- 在没有截断采样器（如 `Top K`、`Top P`、`Min P` 等）时效果最好。

将值设置为 0 以禁用其效果。

### Dynamic Temperature（动态温度）

根据最高 token 的可能性动态调整温度。旨在产生更有创意的输出而不牺牲连贯性。

- 接受从最小到最大的温度范围。例如：`最小温度: 0.75` 和 `最大温度: 1.25`
- `指数` 根据最高 token 应用指数曲线。

取消勾选以禁用其效果。

### Epsilon Cutoff（Epsilon 截断）

Epsilon 截断设置一个概率下限，低于该下限的 token 将被排除在采样之外。单位为 1e-4；合理的值是 3。

设置为 0 以禁用。

### Eta Cutoff（Eta 截断）

Eta 截断是特殊 Eta 采样技术的主要参数。单位为 1e-4；合理的值是 3。详情请参见论文 [Truncation Sampling as Language Model Desmoothing by Hewitt et al. (2022)](https://arxiv.org/abs/2210.15191)。

设置为 0 以禁用。

### DRY Repetition Penalty（DRY 重复惩罚）

DRY 惩罚那些会将输入的结尾扩展成先前在输入中出现过的序列的 token。如果您想允许逐字重复某些序列（例如名字），您可以将它们添加到序列中断器列表中。请参见[这里](https://github.com/oobabooga/text-generation-webui/pull/5677)的 Pull Request。

将乘数设置为 0 以禁用。

### Exclude Top Choices (XTC)（排除顶级选择）

XTC 采样算法移除最可能的 token，而不是修剪最不可能的 token。它以给定的概率移除除了满足给定阈值的最不可能的 token 之外的所有 token。这确保至少保留一个"可行"的选择，保持连贯性。请参见[这里](https://github.com/oobabooga/text-generation-webui/pull/6335)的 Pull Request。

将概率设置为 0 以禁用。

### Mirostat

Mirostat 将输出的困惑度与输入的困惑度匹配，从而避免重复陷阱（在自回归推理产生文本时，输出的困惑度趋向于零）和混乱陷阱（困惑度发散）。详情请参见论文 [Mirostat: A Neural Text Decoding Algorithm that Directly Controls Perplexity by Basu et al. (2020)](https://arxiv.org/abs/2007.14966)。

模式选择 Mirostat 版本。

- 0 = 禁用，
- 1 = Mirostat 1.0（仅限 llama.cpp），
- 2 = Mirostat 2.0。

### Beam Search（束搜索）

一种贪婪的暴力算法，用于 LLM 采样以找到最可能的词序或 token 序列。它一次性扩展多个候选序列，在每一步保持固定数量（束宽）的顶级序列。

### Top nsigma

一种基于 logits 统计特性的采样方法。它保留在最大 logit 值的 n 个标准差内的 token，为 top-p/top-k 采样提供了一个更简单的替代方案，同时保持不同温度下的采样稳定性。
