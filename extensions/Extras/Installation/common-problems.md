---
label: Common Problems
icon: bug
---

# Extras 安装常见问题

本页面列出了安装 SillyTavern Extras 时遇到的常见问题和解决方案。

---
在您的操作系统上本地安装 Extras 可能很困难或不可能（尤其是 Termux）。

#### 使用 [官方 Extras Colab](https://colab.research.google.com/github/SillyTavern/SillyTavern/blob/release/colab/GPU.ipynb)

* 设置简单
* 免费使用
* 不需要 Colab GPU 额度（使用 `use_cpu` 选项）
* 详情请参见 [Colab 指南页面](/extensions/Extras/running-extras-in-colab.md)。

---

### 错误：在 Linux 上无法导入 'talkinghead' 模块

由于与 Colab 不兼容，它需要安装一个额外的包，所以不会自动安装。在安装其他要求后运行以下命令：

`pip install wxpython`

### Extras 服务器无法连接到 AUTOMATIC1111 的 Stable Diffusion Web UI

> 无法连接到远程 SD 后端 <http://127.0.0.1:7860>！禁用 SD 模块...

**确保您启动 Stable Diffusion 的 webui-user.bat 在 COMMANDLINE_ARGS 变量中包含 --api 命令行选项。**

在您的 "webui-user.bat" 中找到并替换该行：`set COMMANDLINE_ARGS=--api`

![应该是这样的](/static/extensions/sd-user.png)

如果 SD Web UI 的 API 模式被禁用，Extras 服务器将无法建立连接，您将无法生成图像！

#### 仍然不工作？

确保您按照正确的顺序启动所有程序，等待每个程序完成加载后再进行下一步：

1. Stable Diffusion Web UI
2. SillyTavern Extras
3. SillyTavern

如果在加载后启动，extras 服务器无法重新连接到 Stable Diffusion API。

### 安装 ChromaDB 时出现 hnswlib wheel 构建错误

> ERROR: Could not build wheels for hnswlib, which is required to install pyproject.toml-based projects

在安装 ChromaDB 模块之前，您必须先执行`以下操作之一`：

* 安装 Visual C++ build tools：<https://visualstudio.microsoft.com/visual-cpp-build-tools/>
* 使用 conda 安装 `hnswlib` 包：`conda install -c conda-forge hnswlib`

---

### 在 Mac 上安装 Python 要求时出错

> ERROR: No matching distribution found for torch==2.0.0+cu117

Mac 不支持 CUDA，所以应该安装不带 CUDA 支持的 torch 包。

请使用 `requirements-silicon.txt` 文件来安装要求。

---

### 缺少模块？

* 您必须在 Python 命令行中使用 `--enable-modules` 修饰符指定模块名称列表。
* 参见[模块](/extensions/Extras/Installation.md#决定使用哪个模块)部分。

---

### API 密钥框是用来做什么的？

* SillyTavern 扩展面板中的 API 密钥框仅在以下情况下使用：
  * 在您的 Extras 安装文件夹中创建了一个名为 `api_key.txt` 的文本文件，其中包含您选择的 Extras "密码"。
  * 使用 `--secure` 命令行参数启动 extras。
* 这使得 Extras API "密码锁定"，只有在 API 密钥框中有该密钥的用户才能访问它。
* 这主要对想要部署自己的公共 Extras（colab 等）的人有用。
* 在自己的电脑上运行 Extras 供个人使用的用户不应在 API 密钥框中输入任何内容。

### 移动设备/Android/Termux 怎么办？🤔

* 社区中有一些人通过 Termux 上的 Ubuntu 成功运行 Extras。
* 但是，Extras 并不是为移动设备支持而设计的。
* 不会为在 Android 设备上运行 Extras 的人提供支持。
* 请将您的所有问题直接提交给下面链接的指南的作者。

#### ❗ 这是不受支持的

<https://rentry.org/STAI-Termux#downloading-and-running-tai-extras>
