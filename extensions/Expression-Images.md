---
route: /extensions/expression-images/
---

# Character Expressions

## 这是什么?

Expression images 是您的 AI 角色的图像(也称为 'sprites'),显示在聊天窗口旁边(或后面)。

Expression images 可以根据分类自动更改,调整为 AI 最新聊天回复中表达的情感。

## 添加角色表情图像

1. 打开 Extensions 面板并展开 'Character Expressions' 部分。如果您打开了角色聊天,您会看到一个图像占位符网格。
![Expression Drawer](/static/extensions/expression-drawer.png)
2. 点击网格中每个图像左上角的 'Upload image' 按钮,然后选择要应用于该情绪的图像。这将使用正确的文件名将图像保存在 `/data/<user-handle>/characters/(character_name_here)/` 文件夹中。
3. 对所有要分配图像的表情重复此操作。

### 导入 Expression images ZIP 文件

使用 '<i class="fa-solid fa-file-zipper"></i> Upload sprite pack (ZIP)' 按钮,您可以导入包含表情图像集合的 zip 文件,这些图像将自动添加到**当前选定角色**的正确文件夹中。ZIP 文件必须包含扁平结构(无子文件夹)中的所有图像和正确命名的文件。导入 zip 不会自动重命名任何图像以使其与情绪匹配。

## 手动更改表情

1. 点击任何已上传的表情图像(sprites)以在聊天界面附近显示它们(默认 UI 模式)或在屏幕中央显示(Visual Novel 模式)。
2. 使用 `/expression-set (name)` slash command 或匹配的 Quick Reply 来设置 sprite,无需打开扩展菜单。

## 自动更改表情

要在角色回复时自动设置表情,您有多种选择。
当启用消息流式传输时,表情会在每条消息或定期间隔更改。

### classify 模块如何工作?

`classify` 模块使用一个小型的"情感解析"模型,该模型与 SillyTavern 服务器一起运行。该模型获取 AI 的新输出并检测文本表达的情感或情绪类型。虽然单条消息中可能表达多种情感,但该模型只选择最可能的一种并将其返回给 SillyTavern。然后前端扩展显示与该情感关联的图像。

### 设置说明(Local)

1. 打开 Extensions 面板并展开 "Character Expressions" 扩展菜单。
2. 在 classification source 下拉菜单中选择 "Local"。
3. 这将从 HuggingFace Hub 开始一次性下载分类模型(约 ~100 Mb)。
4. 生成任何消息以验证分类是否有效以及 sprite 是否出现。您也可以检查服务器控制台以获取调试日志。

Local classification 默认为 28 个可能的图像标签:[Cohee/distilbert-base-uncased-go-emotions-onnx](https://huggingface.co/Cohee/distilbert-base-uncased-go-emotions-onnx)

要使用 6 选项分类模型,请将 `config.yaml` 文件中的 `extensions.models.classification` 变量的值更改为:[Cohee/bert-base-uncased-emotion-onnx](https://huggingface.co/Cohee/bert-base-uncased-emotion-onnx)

### 设置说明(使用 LLM)

1. 通过 **<i class="fa-solid fa-plug"></i> API Connections** 连接到任何受支持且正确配置的 API。
2. 按照上述方法导入表情图像。
3. 在 classification source 下拉菜单中选择 "Main API"。
4. 可选地,配置分类指令提示。
5. 生成任何消息以验证分类是否有效以及 sprite 是否出现。您也可以检查服务器控制台以获取调试日志。

#### Prompt 构建策略

Main LLM source 允许选择如何构建分类提示:

* **Limited Context**: 仅发送最后一条消息和系统指令提示。
* **Full Context**: 发送整个聊天历史,包括角色卡。

### 设置说明(WebLLM)

1. 安装官方 [WebLLM extension](https://github.com/SillyTavern/Extension-WebLLM)。
2. 按照上述方法导入表情图像。
3. 在 classification source 下拉菜单中选择 "WebLLM"。
4. 可选地,配置分类指令提示。
5. 生成任何消息以验证分类是否有效以及 sprite 是否出现。您也可以检查服务器控制台以获取调试日志。

### 设置说明(使用 Extras)

> [!WARNING]
> Extras 已弃用,可能会在未来更新中删除。

1. 安装并运行 Extras,启用 `classify` 模块:`python server.py --enable-modules=classify`
2. 按照上述方法导入表情图像。
3. 在 classification source 下拉菜单中选择 "Extras"。
4. 每当 AI 向您发送回复时,适当的表情图像将自动显示。

Extras API 默认使用具有 6 个选项的分类模型:[nateraw/bert-base-uncased-emotion](https://huggingface.co/nateraw/bert-base-uncased-emotion)

还有一个具有 28 个选项的模型:[joeddav/distilbert-base-uncased-go-emotions-student](https://huggingface.co/joeddav/distilbert-base-uncased-go-emotions-student)

要使用此模型,您需要更改 Extras 命令行以包含以下参数(前后有空格):`--classification-model=joeddav/distilbert-base-uncased-go-emotions-student`

## 自定义表情

如何获得比默认提供的更多表情选项?您可以在扩展设置中设置 **Custom Expressions**。您可以为 Custom Expressions 分配任何名称。它们将出现在表情图像列表中,并且可以像其他表情一样分配图像。它们将有一个指示符显示这些是自定义的。

> [!TIP]
> Local 和 Extras 仅支持有限的表情列表。
>
> 如果您希望显示 Custom Expressions,您需要训练一个支持标签的分类模型(超出本指南范围),或者您可以使用 LLM 或 WebLLM 作为分类源,两者都会自动使用所有现有表情 - 包括默认表情和任何自定义表情。

## 支持哪些图像格式的 Expressions?

允许任何图像格式,包括 webp 和动画 gif。

最常见的格式是具有透明背景的 PNG 文件。

## 使用默认表情

如果您没有角色所有表情的表情图像,或者根本没有图像,则有多种选项可供默认显示。
所有这些都可以通过 'Default / Fallback Expression' 下的下拉菜单选择。

1. **Choose a Fallback Expression**: 如果选择了一个您没有图像的表情,则会显示 fallback 表情。只需从下拉菜单中选择一个可用的表情即可。
2. **[No Fallback]**: 当不存在图像时,不显示任何内容。
3. **[Default emojis]**: 您可以使用 SillyTavern 附带的内置默认表情。这些是简单的表情符号风格图像。

## 每个表情使用多个图像

可以为每个表情添加多个图像,以在显示的表情中提供更多变化。
要启用此功能,只需切换 **Allow multiple sprites per expression**。
您现在可以上传多个图像,任何其他图像都将显示一个小标记。

可以通过单击手动选择单个图像,或通过 `/expression-set type=sprite` 手动选择,这将列出可用的 sprite 图像,而不是表情。

每当自动选择具有多个图像的表情时,将随机选择现有图像之一。
如果您想在多次使用同一表情时强制选择该表情的新图像,可以启用 **Re-roll if same sprite is used again**。

### 每个表情多个图像的命名约定

如果每个表情有多个图像,则文件需要以特定方式命名。
文件需要以表情的名称开头,然后跟一个后缀,用点或破折号分隔。例如:`joy.png`、`joy-1.png`、`joy.expressive.png`
文件名必须遵循此格式,无论是直接上传还是 ZIP 导入。

## Sprite 文件夹覆盖

> [!NOTE]
> 显示名称(而非角色卡文件名)决定使用哪个图像集

如果您有多个具有相同显示名称的角色,它们都将使用相同的表情图像集。

如果您希望为同名角色的每个版本使用不同的图像集,可以使用 sprites 文件夹覆盖。
文件夹覆盖也可用于定义同一角色的不同 sprite 集(服装等)。

### 如何设置覆盖

1. 在 `/data/<user-handle>/characters` 中创建一个任意名称的文件夹并在那里放置图像,例如 `/data/<user-handle>/characters/Boris`。
2. 打开您想要覆盖其 sprites 的角色的聊天。
3. 在 "Sprite Folder Override" 输入框中输入覆盖文件夹的名称并点击 "Submit"。
4. Sprites 列表将重新加载,"Sprite set" 指示符应显示覆盖文件夹。
5. 或者,您可以使用 `/costume` slash command 实现相同的结果:`/costume Boris`。
6. 通过在覆盖文件夹名称前加反斜杠,它将解析为当前角色 sprites 文件夹中的子文件夹,例如,名为 Boris 的角色的 `/costume \tracksuit` 将解析为 `/data/<user-handle>/characters/Boris/tracksuit` 文件夹。
