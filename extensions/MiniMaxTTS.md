---
order: tts-minimax
route: /extensions/minimaxtts/
---

# MiniMax TTS

本页将教您如何正确使用 MiniMax TTS 提供商。

## 前提条件

1. 具有 API 访问权限的 MiniMax 账户
2. 来自 MiniMax 的有效 API 密钥和组 ID

## 获取 API 凭据

### 1. 创建 MiniMax 账户

1. 访问 [MiniMax 网站（国际版）](https://www.minimax.io/)
2. 点击"Sign Up"或"Login"
3. 完成账户注册流程

!!!warning 地区差异
MiniMax 有独立的中文和国际版本。请注意：
- 中文版不支持声音克隆功能
- 中文版仅支持 `api.minimax.chat` API 主机
!!!

### 2. 获取 API 密钥和组 ID

1. 登录 [MiniMax 控制台（国际版）](https://www.minimax.io/platform/user-center/basic-information)
2. 您可以在基本信息页面上找到您的 GroupId
3. 前往左侧边栏的设置 → API 密钥以创建和获取您的 API 密钥

## 在 SillyTavern 中配置

### 1. 基本设置

1. 打开 SillyTavern
2. 导航到"扩展" → "TTS"
3. 选择"MiniMax"作为您的 TTS 提供商
4. 配置以下设置：
    - **API 密钥**：您的 MiniMax API 密钥
    - **组 ID**：您的 MiniMax 组 ID
    - **API 主机**：根据您的地区选择合适的服务器：
        - `api.minimax.io`（官方国际服务器）
        - `api.minimaxi.chat`（另一个国际服务器主机）
        - `api.minimax.chat`（中国大陆服务器）

### 2. 模型选择

可用模型包括：
- **Speech-02-HD**：高质量语音合成（推荐）
- **Speech-02-Turbo**：快速语音合成
- **Speech-01**：旧版模型
- **Speech-01-240228**：旧版模型（特定版本）

### 3. 语音参数

调整以下参数以自定义语音输出：
- **速度**：0.5 - 2.0（1.0 = 正常速度）
- **音量**：0.1 - 2.0（1.0 = 正常音量）
- **音高**：0.5 - 2.0（1.0 = 正常音高）
- **音频格式**：MP3、WAV、FLAC

## 自定义语音

### 1. 获取语音 ID

1. 访问 [MiniMax TTS 页面（国际版）](https://www.minimax.io/audio/text-to-speech)
2. 点击右侧的"Voice"进入语音选择界面
3. 找到您想要使用的语音
4. 点击语音名称旁边的复制按钮以复制语音 ID

### 2. 添加自定义语音

1. 在 MiniMax TTS 设置中，找到"自定义语音管理"部分
2. 填写以下信息：
    - **语音名称**：选择任何名称以便识别
    - **语音 ID**：从 MiniMax 平台获取的语音 ID
    - **语言**：选择语音的相应语言
3. 点击"添加自定义语音"

## 自定义模型

### 1. 添加自定义模型

1. 在"自定义模型管理"部分
2. 填写：
    - **模型 ID**：模型标识符
    - **模型名称**：模型的显示名称
3. 点击"添加自定义模型"

### 2. 获取模型 ID

1. 查看官方 [MiniMax 文档](https://www.minimax.io/platform/document/Model?key=684261f14c5738213294faa7)中的模型列表
2. 或在控制台中查看可用的自定义模型
3. 复制相应的模型 ID

## 故障排除

### 常见问题

1. **API 身份验证失败**
    - 验证 API 密钥对应正确的 API 主机
    - 确认组 ID 正确
    - 检查您的账户是否有足够的余额

2. **语音生成失败**
    - 验证所选的语音 ID 有效
    - 确保语音与您选择的模型兼容

3. **连接超时**
    - 尝试切换到不同的 API 主机
    - 检查您的网络连接
    - 验证防火墙设置

4. **音频质量问题**
    - 尝试使用不同的模型（Speech-02-HD 以获得最佳质量）
    - 调整语音参数（速度、音高、音量）
    - 检查音频格式兼容性
