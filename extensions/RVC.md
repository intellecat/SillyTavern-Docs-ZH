---
route: /extensions/rvc/
---

# 基于检索的语音转换(RVC)

本指南将引导您使用 RVC,这是一种允许将语音特征从一个音频片段转移到另一个音频片段的技术,使语音能够以不同的音调和风格说话。

曾经喜欢过那些著名的"总统玩 X"视频吗?它们是使用 RVC 创建的。使用 RVC 扩展,您可以让您的 SillyTavern 角色以您想要的任何声音说话,无论是动漫、电影,甚至是您自己的独特声音。

RVC 不是 TTS:它更像是语音到语音。它以音频片段作为输入。在后台,RVC 所做的是与 SillyTavern 的 TTS 扩展协同工作:它等待 TTS 生成音频文件(无论您是否使用 RVC,TTS 都会这样做),然后 RVC 将执行第二次传递,该传递获取 TTS 音频文件并将其转换为您的 RVC 配置中的克隆语音。

## RVC 设置

SillyTavern 的 RVC 支持几个执行音频转换的 API 来源:

* [rvc-python](https://github.com/daswer123/rvc-python)
* [SillyTavern Extras](https://github.com/SillyTavern/SillyTavern-Extras)(已弃用)

### 通用先决条件

在开始之前,请确保您已满足以下先决条件。

#### ffmpeg

确保您的 PATH 环境变量中有 `ffmpeg` 二进制文件。此工具用于转换传入的音频。

**Windows**:

* 使用 SillyTavern Launcher 脚本中的工具箱自动安装 ffmpeg: <https://github.com/SillyTavern/SillyTavern-Launcher>
* 或在此处下载构建版本: <https://www.gyan.dev/ffmpeg/builds/>
* 如何修改 PATH 变量: <https://www.architectryan.com/2018/03/17/add-to-the-path-on-windows-10/>
* 要测试您是否正确执行了操作,请打开命令提示符并运行 ```ffmpeg```。它应该打印 ffmpeg 版本和信息。

**Linux**:

使用您的包管理器安装 ffmpeg。

```shell
# Debian/Ubuntu
sudo apt install ffmpeg
# Arch Linux
sudo pacman -S ffmpeg
# Fedora
sudo dnf install ffmpeg
```

**macOS**:

使用 [Homebrew](https://brew.sh/) 安装 ffmpeg:

```shell
brew install ffmpeg
```

#### 确保 TTS 已启用并正常工作

RVC 依赖于 TTS,您需要启用 TTS 扩展。在尝试将 RVC 添加到混合中之前,您的 TTS 必须已经正常工作并正确地叙述您的聊天!

请注意:

* 系统 TTS 引擎根本不支持语音转换。
* 流式 TTS 将在转换之前等待音频流结束。

#### 安装扩展

从扩展面板(堆叠块图标)中的"下载扩展和资源"菜单安装"RVC"扩展。

#### 在 SillyTavern 中启用 RVC*

在 SillyTavern 中,导航到**扩展** > **RVC** 并启用它。

#### 选择来源

在扩展设置中,选择要使用的 RVC 来源。然后继续进行特定于来源的安装说明。

### rvc-python 设置

#### 1. 安装包

按照 GitHub 页面上的安装说明操作:[rvc-python 安装](https://github.com/daswer123/rvc-python?tab=readme-ov-file#installation)。如果您有 Nvidia GPU,建议遵循 CUDA 安装说明。

如果您在 Windows 上安装时遇到问题(例如构建 fairseq 步骤失败),请确保您的 PC 上安装了以下软件:

* [Windows 10 SDK](https://developer.microsoft.com/en-us/windows/downloads/windows-sdk/)
* [Visual Studio Build Tools 2022](https://visualstudio.microsoft.com/downloads/#build-tools-for-visual-studio-2022)

#### 2. 准备模型

创建一个用于存储 RVC 模型的目录。默认情况下,它名为 `rvc_models`,并在启动服务器时从您的当前目录中获取。每个模型都是一个子文件夹(其名称将在 UI 中可见),应包含 `.pth`(必需)和 `.index`(可选)文件。

阅读更多:[rvc-python 模型管理](https://github.com/daswer123/rvc-python?tab=readme-ov-file#model-management)

#### 3. 启动 API 服务器

通过运行以下命令启动 API 服务器:

```shell
python -m rvc_python api -p 5050 -l -md models_path
```

**参数:**

* `5050` - 为服务器设置监听端口。如果您想在不同端口上托管,请更改。
* `models_path` - 设置模型的路径。如果您想使用默认的 `rvc_models` 目录,请删除。
* `-l` - 将服务器设置为在所有网络接口上监听。删除以仅在 localhost 上监听。

#### 4. 连接到服务器

* 在 RVC 扩展设置中,设置适当的 **rvc-python API URL**。默认情况下,它将是 `http://localhost:5050`。
* 如果您安装了支持 CUDA 加速的 rvc-python,请选中**使用 CUDA** 复选框。
* 按"刷新"以加载可用语音列表。

#### 5. 配置语音映射

**语音映射为每个角色或用户角色定义语音转换设置。**

* 要设置语音映射,请从"角色"下拉菜单中选择您的角色或角色名称,然后选择一个 RVC"语音",然后单击应用。
* 可选地,您还可以配置其他相关设置,例如音高校正或过滤。
* 如果您正确执行了所有操作,语音映射调试区域将显示类似"Betty:MyVoice(rvpme)"的内容。

### SillyTavern Extras 设置

#### 1. 准备 RVC 模型文件

* 在文件浏览器中,导航到:`\SillyTavern-extras\data\models\rvc`。
* 创建一个像"Betty"这样的子文件夹,并将 `.pth` 和 `.index` 文件放入其中。(提示:您可以从 https://voice-models.com 下载语音文件,确保语音名称显示它是 RVPME。)

#### 2. 安装要求

使用以下命令安装必要的要求:

```shell
pip install -r requirements-rvc.txt`
```

#### 3. 启用 RVC 运行 SillyTavern-extras

启用 RVC 模块启动 SillyTavern-extras。此示例调用假设您使用了 Edge TTS,它预装了 SillyTavern-extras:

```shell
python server.py --enable-modules=rvc,edge-tts
```

可选地,如果您有能够运行的 GPU,您可能希望在 GPU 上运行 RVC,方法是在启动命令中添加 ```--cuda```。根据快速测试,叙述 50 个令牌(约 36 个单词)的 VRAM 使用量为 3.4GB,200 个令牌(约 150 个单词)的 VRAM 使用量为 7.6GB。

#### 4. 设置语音映射

为 RVC 创建语音映射。将您的角色设置为您想要的 SillyTavern 角色名称,并将语音设置为您在步骤 1 中创建的 RVC 文件夹,然后单击应用。如果您正确执行了操作,语音映射将显示类似"Betty:MyVoice(rvpme)"的内容。

#### 5. 选择音高提取

* 选择"rmvpe"作为音高提取方法。
* 如果您在使用"rmvpe"时遇到问题,请尝试其他方法(例如,"harvest"或"torchcrepe")。

#### 6. (可选)配置 RVC 以将您的生成保存到文件

如果出于测试或故障排除目的,您希望保存生成的 RVC 音频,请在启动命令中添加 ```--rvc-save-file```。这将在 `SillyTavern-extras/data/tmp/rvc_output.wav` 下保存最后一次生成:

```shell
python server.py --enable-modules=rvc,edge-tts --rvc-save-file
```

#### 基于表情的动态语音

##### 1. 配置 RVC 模型

在您的 RVC 模型文件夹中,为每个分类表情(例如,愤怒、恐惧、喜悦、爱、悲伤、惊讶)准备单独的 `.pth` 和 `.index` 文件。

##### 2. 启用模块

启用 RVC 和分类模块:

```shell
python server.py --enable-modules=rvc,classify
```

##### 3. 使用 RVC 模块

其余设置与单独使用 RVC 模块类似(如上所述)。

## 训练您自己的 RVC 模型

### 使用 Deffcolony 的 RVC Easy Menu(仅限 Windows)

自动安装和启动 Mangio-RVC: https://github.com/deffcolony/rvc-easy-menu

#### 1. 克隆存储库

将存储库克隆到您想要的位置:

```shell
git clone https://github.com/deffcolony/rvc-easy-menu.git
```

#### 2. 启动 RVC-Launcher.bat

* 打开 `RVC-Launcher.bat` 文件。
* 选择选项 1 以安装 RVC。

#### 3. 完成安装

在提示时,安装所需的包和依赖项。

#### 4. 打开 WebUI 进行语音训练

安装后,选择选项 2 以打开用于语音训练的 WebUI。

### Mangio-RVC: 训练语音模型

***数据集准备***:

**1. 准备音频**:

* 将您想要训练的音频放在 `datasets` 文件夹中。
* 确保音频没有背景噪音 - 只需要原始语音。
* 音频越长,输出质量越好。

***WebUI 训练***:

**1. 访问训练选项卡**:

* 单击 WebUI 中的训练选项卡。

**2. 配置实验**:

* 输入实验名称(例如,`my-epic-voice-model`)。
* 将版本设置为 v2。

**3. 处理数据和提取特征**:

* 单击"处理数据"和"特征提取"。
* 将"保存频率"设置为 50。

**4. 训练参数**:

* 将"总训练轮数"设置为 300。
* 单击"训练特征索引"和"训练模型"。
