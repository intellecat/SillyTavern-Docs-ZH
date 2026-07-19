---
order: 160
icon: sliders
route: /usage/common-settings/
---

# 通用设置

这些设置控制使用语言模型生成文本时的采样过程。以下所有设置的含义对所有支持的后端均通用。

## 上下文设置

### 响应长度（token） {#response-tokens}

API 生成回复时的最大 token 数。

- 响应长度越高，生成回复所需的时间越长。
- 如果 API 支持，你可以启用`流式传输`，以便在生成过程中逐步显示回复内容。
- 关闭`流式传输`时，回复将在生成完成后一次性显示。

### 上下文长度（token）

SillyTavern 作为提示词发送给 API 的最大 token 数，不含响应长度。

- 上下文包含角色信息、系统提示词、聊天历史等内容。
- 消息之间的虚线表示聊天的上下文范围，虚线以上的消息不会发送给 AI。
- 若要在生成消息后查看上下文的组成，请点击`提示词分项`消息选项（展开 `...` 菜单并点击有线条的方形图标）。

## 采样参数

### 温度（Temperature）

温度控制 token 选择时的随机性：

- 低温度（<1.0）使文本更具可预测性，更倾向于选择高概率 token
- 高温度（>1.0）通过提高低概率 token 的权重，增加输出的创造性和多样性

设为 1 时使用原始概率分布。

### 重复惩罚（Repetition Penalty）

通过对上下文中出现频率较高的 token 施加惩罚来抑制重复内容。

设为 1 时禁用该效果。

#### 重复惩罚范围（Repetition Penalty Range）

从最后生成的 token 向前计算，纳入重复惩罚范围的 token 数量。设置过高可能导致回复质量下降，因为"the、a、and"等常见词会受到最强的惩罚。

设为 0 时禁用该效果。

#### 重复惩罚斜率（Repetition Penalty Slope）

若此值和`重复惩罚范围`均大于 0，重复惩罚在提示词末尾的效果将更强。值越高，效果越强。

设为 0 时禁用该效果。

### Top K

Top K 设定可供选择的顶部 token 的最大数量。例如，若 Top K 为 20，则仅保留概率最高的 20 个 token（无论其概率分布是否集中）。

设为 0（或 -1，取决于后端）时禁用。

### Top P

Top P（又称核采样）将概率从高到低累加，直至达到目标百分比。例如，若前 2 个 token 各占 25%，且 Top P 为 0.50，则仅考虑这 2 个 token。

设为 1 时禁用该效果。

### Typical P

Typical P 采样根据 token 与集合平均熵的偏差进行优先级排序。它保留累积概率接近预定义阈值（如 0.5）的 token，强调具有平均信息量的 token。

设为 1 时禁用该效果。

### Min P

通过相对于最高概率 token 截断低概率 token 来限制候选池。可提升回复的连贯性，但设置过高时可能加剧重复。

- 在较低值（如 `0.1-0.01`）下效果最佳，但在高`温度`下可以设置更高。例如：`温度: 5，Min P: 0.5`

设为 0 时禁用该效果。

### Top A

Top A 根据最高 token 概率的平方为 token 选择设定阈值。例如，若 Top A 值为 0.2，且最高 token 概率为 50%，则概率低于 5%（0.2 × 0.5²）的 token 将被排除。

设为 0 时禁用该效果。

### 尾部自由采样（Tail Free Sampling）

尾部自由采样（TFS）通过分析 token 概率的变化率（使用导数）来寻找概率分布中的低概率尾部。它根据归一化的二阶导数保留 token，直至达到阈值（如 0.3）。值越接近 0，被丢弃的 token 越多。

设为 1 时禁用该效果。

### 平滑因子（Smoothing Factor）

使用二次变换提高高概率 token 的可能性，同时降低低概率 token 的可能性。旨在不依赖`温度`的情况下产生更具创意的回复。

- 在不使用 `Top K`、`Top P`、`Min P` 等截断采样器时效果最佳。

设为 0 时禁用该效果。

### 动态温度（Dynamic Temperature）

根据最高概率 token 的可能性动态调整温度。旨在在不牺牲连贯性的情况下产生更具创意的输出。

- 接受从最小值到最大值的温度范围。例如：`最小温度: 0.75`，`最大温度: 1.25`
- `指数（Exponent）`根据最高 token 应用指数曲线。

取消勾选时禁用该效果。

### Epsilon 截断（Epsilon Cutoff）

Epsilon 截断设定概率下限，低于该值的 token 将被排除在采样之外。单位为 1e-4，合理值为 3。

设为 0 时禁用。

### Eta 截断（Eta Cutoff）

Eta 截断是特殊 Eta 采样技术的主要参数。单位为 1e-4，合理值为 3。详情参见论文 [Truncation Sampling as Language Model Desmoothing by Hewitt et al. (2022)](https://arxiv.org/abs/2210.15191)。

设为 0 时禁用。

### DRY 重复惩罚（DRY Repetition Penalty）

DRY 惩罚那些会将输入末尾延伸为输入中已出现过的序列的 token。如果希望允许某些序列逐字重复（例如名字），可将其添加到序列分隔符列表中。详情参见 [Pull Request](https://github.com/oobabooga/text-generation-webui/pull/5677)。

将乘数设为 0 时禁用。

### 排除最高选项（XTC）

XTC 采样算法移除最有可能的 token，而非修剪最不可能的 token。它以给定概率移除所有超过给定阈值的 token（仅保留最不可能的那个），从而确保至少保留一个"可行"选择，维持连贯性。详情参见 [Pull Request](https://github.com/oobabooga/text-generation-webui/pull/6335)。

将概率设为 0 时禁用。

### Mirostat

Mirostat 将输出的困惑度与输入的困惑度相匹配，从而避免重复陷阱（即自回归推理产生文本时，输出困惑度趋向于零）和混乱陷阱（困惑度发散）。详情参见论文 [Mirostat: A Neural Text Decoding Algorithm that Directly Controls Perplexity by Basu et al. (2020)](https://arxiv.org/abs/2007.14966)。

模式选择 Mirostat 版本：

- 0 = 禁用
- 1 = Mirostat 1.0（仅限 llama.cpp）
- 2 = Mirostat 2.0

### 束搜索（Beam Search）

一种在 LLM 采样中用于寻找最可能词/token 序列的贪心暴力算法。它同时扩展多个候选序列，在每步保留固定数量（束宽）的最优序列。

### Top nsigma

一种基于统计属性过滤 logit 的采样方法。它保留在最大 logit 值 n 个标准差以内的 token，相比 top-p/top-k 采样提供了更简单的替代方案，同时在不同温度下保持采样稳定性。
