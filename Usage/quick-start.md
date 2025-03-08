---
order: 190
icon: rocket
---

# 快速入门

!!!light
我完全不知道从何下手。请告诉我使用 SillyTavern 最简单最快的方法。 -- *匿名用户*
!!!

您只需几分钟就可以开始使用 SillyTavern。以下是两种简单的入门方式：

* 您可以[使用 AI Horde](#使用-ai-horde-快速入门)，这是免费的。AI Horde 是一个社区驱动的 AI 服务，提供各种 AI 模型的访问。

* 如果您有 OpenAI 账户或想注册一个，您可以[使用 OpenAI](#使用-openai-快速入门)。

## 使用 AI Horde 快速入门

1. 按照[安装指南](/Installation/index.md)安装并启动 SillyTavern。

2. 在 SillyTavern 的引导界面中，为您的角色输入一个名字。这个名字将在聊天中使用。

   ![这是一个可选的标题](/static/quick-start/1_name.png)
3. 点击顶部栏中的 API 连接按钮。

   ![这是一个可选的标题](/static/quick-start/2_api_conn.png)
4. 输入 AI Horde 的 API 密钥。您现在可以使用 `0000000000`，或者从 [AI Horde](https://aihorde.net/) 获取一个免费密钥。

   ![这是一个可选的标题](/static/quick-start/3_horde_key.png)
5. 选择一些要使用的 AI 模型。只需从顶部选择几个即可。您以后随时可以更改它们。

   ![这是一个可选的标题](/static/quick-start/4_horde_models.png)
6. 关闭 API 连接窗口。在底部的聊天框中输入消息并按回车键。

   ![这是一个可选的标题](/static/quick-start/5_msg.png)
7. 您的 AI 将在几分钟内回复。您可以继续与它[聊天](/Usage/Chatting/index.md)。成功！

   ![这是一个可选的标题](/static/quick-start/6_success.png)

## 使用 OpenAI 快速入门

### 安装 SillyTavern

按照[安装指南](/Installation/index.md)安装并启动 SillyTavern。

### 获取 OpenAI 访问权限

1. 注册 OpenAI。
2. 访问 <https://platform.openai.com>
3. 点击右上角的账户图标，然后选择查看 API 密钥。
4. 点击"创建新的密钥"。立即将其复制到某处。**不要分享这个密钥。任何拥有它的人都可以使用您的账户使用 GPT，费用由您承担。**

### 配置 SillyTavern 使用您的 API

1. 在 SillyTavern 的顶部栏中，点击 API 连接。
2. 在 API 下，选择 Chat Completion (OpenAI)。
3. 在 Chat Completion Source 下，选择 OpenAI。
4. 粘贴您在上一步中保存的 API 密钥。
5. 点击连接按钮。确认显示有效。
6. 默认情况下，SillyTavern 将使用 GPT-4 Turbo。您可以选择不同的模型，但请了解相关定价。

### 测试您的设置

1. 在 SillyTavern 的顶部栏中，点击最右侧的角色管理。
2. 选择一个现有角色，如 Seraphina。
3. 在底部的文本框中，向 Seraphina 写些什么，然后按回车键或点击发送按钮。

如果您一切设置正确，几秒钟后，Seraphina 应该会回复。
