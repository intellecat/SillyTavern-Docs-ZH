---
order: 190
icon: rocket
route: /usage/quick-start/
---

# 快速开始

!!!light
我完全不懂。请直接告诉我最简单、最快速的 SillyTavern 使用方法。 -- *匿名*
!!!

您可以在几分钟内开始使用 SillyTavern。以下是两种简单的入门方式：

* 您可以免费[使用 AI Horde](#使用-ai-horde-快速开始)。AI Horde 是一个社区驱动的 AI 服务，提供多种 AI 模型的访问。

* 如果您有 OpenAI 账户或想要注册一个，您可以[使用 OpenAI](#使用-openai-快速开始)。

## 使用 AI Horde 快速开始

1. 按照[安装指南](/Installation/index.md)安装并启动 SillyTavern。

2. 在 SillyTavern 的引导屏幕中，为您的人设输入一个名称。这个名称将在聊天中使用。

   ![This is an optional caption](/static/quick-start/1_name.png)
3. 点击顶部栏中的 API Connections 按钮。

   ![This is an optional caption](/static/quick-start/2_api_conn.png)
4. 输入 AI Horde 的 API 密钥。您现在可以使用 `0000000000`，或从 [AI Horde](https://aihorde.net/) 获取免费密钥。

   ![This is an optional caption](/static/quick-start/3_horde_key.png)
5. 选择一些要使用的 AI 模型。只需从顶部选择几个即可。您以后可以随时更改。

   ![This is an optional caption](/static/quick-start/4_horde_models.png)
6. 关闭 API Connections 窗口。在底部的聊天框中输入消息并按 Enter 键。

   ![This is an optional caption](/static/quick-start/5_msg.png)
7. 您的 AI 将在几秒钟内回复。您可以继续与它[聊天](/Usage/Chatting/index.md)。成功！

   ![This is an optional caption](/static/quick-start/6_success.png)

## 使用 OpenAI 快速开始

### 安装 SillyTavern

按照[安装指南](/Installation/index.md)安装并启动 SillyTavern。

### 获取 OpenAI 访问权限

1. 注册 OpenAI 账户。
2. 前往 <https://platform.openai.com>
3. 点击右上角的账户图标，然后选择 View API Keys。
4. 点击 "Create new secret key"。立即将其复制到某处。**不要分享此密钥。任何拥有此密钥的人都可以使用您的账户，费用由您承担。**

### 配置 SillyTavern 使用您的 API

1. 在 SillyTavern 的顶部栏中，点击 API Connections。
2. 在 API 下，选择 Chat Completion (OpenAI)。
3. 在 Chat Completion Source 下，选择 OpenAI。
4. 粘贴您在上一步中保存的 API 密钥。
5. 点击 Connect 按钮。确认显示 Valid。
6. 默认情况下，SillyTavern 将使用 GPT-4 Turbo。您可以选择不同的模型，但请先了解定价信息。

### 测试您的设置

1. 在 SillyTavern 的顶部栏最右侧，点击 Character Management。
2. 选择一个现有角色，例如 Seraphina。
3. 在底部的文本框中，写一些内容给 Seraphina，然后按 Enter 键或点击 Send 按钮。

如果一切设置正确，几秒钟后，Seraphina 应该会回复。
