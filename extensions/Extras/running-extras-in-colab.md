---
label: Extras via Colab
order: 100
icon: cloud
---
# 在 Colab 中运行 Extras

!!! Discontinued
Extras 项目已于2024年4月停止维护，不会再收到任何新的更新或模块。绝大多数模块已在 SillyTavern 主应用程序中原生提供。您仍然可以安装和使用它，但如果遇到任何问题，请不要期望能得到及时的支持。
!!!

## 说明

* 打开 [官方 Extras Colab](https://colab.research.google.com/github/SillyTavern/SillyTavern/blob/release/colab/GPU.ipynb)
* 选择所需的 "Extra" 选项
* 选择 `use_cpu` 以不需要 GPU 额度运行 Extras
  * 这会使 Stable Diffusion 变慢，但其他一切都会正常运行
* 不是必需的，但建议：选择 `secure` 选项以生成 API 密钥来保护您的共享实例。
* 点击左侧的开始按钮（看起来像一个三角形的'播放'按钮）
* 等待它完成加载所有内容
* 在输出底部寻找 `trycloudflare.com` 链接。忽略 localhost 链接，它不会工作（我们试过了！）。
* 它将以文本 `Running on` 开头
* 复制该行下面列出的 API URL 链接。（**不要**复制'localhost'URL，使用另一个）
* 启动支持扩展的 SillyTavern：（如果需要，在您的 `config.yaml` 中将 `enableExtensions` 设置为 `true`）
* 导航到 SillyTavern 的扩展菜单（点击页面顶部的'堆叠块'图标）。
* 将 API URL 粘贴到顶部的框中。（**不是** API 密钥框）
* 如果您**没有**启用 `secure` 选项，在使用官方 colab 时确保 API 密钥框完全为空。
* 如果您启用了 `secure` 选项，将生成的 API 密钥粘贴到 API 密钥框中。
* API 密钥将出现在 colab 的控制台输出中，例如：`Your API key is fee2f3f559`
* 点击"连接"
