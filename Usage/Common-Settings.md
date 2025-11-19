---
order: 160
icon: sliders
route: /usage/common-settings/
---

# 通用设置

这些设置控制使用语言模型生成文本时的采样过程。这些设置的含义对所有支持的后端都是通用的。

## Context Settings

### Response (tokens)

API 将生成的最大 token 数量来响应。

- 响应长度越高,生成响应所需的时间就越长。
- 如果 API 支持,你可以启用 `Streaming` 来在响应生成时逐步显示。
- 当 `Streaming` 关闭时,响应将在完成后一次性显示。

### Context (tokens)

SillyTavern 将作为 prompt 发送给 API 的最大 token 数量,减去响应长度。

- Context 包括角色信息、系统 prompts、聊天历史等。
- 消息之间的虚线表示聊天的 context 范围。该线以上的消息不会发送给 AI。
- 要在生成消息后查看 context 的组成,点击 `Prompt Itemization` 消息选项(展开 `...` 菜单并点击带线条的方形图标)。

## Sampler Parameters

### Temperature

Temperature 控制 token 选择的随机性:

- 低 temperature (<1.0) 会产生更可预测的文本,倾向于选择概率更高的 tokens
- 高 temperature (>1.0) 通过给予低概率 tokens 更好的机会,增加输出的创造性和多样性。

设置为 1 以保持原始概率。

### Repetition Penalty

尝试通过根据 tokens 在 context 中出现的频率来惩罚它们以抑制重复。

将值设置为 1 以禁用其效果。

#### Repetition Penalty Range

从最后生成的 token 开始,将考虑多少个 tokens 进行重复惩罚。如果设置得太高,可能会破坏响应,因为常用词如"the, a, and"等将受到最严重的惩罚。

将值设置为 0 以禁用其效果。

#### Repetition Penalty Slope

如果这个值和 `Repetition Penalty Range` 都大于 0,重复惩罚在 prompt 的末尾将有更大的效果。值越高,效果越强。

将值设置为 0 以禁用其效果。

### Top K

Top K 设置可以选择的最大顶级 tokens 数量。例如,如果 Top K 是 20,这意味着只会保留排名最高的 20 个 tokens(无论它们的概率是多样的还是有限的)。

设置为 0(或 -1,取决于你的后端)以禁用。

### Top P

Top P(又称 nucleus sampling)将所需的顶级 tokens 相加以达到目标百分比。如果前 2 个 tokens 都是 25%,而 Top P 是 0.50,则只考虑前 2 个 tokens。

将值设置为 1 以禁用其效果。

### Typical P

Typical P Sampling 基于 tokens 与集合平均熵的偏差来确定优先级。它保留累积概率接近预定阈值(例如 0.5)的 tokens,强调那些具有平均信息内容的 tokens。

将值设置为 1 以禁用其效果。

### Min P

通过相对于最高 token 切断低概率 tokens 来限制 token 池。产生更连贯的响应,但如果设置得太高也可能加剧重复。

- 在低值如 `0.1-0.01` 时效果最好,但在高 `Temperature` 时可以设置更高。例如: `Temperature: 5, Min P: 0.5`

将值设置为 0 以禁用其效果。

### Top A

Top A 基于最高 token 概率的平方设置 token 选择的阈值。例如,如果 Top-A 值为 0.2,最高 token 的概率为 50%,则概率低于 5% (0.2 * 0.5^2) 的 tokens 将被排除。

将值设置为 0 以禁用其效果。

### Tail Free Sampling

Tail-Free Sampling (TFS) 通过使用导数分析 token 概率的变化率来搜索分布中的低概率 tokens 尾部。它根据归一化的二阶导数保留阈值(例如 0.3)以内的 tokens。越接近 0,丢弃的 tokens 越多。

将值设置为 1 以禁用其效果。

### Smoothing Factor

使用二次变换增加高概率 tokens 的可能性,同时降低低概率 tokens 的可能性。旨在无论 `Temperature` 如何都产生更有创意的响应。

- 在没有截断 samplers(如 `Top K`、`Top P`、`Min P` 等)时效果最好。

将值设置为 0 以禁用其效果。

### Dynamic Temperature

根据最高 token 的可能性动态调整 temperature。旨在产生更有创意的输出而不牺牲连贯性。

- 接受从最小到最大的 temperature 范围。例如: `Minimum Temp: 0.75` 和 `Minimum Temp: 1.25`
- `Exponent` 根据最高 token 应用指数曲线。

取消勾选以禁用其效果。

### Epsilon Cutoff

Epsilon cutoff 设置一个概率下限,低于该下限的 tokens 将被排除在采样之外。单位为 1e-4;合理的值是 3。

设置为 0 以禁用。

### Eta Cutoff

Eta cutoff 是特殊 Eta Sampling 技术的主要参数。单位为 1e-4;合理的值是 3。详情请参见论文 [Truncation Sampling as Language Model Desmoothing by Hewitt et al. (2022)](https://arxiv.org/abs/2210.15191)。

设置为 0 以禁用。

### DRY Repetition Penalty

DRY 惩罚那些会将输入的结尾扩展成先前在输入中出现过的序列的 tokens。如果你想允许逐字重复某些序列(例如名字),你可以将它们添加到序列中断器列表中。请参见 Pull Request [here](https://github.com/oobabooga/text-generation-webui/pull/5677)。

将乘数设置为 0 以禁用。

### Exclude Top Choices (XTC)

XTC sampling 算法移除最可能的 tokens,而不是修剪最不可能的 tokens。它以给定的概率移除除了满足给定阈值的最不可能的 token 之外的所有 tokens。这确保至少保留一个"可行"的选择,保持连贯性。请参见 Pull Request [here](https://github.com/oobabooga/text-generation-webui/pull/6335)。

将概率设置为 0 以禁用。

### Mirostat

Mirostat 将输出的困惑度与输入的困惑度匹配,从而避免重复陷阱(在自回归推理产生文本时,输出的困惑度趋向于零)和混乱陷阱(困惑度发散)。详情请参见论文 [Mirostat: A Neural Text Decoding Algorithm that Directly Controls Perplexity by Basu et al. (2020)](https://arxiv.org/abs/2007.14966)。

Mode 选择 Mirostat 版本。

- 0 = 禁用,
- 1 = Mirostat 1.0 (仅限 llama.cpp),
- 2 = Mirostat 2.0。

### Beam Search

一种贪婪的暴力算法,用于 LLM sampling 以找到最可能的词序或 token 序列。它一次性扩展多个候选序列,在每一步保持固定数量(beam width)的顶级序列。

### Top nsigma

一种基于 logits 统计特性过滤的 sampling 方法。它保留在最大 logit 值的 n 个标准差内的 tokens,为 top-p/top-k sampling 提供了一个更简单的替代方案,同时保持不同 temperatures 下的 sampling 稳定性。
