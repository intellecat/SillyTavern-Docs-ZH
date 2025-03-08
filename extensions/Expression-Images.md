# Expression Images (表情图片)

### 这是什么？

表情图片是您的 AI 角色的图片（也称为"精灵图"），显示在聊天窗口旁边（或后面）。

表情图片可以使用与 SillyTavern 主应用程序一起运行的分类模型。这允许表情根据 AI 最近的聊天回应中表达的情感自动改变。

### 设置说明（无需 Extras 的离线模式）

1. 打开扩展面板并展开"Expression images"部分。如果您已打开角色聊天，您将看到一个图片占位符网格。
![表情抽屉](/static/extensions/expression-drawer.png)

2. 点击网格中每个图片左上角的"导入"按钮，并选择您想应用到该情绪的图片。这将把图片以正确的文件名保存在 `/data/<user-handle>/characters/(character_name_here)/` 文件夹中。

3. 要在您的 SillyTavern 窗口中显示图片，请在上传后点击网格中的图片。

### 分类模块如何工作？

`classify` 模块使用一个在 SillyTavern 主机（例如您的 PC 或 colab 机器）上运行的小型"情感解析"模型。该模型接收 AI 的新输出并检测文本表达的是什么类型的情感或情绪。虽然一条消息中可能表达多种情感，但模型只选择最可能的一种并返回给 SillyTavern。然后前端插件显示与该情感相关联的图片。

### 手动更改表情

1. 点击任何已上传的表情图片（精灵图）以在聊天界面附近（默认 UI 模式）或屏幕中央（视觉小说模式）显示它们。
2. 使用 `/emote (name)` 斜杠命令或匹配的快速回复来设置精灵图，无需打开扩展菜单。

### 设置说明（本地分类）

1. 确保您使用的是最新版本的 SillyTavern。
2. 打开扩展面板并展开"Character Expressions"插件菜单。
3. 在分类源下拉菜单中选择"Local"。
4. 这将启动一次性从 HuggingFace Hub 下载分类模型（约 ~100 Mb）。
5. 生成任何消息以验证分类是否正常工作并且精灵图是否出现。您也可以检查服务器控制台的调试日志。

### 设置说明（使用 Extras）

1. 安装并运行 Extras，并启用 `classify` 模块：`python server.py --enable-modules=classify`
2. 按照上述方式导入表情图片。
3. 在分类源下拉菜单中选择"Extras"。
4. 当 AI 向您发送回应时，适当的表情图片将自动显示。

### 设置说明（使用 LLM）

1. 连接到任何受支持且正确配置的文本生成 API。
2. 按照上述方式导入表情图片。
3. 在分类源下拉菜单中选择"LLM"。
4. 可选择配置分类指令提示。
5. 生成任何消息以验证分类是否正常工作并且精灵图是否出现。您也可以检查服务器控制台的调试日志。

### 如何获得更多表情选项？

#### 本地

本地分类默认有 28 个可能的图片标签：[Cohee/distilbert-base-uncased-go-emotions-onnx](https://huggingface.co/Cohee/distilbert-base-uncased-go-emotions-onnx)

要使用 6 选项分类模型，请用文本编辑器打开 `config.yaml` 文件，并将 `classificationModel` 变量的值更改为 [Cohee/bert-base-uncased-emotion-onnx](https://huggingface.co/Cohee/bert-base-uncased-emotion-onnx)

之后，重启 ST 服务器以重新下载模型。要恢复，请再次更改 `config.yaml` 中的值。

#### Extras API

Extras API 默认使用具有 6 个选项的分类模型：[nateraw/bert-base-uncased-emotion](https://huggingface.co/nateraw/bert-base-uncased-emotion)

还有一个具有 28 个选项的模型：[joeddav/distilbert-base-uncased-go-emotions-student](https://huggingface.co/joeddav/distilbert-base-uncased-go-emotions-student)

要使用此模型，您需要在 Extras 命令行中添加以下参数（前后都有空格）：

`--classification-model=joeddav/distilbert-base-uncased-go-emotions-student`

### 表情支持哪些图片格式？

允许任何图片格式，包括 webp 和动态 gif。

最常见的格式是带有透明背景的 PNG 文件。

### 使用"默认表情"

!!! warning
此功能仅在启用本地分类或连接 Extras API 时激活。无法手动显示默认表情图片。
!!!

![使用默认表情](/static/extensions/expression-default.png)

如果您没有角色的自定义表情图片，可以使用基本 SillyTavern 安装中包含的内置默认表情。这些是简单的表情符号风格图片。只需点击扩展面板内表情图片部分顶部的复选框。默认表情将与任何可用的自定义表情一起工作，并在您的自定义图片集缺少特定情绪的图片时显示。

### 导入表情图片 zip 文件

使用"import ZIP"按钮，您可以导入包含表情图片集合的 zip 文件，这些图片将自动添加到**当前选择的角色**的正确文件夹中。zip 文件必须具有扁平的内部结构（无子文件夹），并且各个图片应该正确命名。导入 zip 不会自动重命名任何图片以使其匹配情绪。

### 限制

#### 显示名称（而不是角色卡片文件名）决定使用哪个图片集

如果您有多个具有相同显示名称的角色，它们都将使用相同的表情图片集。

如果您想为同名角色的每个版本使用不同的图片集，您可以使用精灵图文件夹覆盖。

#### 如何设置覆盖

1. 在 `/data/<user-handle>/characters` 中创建一个任意名称的文件夹并将图片放在那里，例如 `/data/<user-handle>/characters/Boris`。
2. 打开要覆盖精灵图的角色的聊天。
3. 在"Sprite Folder Override"输入框中输入覆盖文件夹的名称并点击"Submit"。
4. 精灵图列表将重新加载，"Sprite set"指示器应显示覆盖文件夹。
5. 或者，您可以使用 `/costume` 斜杠命令达到相同的效果：`/costume Boris`。
6. 通过在覆盖文件夹名称前加上反斜杠，它将解析为当前角色精灵图文件夹中的子文件夹，例如，对于名为 Boris 的角色，`/costume \tracksuit` 将解析为 `/data/<user-handle>/characters/Boris/tracksuit` 文件夹。

#### 自定义表情

1. 您可以向列表添加自定义表情标签并上传精灵图。
2. **但是**，它们不会被分类模型自动标记。您*需要*手动设置它们（通过点击或使用 `/emote` 命令）。
