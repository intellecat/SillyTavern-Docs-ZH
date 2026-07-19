---
route: /usage/api-connections/google/
---

# Google Gemini

Gemini 是 Google 最先进的多模态大语言模型，可通过多个 API 访问，包括 Google Vertex AI 和 Google AI Studio（前身为 MakerSuite）。本指南将帮助你在 SillyTavern 中配置 Gemini API 连接。

## Google AI Studio

AI Studio 是体验最新 Google AI 模型最快捷、最易上手的方式，无需设置 Google Cloud Platform（GCP）项目。它提供简单的 API 密钥，可直接用于访问 Gemini 模型。

### 第一步：创建 Google AI Studio 密钥

1. 前往 [Google AI Studio](https://aistudio.google.com/apikey) 页面，使用 Google 账号登录。
2. 点击「获取 API 密钥」，接受条款与条件。
3. 点击「创建 API 密钥」生成密钥。
4. 将 API 密钥复制到剪贴板。

### 第二步：将 API 密钥填入 SillyTavern

1. 在 SillyTavern 中，进入「API 连接」页面。
2. 选择「聊天补全」作为 API 类型。
3. 在下拉菜单中选择「Google AI Studio」。
4. 将之前复制的 API 密钥粘贴到「API 密钥」文本框中。
5. 点击「连接」按钮保存密钥。

现在即可在 SillyTavern 中使用 Google AI Studio API。

## Google Vertex AI

Vertex AI 是 Google Cloud Platform（GCP）提供的服务，可访问包括 Gemini 系列在内的多种 AI 模型。

Vertex AI API 有多种配置方式，可用模型因方式不同而有所差异。

### 服务账号

Google Cloud Platform（GCP）要求通过服务账号访问 Vertex AI，普通 API 密钥无法使用。系统将从服务账号 JSON 文件生成令牌，再用于向 Vertex AI API 发起认证请求。

创建服务账号的步骤如下：

**前提条件：**

1. 必须拥有 Google Cloud Platform（GCP）账号。
2. 必须在 GCP 账号中创建了项目。
3. 必须为该项目启用了计费功能。

#### 第一步：启用 Vertex AI API

密钥生效前，必须先为项目启用 API。

1. 进入 Google Cloud 控制台：<https://console.cloud.google.com/>
2. 确认顶部栏中选择了正确的项目。
3. 导航至 Vertex AI API 页面：<https://console.cloud.google.com/apis/library/aiplatform.googleapis.com>
4. 若尚未启用，点击「启用」按钮。

#### 第二步：创建服务账号

这是用于访问 Vertex AI API 的身份凭证。

1. 在 Google Cloud 控制台中导航至「服务账号」页面，可在顶部搜索栏搜索，也可直接使用此链接：<https://console.cloud.google.com/iam-admin/serviceaccounts>
2. 选择你的 GCP 项目，点击「+ 创建服务账号」。
3. 填写服务账号名称，例如 `my-vertex-ai-client`。
4. 点击「创建并继续」。
5. 授予项目访问权限：在「角色」下拉菜单中搜索并选择「Vertex AI 用户」。该角色拥有运行模型所需的必要权限，且不会授予过多访问权限。
6. 点击「继续」，然后点击「完成」。

#### 第三步：生成 JSON 密钥

这是你需要的「密码」文件，包含敏感信息，请勿分享或上传至任何公开位置。

1. 返回服务账号列表，找到刚刚创建的账号（如 sillytavern-vertex-ai）。
2. 点击该行最右侧的三点菜单（⋮），选择「管理密钥」。
3. 点击「添加密钥」→「创建新密钥」。
4. 确保密钥类型为 JSON。
5. 点击「创建」。

系统会立即将一个 .json 文件下载到你的电脑。请妥善保管，因为此密钥一旦丢失无法找回。

#### 第四步：将 JSON 内容填入 SillyTavern

下载的 JSON 文件包含与 Vertex AI API 进行身份验证所需的全部信息，内容类似如下：

```json
{
    "type": "service_account",
    "project_id": "your-gcp-project-name",
    "private_key_id": "...",
    "private_key": "-----BEGIN PRIVATE KEY-----\n...\n-----END PRIVATE KEY-----\n",
    "client_email": "sillytavern-vertex-ai@your-gcp-project-name.iam.gserviceaccount.com",
    "client_id": "...",
    "auth_uri": "https://accounts.google.com/o/oauth2/auth",
    "token_uri": "https://oauth2.googleapis.com/token",
    "auth_provider_x509_cert_url": "https://www.googleapis.com/oauth2/v1/certs",
    "client_x509_cert_url": "..."
}
```

1. 用简单的文本编辑器（Windows 用记事本、Mac 用 TextEdit 或 VS Code）打开刚下载的 .json 文件。
2. 全选文件中的文本（Ctrl+A 或 Cmd+A）。
3. 复制文本（Ctrl+C 或 Cmd+C）。
4. 在 SillyTavern 中，进入「API 连接」页面，选择「聊天补全」作为 API 类型，再从下拉菜单选择「Google Vertex AI」。将认证方式切换为「服务账号」。
5. 将全部复制的内容粘贴到「服务账号 JSON 内容」文本框中。
6. 点击「验证 JSON」按钮确认复制正确。
7. 最后向下滚动，点击 API 设置页面底部的「连接」。

现在即可在 SillyTavern 中使用 Google Vertex AI API。

### 快速模式

快速模式是在 Google Cloud 上开始使用生成式 AI 的最便捷方式。无需配置服务账号，可直接使用 API 密钥。

详见官方文档：[Vertex AI 快速模式概览](https://cloud.google.com/vertex-ai/generative-ai/docs/start/express-mode/overview)。

#### 第一步：确认账号符合快速模式资格

必须使用此前未曾创建过 Google Cloud 项目的 Google 账号。
若已有 Google Cloud 项目（包括免费试用），可为此专门创建一个新项目。

#### 第二步：激活 Vertex AI 快速模式

1. 访问以下页面：[Vertex AI Studio](https://cloud.google.com/generative-ai-studio)
2. 点击「免费试用」。
3. 接受条款与条件，使用 Google 账号登录。
4. 选择所在国家/地区，点击「同意并免费开始」，等待设置完成。

#### 第三步：创建 API 密钥

1. 确认 Google Cloud 控制台处于快速模式运行状态，页面左上角应有提示横幅。
2. 点击左侧边栏中的「API 密钥」链接。
3. 点击「创建 API 密钥」按钮。
4. 生成新密钥后，将其复制到剪贴板。

#### 第四步：将 API 密钥填入 SillyTavern

1. 在 SillyTavern 中，进入「API 连接」页面。
2. 选择「聊天补全」作为 API 类型。
3. 从下拉菜单选择「Google Vertex AI」。
4. 将认证方式切换为「快速模式（API 密钥）」。
5. 将之前复制的 API 密钥粘贴到「API 密钥」文本框中。
6. 点击「连接」按钮保存密钥。

现在即可在 SillyTavern 中以快速模式使用 Google Vertex AI API。
