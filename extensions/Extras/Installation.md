---
icon: gear
label: Local Installation
---

# Extras 安装

本页面包含在本地设备上安装 SillyTavern Extras 的说明。

!!! Discontinued
Extras 项目已于2024年4月停止维护，不会再收到任何新的更新或模块。绝大多数模块已在 SillyTavern 主应用程序中原生提供。您仍然可以安装和使用它，但如果遇到任何问题，请不要期望能得到及时的支持。
!!!


在您的操作系统上本地安装 Extras 可能很困难或不可能（尤其是 Termux）。

#### 使用 [官方 Extras Colab](https://colab.research.google.com/github/SillyTavern/SillyTavern/blob/release/colab/GPU.ipynb)

* 设置简单
* 免费使用
* 不需要 Colab GPU 额度（使用 `use_cpu` 选项）
* 详情请参见 [Colab 指南页面](/extensions/Extras/running-extras-in-colab.md)。

---

## 安装方法

### MiniConda（推荐）

推荐这种方法是因为 Conda 为 Extras 需要的包创建了一个"虚拟环境"，这样它们就不会影响您系统范围的 Python 设置。

1. 安装 [Miniconda](https://docs.conda.io/en/latest/miniconda.html)

    _（重要！）阅读 [如何使用 Conda](https://docs.conda.io/projects/conda/en/latest/user-guide/getting-started.html)_

2. 安装 [git](https://git-scm.com/downloads)

    _（一开始就用 git 安装 SillyTavern 的大神可以跳过这一步！）_
    
    在您安装好这两个之后...
    
    在 `CONDA 命令提示符窗口` 中 `逐个` 输入/粘贴下面的命令，每个命令后按 `Enter`。

3. 创建一个新的 Conda 环境（让我们称它为 `extras`）：

    `conda create -n extras`

4. 激活新环境

    `conda activate extras`（您应该会在命令提示符左侧看到 `(extras)` 出现）

5. 安装所需的系统包（这需要一些时间）

    `conda install python=3.11 git`

6. 克隆 Extras GitHub 仓库

    `git clone https://github.com/SillyTavern/SillyTavern-extras`

7. 导航到您克隆的 Extras 仓库

    `cd SillyTavern-extras`

8. 使用以下命令之**一**安装 Extras 的依赖（这又需要一些时间）：

   * `pip install -r requirements.txt` - 用于基本功能
   * `pip install -r requirements-rvc.txt` - 用于实时语音克隆
   * `pip install -r requirements-coqui.txt` - 用于 Coqui TTS（不推荐）

    如果在此步骤遇到错误，请参见 [常见问题](/extensions/Extras/Installation/common-problems.md)！

9. 参见下面的"安装后运行 Extras"

---

### 系统范围安装

这更容易，但会影响您系统范围的 Python 安装。

如果您使用许多具有不同需求的 Python 程序，这可能会导致冲突。

如果这是您第一次接触任何 Python 相关的东西，那应该不是问题。

1. 安装 Python 3.11：<https://www.python.org/downloads/release/python-3115/>
2. 安装 git：<https://git-scm.com/downloads>
3. 打开命令提示符窗口，转到一个您拥有完全访问权限的文件夹。
4. 克隆仓库：输入 `git clone https://github.com/SillyTavern/SillyTavern-extras`，按 Enter。
5. 克隆完成后，输入 `cd SillyTavern-extras`，按 Enter。
6. 输入 `python -m pip install -r requirements.txt`
7. 参见下面的"安装后运行 Extras"

---

## 安装后运行 Extras

### 确认已启用扩展

1. 在文本编辑器中打开 ST 基本安装文件夹中的 `config.yaml` 文件。
2. 查找包含 `enableExtensions` 的行。
3. 确保该行是 `true`，而不是 `false`。

### 决定使用哪个模块

（这只需要做一次）

* Extras 总是通过 Python 命令行启动。
* `python server.py` 是最基本的，但它不启用任何有用的模块。
* 要启用模块，您必须使用 `--enable-modules=` 修饰符，后面跟着以逗号分隔的模块名称列表

示例：`python server.py --enable-modules=caption,summarize,classify`

这将启用图像说明、聊天摘要和实时更新角色表情。

下表描述了每个模块。

| 名称         | 描述                                                              |
|--------------|------------------------------------------------------------------|
| `caption`    | 图像说明                                                         |
| `summarize`  | 文本摘要                                                         |
| `classify`   | 文本情感分类                                                     |
| `sd`         | Stable Diffusion 图像生成                                        |
| `silero-tts` | [Silero TTS 服务器](https://github.com/ouoertheo/silero-api-server) |
| `edge-tts`   | [Microsoft Edge TTS 客户端](https://github.com/rany2/edge-tts)    |
| `chromadb`   | 向量存储服务器                                                   |
| `coqui-tts`  | Coqui TTS                                                        |
| `rvc`        | 实时语音克隆                                                     |

* 决定您想要添加到 Python 命令行的模块。
* 它们将在下一步中使用。

**注意：您的 Python 命令的模块列表中`不能有任何空格`！**

### 启动 Extras 服务器

在 Extras 安装文件夹中的命令提示符窗口中...

1. 确保您的 conda 环境处于活动状态（如果您使用了 Conda 安装方法）
2. 如果环境未激活，输入 `activate extras`。
3. 输入 `python server.py --enable-modules=您的,选定的,模块,列表,在这里`
4. extras 服务器将加载。
5. 一段时间后，它会在最后显示一个 URL。对于本地安装，默认为 `http://localhost:5100`。
6. 复制 API URL。

### 将 ST 连接到 Extras 服务器

1. 启动您的 SillyTavern 服务器，并在浏览器中查看 SillyTavern 界面。
2. 打开扩展面板（通过页面顶部的'堆叠块'图标）
3. 将 API URL 粘贴到输入框中。
4. 点击 `连接`。

要再次运行 Extras，只需在命令提示符中激活环境并运行这些命令。

`conda activate extras`，按 Enter。
`python server.py`，按 Enter。

确保添加您的设置所需的 server.py 的其他选项（见下文）。

## 创建 .bat 文件以便于启动

这是可选的，仅适用于 Windows，但在 MacOS 上应该也可以做类似的事情。

1. 查看您的 Windows 桌面
2. 右键单击，选择 `新建`，然后点击 `文本文档`
3. 桌面上会出现一个新文件，要求输入名称。
4. 将文件命名为 `STExtras.txt`
5. 在文本编辑器中打开新创建的文件。
6. 将以下代码粘贴到其中：

    ```
    cd C:\_your_\_full_\_Extras_\_folder_\_path_\
    call conda activate extras
    python server.py --enable-modules=您的,选定的,模块,列表,在这里,不要,有,空格
    call conda deactivate
    pause
    ```

7. 将占位符文件夹路径替换为您实际的 Extras 安装文件夹路径。
8. 将 python 命令行替换为您实际的命令行
9. 用新名称 `STExtras.bat` 保存文件（在大多数文本编辑器中使用 `文件` >> `另存为`）

现在您只需双击这个 .bat 文件就可以轻松启动 Extras。

如果您想更改模块列表（或 extras 服务器的任何其他命令行修饰符），只需编辑 .bat 文件中的 python 命令。
