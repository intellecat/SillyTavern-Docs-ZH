---
route: /extensions/talkinghead/
tags: ['obsolete']
---

# talkinghead

!!!warning
**SILLYTAVERN 1.12.13 中已停止对 TALKINGHEAD 的支持。此页面保留用于历史目的。**
!!!

### 它是什么?

用于 AITuber 的 Talking Head Anime 3 Demo 的实现。它具有以下功能:

- 从单个静态图像生成随机的类似 Live 2D 的动作动作。
- 与任何 TTS 输出的声音输出同步唇形。

此扩展包含 Talking Head(?) Anime from a Single Image 3: Now the Body Too 项目的原始演示程序。顾名思义,该项目允许您为动漫角色制作动画,您只需要该角色的单张图像即可。有两个演示程序:

manual_poser 允许您通过图形用户界面操纵角色的面部表情、头部旋转、身体旋转和由于呼吸引起的胸部扩张,以便您可以将它们保存为默认表情,即快乐、悲伤、喜悦等。
ifacialmocap_puppeteer 允许您将面部动作转移到动漫角色。

### 硬件要求

您可以使用 CPU 或 GPU 模式(默认为 CPU)。但是,在 CPU 模式下,预计约 1 FPS,在 RTX3060 上的 GPU 模式下,我获得约 9-10 FPS。

ifacialmocap_puppeteer 需要一个能够从视频源计算混合形状参数的 iOS 设备。这意味着设备必须能够运行 iOS 11.0 或更高版本,并且必须具有 TrueDepth 前置摄像头。(有关更多信息,请参阅此页面。)换句话说,如果您有 iPhone X 或更好的产品,您应该已经准备就绪。

### 如何使用

您必须使用以下模块启动 extras 才能使 talkinghead 工作:`classify` 和 `talkinghead`!
classify 是处理 talkinghead.png 文件所必需的。此外,您还可以使用 `--talkinghead-gpu` 将混合模型加载到 GPU 内存中,使动画速度提高 10 倍。强烈建议使用 GPU 加速!默认情况下,一旦程序启动,它将加载默认图像 SillyTavern-extras\talkinghead\tha3\images\lambda_00.png。您可以通过访问 http://localhost:5100/api/talkinghead/result_feed 或 `YOUR EXT URL:PORT/api/talkinghead/result_feed` 来验证它是否正常工作。

- 一旦服务器启动,转到扩展 API 选项卡并连接。然后简单地选择一个角色卡来加载。(`--enable-modules=classify,talkinghead --talkinghead-gpu` 启动 server.py 时)

- 现在选择角色表情,如果您选中图像类型 talkinghead 框,脚本将用 `YOUR EXT URL:PORT/api/talkinghead/result_feed` 的结果替换您当前的角色表情,取消选中该框应该将图像返回到原始表情,但是有时您必须向聊天发送新消息以"重新加载"图像。

- 如果角色目录中没有 talkinghead.png 文件,它将简单地显示默认图像或具有 talkinghead.png 文件的最后一个角色卡。当角色卡更改时,动画源图像会更改。

- 现在打开角色表情,向下滚动到 talkinghead 图像并上传符合下面"输入图像约束"部分中要求的图像文件。

- 然后选中和取消选中 talkinghead 框以重新加载角色。如果图像看起来很奇怪,可能是因为它不是透明的/没有 alpha 层。否则,请遵循下面的说明和模板。

### 输入图像的约束
为了使系统正常工作,输入图像必须遵守以下约束:

它应该是 512 x 512 的分辨率。(如果程序接收到任何其他大小的输入图像,它将把图像调整为此分辨率并以此分辨率输出。)
它必须有一个 alpha 通道。
它必须只包含一个类人角色。
角色应该直立站立并面向前方。
角色的手应该在头部下方并远离头部。
角色的头部应该大致包含在图像上半部分中间的 128 x 128 框中。
所有不属于角色的像素(即背景像素)的 alpha 通道必须为 0。

![输入约束](/static/input_spec.png)

### 高级部分

### Python 环境

除了基本功能(app.py)之外,manual_poser 和 ifacialmocap_puppeteer 都可作为桌面应用程序使用。要运行它们,您需要设置一个用于运行用 Python 语言编写的程序的环境。该环境需要具有以下软件包:

* Python >= 3.8
* PyTorch >= 1.11.0 with CUDA support
* SciPY >= 1.7.3
* wxPython >= 4.1.1
* Matplotlib >= 3.5.1

一种方法是安装 Anaconda 并在 shell 中运行以下命令:

> conda create -n talking-head-anime-3-demo python=3.8
> conda activate talking-head-anime-3-demo
> conda install pytorch torchvision torchaudio cudatoolkit=11.3 -c pytorch
> conda install scipy
> pip install wxpython
> conda install matplotlib

### 附加混合模型

只包含一个(最轻的)模型,如果您想要额外的混合模型,您需要从 https://www.dropbox.com/s/y7b8jl4n2euv8xe/talking-head-anime-3-models.zip?dl=0 下载模型文件并将其解压缩到 SillyTavern-extras\talkinghead\tha3\models 文件夹。最后,数据文件夹应该看起来像:

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

模型文件采用 Creative Commons Attribution 4.0 International License 分发,这意味着您可以将它们用于商业目的。但是,Pramook Khungurn. Talking Head(?) Anime from a Single Image 3: Now the Body Too. <https://github.com/pkhungurn/talking-head-anime-3-demo> 是创建者。

### 运行 manual_poser 桌面应用程序
打开 shell。将工作目录更改为存储库的根目录。然后,运行:

> python tha3/app/manual_poser.py
请注意,在运行上述命令之前,您可能必须激活包含所需包的 Python 环境。

> conda activate extras
如果您尚未激活环境。
