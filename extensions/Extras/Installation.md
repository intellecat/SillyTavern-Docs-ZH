---
icon: gear
label: 本地安装
route: /extensions/Extras/Installation/
---

# Extras 安装

本页包含在本地设备上安装 SillyTavern Extras 的说明。

!!! Discontinued
Extras 项目已于 2024 年 4 月停止维护，不会再收到任何新的更新或模块。绝大多数模块已在主 SillyTavern 应用程序中原生可用。您仍然可以安装和使用它，但如果遇到任何问题，请不要期望获得即时支持。
!!!

在您的操作系统上本地安装 Extras 可能很困难甚至不可能(尤其是 Termux)。

## 使用[官方 Extras Colab](https://colab.research.google.com/github/SillyTavern/SillyTavern/blob/release/colab/GPU.ipynb)

* 设置简单
* 免费使用
* 不需要 Colab GPU 积分(使用 `use_cpu` 选项)
* 有关详细信息，请参阅 [Colab 指南页面](/extensions/Extras/Installation.md#running-extras-in-colab)。

### 在 Colab 中运行 Extras

* 打开[官方 Extras Colab](https://colab.research.google.com/github/SillyTavern/SillyTavern/blob/release/colab/GPU.ipynb)
* 选择所需的"Extra"选项
* 选择 `use_cpu` 以在不需要 GPU 积分的情况下运行 Extras
  * 这将使 Stable Diffusion 变慢，但其他一切都会正常运行
* 非必需，但推荐:选择 `secure` 选项以生成 API 密钥来保护您的共享实例。
* 点击左侧的开始按钮(看起来像三角形"播放"按钮)
* 等待它完成加载所有内容
* 在输出底部查找 `trycloudflare.com` 链接。忽略 localhost 链接，它不起作用(我们试过了!)。
* 它将以 `Running on` 文本开头
* 复制该行下列出的 API URL 链接。(**不要复制 'localhost' URL，使用另一个**)
* 启动支持扩展的 SillyTavern:(如有必要，在 `config.yaml` 中将 `enableExtensions` 设置为 `true`)
* 导航到 SillyTavern 的扩展菜单(点击页面顶部的"堆叠方块"图标)。
* 将 API URL 粘贴到顶部的框中。(**不是 API 密钥框**)
* 如果您没有启用 `secure` 选项，请确保在使用官方 colab 时 API 密钥框完全为空。
* 如果您启用了 `secure` 选项，请将生成的 API 密钥粘贴到 API 密钥框中。
* API 密钥将出现在 colab 的控制台输出中，例如:`Your API key is fee2f3f559`
* 点击"连接"

---

## 本地安装方法

### MiniConda(推荐)

推荐使用此方法，因为 Conda 会为 Extras 需求包创建一个"虚拟环境"，使它们不会影响您的系统级 Python 设置。

1. 安装 [Miniconda](https://docs.conda.io/en/latest/miniconda.html)

    _(重要!)阅读[如何使用 Conda](https://docs.conda.io/projects/conda/en/latest/user-guide/getting-started.html)_

2. 安装 [git](https://git-scm.com/downloads)

    _(一开始就用 git 安装 SillyTavern 的大佬们可以跳过此步骤!)_

    安装完这两者后...

    在 `CONDA 命令提示符窗口`中`逐个`输入/粘贴下面的命令，每个命令后按 `Enter`。

3. 创建一个新的 Conda 环境(我们称之为 `extras`):

    `conda create -n extras`

4. 激活新环境

    `conda activate extras`(您应该看到 `(extras)` 出现在命令提示符的左侧)

5. 安装所需的系统包(这将需要一些时间)

    `conda install python=3.11 git`

6. 克隆 Extras GitHub 仓库

    `git clone https://github.com/SillyTavern/SillyTavern-extras`

7. 导航到克隆的 Extras 仓库

    `cd SillyTavern-extras`

8. 使用以下命令之一安装 Extras 的需求(将再次需要时间):

   * `pip install -r requirements.txt` - 用于基本功能
   * `pip install -r requirements-rvc.txt` - 用于实时语音克隆
   * `pip install -r requirements-coqui.txt` - 用于 Coqui TTS(不推荐)

    如果在此步骤遇到错误，请参阅[常见问题](/extensions/Extras/Installation.md#extras-install-common-problems)页面!

9. 请参阅下面的"安装后运行 Extras"

---

### 系统级安装

这更容易，但会影响您的系统级 Python 安装。

如果您使用许多具有不同需求的 Python 程序，这可能会导致冲突。

如果这是您第一次接触与 Python 相关的任何内容，那应该不是问题。

1. 安装 Python 3.11: <https://www.python.org/downloads/release/python-3115/>
2. 安装 git: <https://git-scm.com/downloads>
3. 打开命令提示符窗口并转到您具有完全访问权限的文件夹。
4. 克隆仓库:`git clone https://github.com/SillyTavern/SillyTavern-extras`，按 Enter。
5. 克隆完成后，输入 `cd SillyTavern-extras`，按 Enter。
6. 输入 `python -m pip install -r requirements.txt`
7. 请参阅下面的"安装后运行 Extras"

---

## 安装后运行 Extras

### 确认扩展已启用

1. 在文本编辑器中打开名为 `config.yaml` 的文件。该文件位于 ST 的基本安装文件夹中。
2. 查找读取 `enableExtensions` 的行。
3. 确保该行有 `true`，而不是 `false`。

### 决定使用哪个模块

(这只需要做一次)

* Extras 总是使用 Python 命令行启动。
* `python server.py` 是最低要求，但它不启用任何有用的模块。
* 要启用模块，您必须使用 `--enable-modules=` 修饰符，带有逗号分隔的模块名称列表

示例:`python server.py --enable-modules=caption，summarize，classify`

这将启用图像描述、聊天摘要和实时更新角色表情。

下表描述了每个模块。

| 名称         | 描述                                                         |
|--------------|-------------------------------------------------------------|
| `caption`    | 图像描述                                                     |
| `summarize`  | 文本摘要                                                     |
| `classify`   | 文本情感分类                                                 |
| `sd`         | Stable Diffusion 图像生成                                    |
| `silero-tts` | [Silero TTS 服务器](https://github.com/ouoertheo/silero-api-server) |
| `edge-tts`   | [Microsoft Edge TTS 客户端](https://github.com/rany2/edge-tts) |
| `chromadb`   | 向量存储服务器                                               |
| `coqui-tts`  | Coqui TTS                                                   |
| `rvc`        | 实时语音克隆                                                 |

* 决定要将哪些模块添加到 Python 命令行。
* 它们将在下一步中使用。

**注意:Python 命令的模块列表中不能有任何空格!**

### 启动 Extras 服务器

在 Extras 安装文件夹内的命令提示符窗口中...

1. 确保您的 conda 环境处于活动状态(如果您使用了 Conda 安装方法)
2. 如果环境未激活，输入 `activate extras`。
3. 输入 `python server.py --enable-modules=YOUR，SELECTED，MODULE，LIST，HERE`
4. extras 服务器将加载。
5. 过一会儿，它会在最后显示一个 URL。对于本地安装，默认为 `http://localhost:5100`。
6. 复制 API URL。

### 将 ST 连接到 Extras 服务器

1. 启动您的 SillyTavern 服务器，并在浏览器中查看 SillyTavern 界面。
2. 打开扩展面板(通过页面顶部的"堆叠方块"图标)
3. 将 API URL 粘贴到输入框中。
4. 点击 `连接`。

要再次运行 Extras，只需在命令提示符中激活环境并运行这些命令。

`conda activate extras`，按 Enter。
`python server.py`，按 Enter。

确保使用您的设置所需的 server.py 的附加选项(见下文)。

## 制作 .bat 文件以便轻松启动

这是可选的，仅适用于 Windows，但在 MacOS 上应该可以做类似的事情。

1. 查看您的 Windows 桌面
2. 右键单击，选择 `新建`，然后点击 `文本文档`
3. 您的桌面上将出现一个新文件，要求输入名称。
4. 将文件命名为 `STExtras.txt`
5. 在文本编辑器中打开新创建的文件。
6. 将以下代码粘贴到其中:

    ```
    cd C:\_your_\_full_\_Extras_\_folder_\_path_\
    call conda activate extras
    python server.py --enable-modules=YOUR，SELECTED，MODULE，LIST，HERE，WITH，NO，SPACES
    call conda deactivate
    pause
    ```

7. 将占位符文件夹路径替换为您的实际 Extras 安装文件夹路径。
8. 将 python 命令行替换为您的实际命令行
9. 使用新名称 `STExtras.bat` 保存文件(在大多数文本编辑器中使用 `文件` >> `另存为`)

您现在可以简单地双击此 .bat 文件以轻松启动 Extras。

如果您想更改模块列表(或 extras 服务器的任何其他命令行修饰符)，只需编辑 .bat 文件内的 python 命令。

## Extras 安装常见问题

本节列出了安装 SillyTavern Extras 时遇到的常见问题和问题。

### 错误:无法在 Linux 上导入 'talkinghead' 模块

它需要安装额外的软件包，因为由于与 Colab 不兼容，它不会自动安装。在安装其他需求后运行此命令:

`pip install wxpython`

### Extras 服务器无法连接到 AUTOMATIC1111 的 Stable Diffusion Web UI

> Could not connect to remote SD backend at <http://127.0.0.1:7860>! Disabling SD module...

**确保您启动 Stable Diffusion 的 webui-user.bat 在 COMMANDLINE_ARGS 变量中包含 --api 命令行选项。**

在您的 "webui-user.bat" 中找到并替换该行:`set COMMANDLINE_ARGS=--api`

![应该看起来像这样](/static/extensions/sd-user.png)

如果 SD Web UI 禁用了 API 模式，Extras 服务器将无法建立连接，您将无法生成图像!

#### 仍然不起作用?

确保您按正确的顺序启动所有内容，等待每个程序完成加载后再继续下一步:

1. Stable Diffusion Web UI
2. SillyTavern Extras
3. SillyTavern

如果 Extras 服务器在 Stable Diffusion API 之后加载，则无法重新连接。

### 安装 ChromaDB 时 hnswlib wheel 构建错误

> ERROR: Could not build wheels for hnswlib， which is required to install pyproject.toml-based projects

在安装 ChromaDB 模块之前，您必须先执行以下`操作之一`:

* 安装 Visual C++ 构建工具: <https://visualstudio.microsoft.com/visual-cpp-build-tools/>
* 使用 conda 安装 `hnswlib` 包:`conda install -c conda-forge hnswlib`

---

### 在 Mac 上安装 Python 需求时出错

> ERROR: No matching distribution found for torch==2.0.0+cu117

Mac 不支持 CUDA，因此应该安装没有 CUDA 支持的 torch 包。

改用 `requirements-silicon.txt` 文件安装需求。

---

### 缺少模块?

* 您必须在 Python 命令行中使用 `--enable-modules` 修饰符指定模块名称列表。
* 请参阅[模块](/extensions/Extras/Installation.md#decide-which-module-to-use)部分。

---

### API 密钥框是用来做什么的?

* SillyTavern 扩展面板中的 API 密钥框仅在以下情况下使用:
  * 在 Extras 安装文件夹中创建了一个名为 `api_key.txt` 的文本文件，其中包含您选择的 Extras "密码"。
  * 使用 `--secure` 命令行参数启动 extras。
* 这使 Extras API "密码锁定"，因此只有在 API 密钥框中拥有该密钥的用户才能访问它。
* 这主要对想要公开部署 Extras(colab 等)的人有用。
* 在 PC 上运行 Extras 供个人使用的用户不应在 API 密钥框中输入任何内容。

### 移动/Android/Termux 怎么样?🤔

* 社区中有一些人通过 Termux 上的 Ubuntu 在手机上成功运行 Extras。
* 但是，Extras 并非考虑移动支持而制作的。
* 不会为在 Android 设备上运行 Extras 的人提供支持。
* 请将所有问题直接发送给下面链接的指南的创建者。

#### ❗ 这是不支持的

<https://rentry.org/STAI-Termux#downloading-and-running-tai-extras>
