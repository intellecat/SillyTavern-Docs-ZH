---
order: TTS-XTTS
route: /extensions/xtts/
---

# 使用语音克隆的 XTTS

您好！您是否被那些展示 AI 文本转语音技术发展程度的 Reddit 帖子震撼到了？

是否对给您的机器人老婆/老公一个闪亮的新语音调制器感到兴奋？

别担心，这项令人惊叹的突破性技术已经在您的本地 SillyTavern 中可用，您只需要一个简单的...

## 前提条件

1. 最新版本的 SillyTavern。
2. 安装 [Miniconda](https://docs.conda.io/projects/miniconda/en/latest/miniconda-install.html)。
3. (Windows) 安装 [Visual C++ Build Tools](https://visualstudio.microsoft.com/visual-cpp-build-tools/)。
4. 用于克隆的语音片段 WAV 文件（每个文件约 10 秒）。文件要求：PCM、单声道、22050Hz、16 位（通过 Audacity 转换）。
5. 创建一个包含"speakers"和"output"子文件夹的文件夹。将 WAV 文件放入"speakers"中。

示例文件夹结构：
```
C:\xtts
  - speakers
    - alice.wav
    - bob.wav
  - output
```

## 安装

[daswer123](https://github.com/daswer123) 制作了一个 API 服务器，可以在您的计算机上运行 XTTSv2 模型并连接到 SillyTavern 的 TTS 扩展。

它完全独立于 Extras API，并使用单独的环境。

**非常重要：**不要将以下要求安装到您的 Extras 环境或系统 Python 中。
这会破坏您的其他包，进行不必要的降级等。

以下说明使用 Miniconda 提供，但您也可以使用 venv（此处不涉及）。
打开 Anaconda 命令提示符并逐行按照说明操作。

### 启动并运行服务器

1. 导航到您在前提条件步骤 4 中创建的文件夹。
    ```
    cd C:\xtts
    ```
2. 创建一个新的 conda 环境。从现在开始，我们将其称为 `xtts`。
    ```
    conda create -n xtts
    ```
3. 激活新创建的环境。
    ```
    conda activate xtts
    ```
4. 在您的环境中安装 Python 3.10。当提示时确认"y"。
    ```
    conda install python=3.10
    ```
5. 安装 XTTS 服务器及其要求。
    ```
    pip install xtts-api-server pydub
    ```
6. 安装 PyTorch。这可能需要一些时间。以下命令安装带有 GPU 加速支持（CUDA）的 PyTorch。
如果您只想使用 CPU 推理，请删除从 `--index-url` 开始的最后一部分。
    ```
    pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118
    ```
7. 在默认主机和端口上启动 XTTS 服务器：<http://localhost:8020>
    ```
    python -m xtts_api_server
    ```
8. 在首次启动期间，将下载模型（约 2 GB）。
不要忘记仔细阅读来自 Coqui AI 的法律声明。开玩笑的，只需再次点击"y"即可。

### 连接到 SillyTavern

1. 打开扩展面板，展开 TTS 菜单，并在提供商列表中选择"XTTSv2"。
2. 在语言下拉列表中选择您的文本转语音语言（如果不是波兰语我会很伤心）。
3. 验证提供商端点指向 <http://localhost:8020> 并且"Available voices"显示您的语音样本列表。
4. 选择任何角色并设置语音样本和角色之间的映射。
如果角色列表为空，多点击几次"Reload"。
5. 根据您的偏好配置其余的 TTS 设置。

### 现在您已经设置完成！

点击任何消息的上下文操作菜单中的喇叭图标，就能听到从扬声器中传出的美丽克隆语音。
生成需要一些时间，即使在高端 RTX GPU 上也不是实时的。

### 流式传输？

使用最新版本的 XTTS 服务器可以使用 HTTP 流式传输，一旦生成的音频可用就获取音频块！

#### 这不适用于 RVC！

音频仍然会生成（假设您使用的是最新版本的 RVC 扩展）并转换，*但不会流式传输*，因为 RVC 需要在开始转换之前有完整的音频文件。流式 RVC 仍在研究中...

#### 如何获得流式传输支持？

1. 将 SillyTavern 更新到最新版本。
2. 将 XTTS 服务器更新到最新版本。

    ```bash
    conda activate xtts
    pip install xtts-api-server --upgrade
    ```

3. 像往常一样启动并将 XTTS 连接到 ST。
4. 在 SillyTavern 中启用"Streaming" XTTS 扩展设置。

### 音频断断续续？

尝试增加"chunk size"设置。

参考：使用 200 的块大小，RTX 3090 可以产生不间断的音频，代价是略微增加音频延迟。

### 如何重启 TTS 服务器？

只需执行安装说明中的步骤 1、3 和 7。

### Android？？

不太可能，它无法运行需要 PyTorch 的应用程序，除非使用一些我们不提供支持的神秘黑魔法。您可以自行尝试，但如果遇到任何问题，我们不会提供支持。

您最好的解决方案是在本地网络上的 PC 上托管 TTS API，只是不要忘记指定要监听的主机和端口 - 请参阅 [README](https://github.com/daswer123/xtts-api-server/blob/main/README.md)。
