---
route: /extensions/talkinghead/
tags: ['obsolete']
---


# 会说话的头像

### 这是什么？

一个为 AITuber 实现的会说话的动漫头像 3 演示。它具有以下功能：

- 从单个静态图像生成随机的类 Live 2D 动作。
- 与任何 TTS 输出的声音进行口型同步。

这个扩展包含了"从单张图像生成会说话的动漫头像 3：现在还包括身体"项目的原始演示程序。顾名思义，该项目允许您为动漫角色制作动画，而且只需要该角色的一张图片即可。有两个演示程序：

manual_poser 让您通过图形用户界面操控角色的面部表情、头部旋转、身体旋转和呼吸时的胸部扩张，这样您可以将它们保存为默认表情，如开心、悲伤、喜悦等。
ifacialmocap_puppeteer 让您将自己的面部动作转移到动漫角色上。

### 硬件要求

您可以使用 CPU 或 GPU 模式（默认为 CPU）。但是，在 CPU 模式下预计约 1 FPS，而在 GPU 模式下，使用 RTX3060 可以获得约 9-10 FPS。

ifacialmocap_puppeteer 需要一台能够从视频流计算混合形状参数的 iOS 设备。这意味着设备必须能够运行 iOS 11.0 或更高版本，并且必须有 TrueDepth 前置摄像头。（更多信息请参见此页面。）换句话说，如果您有 iPhone X 或更好的设备，就应该没问题。

### 如何使用

您必须使用以下模块启动 extras 才能使 talkinghead 工作：`classify` 和 `talkinghead`！
classify 是处理 talkinghead.png 文件所必需的。此外，您还可以使用 `--talkinghead-gpu` 将混合模型加载到 GPU 内存中，使动画速度提高 10 倍。强烈建议使用 GPU 加速！默认情况下，程序启动后会加载默认图像 SillyTavern-extras\talkinghead\tha3\images\lambda_00.png。您可以通过访问 http://localhost:5100/api/talkinghead/result_feed 或 `您的扩展 URL:端口/api/talkinghead/result_feed` 来验证它是否正常工作。

- 服务器启动后，转到扩展 API 选项卡并连接。然后只需选择一个角色卡进行加载。（启动 server.py 时使用 `--enable-modules=classify,talkinghead --talkinghead-gpu`）

- 现在选择角色表情，如果您勾选 talkinghead 图像类型框，脚本将用 `您的扩展 URL:端口/api/talkinghead/result_feed` 的结果替换您当前的角色表情。取消勾选该框应该会将图像恢复为原始表情，但有时您需要向聊天发送新消息来"重新加载"图像。

- 如果角色目录中没有 talkinghead.png 文件，它将只显示默认图像或最后一个有 talkinghead.png 文件的角色卡。当角色卡更改时，动画源图像也会更改。

- 现在打开角色表情，滚动到 talkinghead 图像，上传一个符合下面"输入图像的约束条件"部分要求的图像文件。

- 然后勾选并取消勾选 talkinghead 框以重新加载角色。如果图像看起来很奇怪，可能是因为它不是透明的/没有 alpha 层。否则，请按照下面的说明和模板操作。

### 输入图像的约束条件
为了使系统正常工作，输入图像必须遵守以下约束条件：

分辨率应为 512 x 512。（如果程序收到任何其他尺寸的输入图像，它会将图像调整为此分辨率并以此分辨率输出。）
必须有 alpha 通道。
必须只包含一个人形角色。
角色应该站直并面向前方。
角色的手应该在头部以下且远离头部。
角色的头部应该大致包含在图像上半部分中间的 128 x 128 框内。
不属于角色的所有像素（即背景像素）的 alpha 通道必须为 0。

![输入约束](/static/input_spec.png)

### 高级部分

### Python 环境

除了基本功能（app.py）外，manual_poser 和 ifacialmocap_puppeteer 都可以作为桌面应用程序使用。要运行它们，您需要设置一个用于运行 Python 语言程序的环境。该环境需要以下软件包：

* Python >= 3.8
* PyTorch >= 1.11.0 with CUDA support
* SciPY >= 1.7.3
* wxPython >= 4.1.1
* Matplotlib >= 3.5.1

一种方法是安装 Anaconda 并在 shell 中运行以下命令：

> conda create -n talking-head-anime-3-demo python=3.8
> conda activate talking-head-anime-3-demo
> conda install pytorch torchvision torchaudio cudatoolkit=11.3 -c pytorch
> conda install scipy
> pip install wxpython
> conda install matplotlib

### 额外的混合模型

只包含一个（最轻量级的）模型，如果您想要额外的混合模型，需要从 https://www.dropbox.com/s/y7b8jl4n2euv8xe/talking-head-anime-3-models.zip?dl=0 下载模型文件并解压到 SillyTavern-extras\talkinghead\tha3\models 文件夹。最终，数据文件夹应该如下所示：

+ tha3
  + models
    + separable_float
      - editor.pt
      - eyebrow_decomposer.pt
      - eyebrow_morphing_combiner.pt
      - face_morpher.pt
      - two_algo_face_body_rotator.pt
    + separable_half
      - editor.pt
          :
      - two_algo_face_body_rotator.pt
    + standard_float
      - editor.pt
          :
      - two_algo_face_body_rotator.pt
    + standard_half
      - editor.pt
          :
      - two_algo_face_body_rotator.pt

模型文件以知识共享署名 4.0 国际许可证分发，这意味着您可以将它们用于商业目的。但是，Pramook Khungurn. Talking Head(?) Anime from a Single Image 3: Now the Body Too. <https://github.com/pkhungurn/talking-head-anime-3-demo>，是创作者。

### 运行 manual_poser 桌面应用程序
打开 shell。将工作目录更改为仓库的根目录。然后运行：

> python tha3/app/manual_poser.py
注意，在运行上述命令之前，您可能需要激活包含所需包的 Python 环境。

> conda activate extras
如果您尚未激活环境。
