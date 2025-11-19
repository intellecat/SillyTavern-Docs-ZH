---
route: /extensions/translation/
templating: false
---

# 聊天翻译

## 概述

Chat Translation Extension 可以使用各种翻译提供商实时翻译不同语言之间的聊天消息。它支持手动和自动翻译模式。

![使用 'Translate Message/翻譯訊息' 消息操作按钮将角色消息从英语翻译成中文](../static/extensions/translation/sensei.png)

+++ English
!["Translate Chat", "Translate Input"](../static/extensions/translation/wand-menu-en.png)
+++ 简体中文
!["翻译聊天", "翻译输入"](../static/extensions/translation/wand-menu-zh-cn.png)
+++ 繁體中文
!["翻譯聊天內容", "翻譯輸入內容"](../static/extensions/translation/wand-menu-zh-tw.png)
+++ 한국어
!["채팅 번역하기", "입력 번역하기"](../static/extensions/translation/wand-menu-ko.png)
+++ Русский
!["Перевести чат", "Перевести моё сообщение"](../static/extensions/translation/wand-menu-ru.png)
+++

## 使用方法

翻译聊天消息的所有方法:

**<i class="fa-solid fa-language"></i> Translate Chat** 按钮,位于 **<i class="fa-solid fa-magic-wand-sparkles"></i>
Extensions** 菜单

- 一次性翻译整个聊天历史

**<i class="fa-solid fa-keyboard"></i> Translate Input** 按钮,位于 **<i class="fa-solid fa-magic-wand-sparkles"></i>
Extensions** 菜单

- 仅翻译当前输入文本
- 在发送消息前很有用

**<i class="fa-solid fa-language"></i> Translate Message** 图标,位于任何消息的 **<i class="fa-solid fa-ellipsis"></i> Message
Actions** 工具栏

- 点击以仅翻译该消息
- 再次点击以恢复原始文本

**Auto-mode** 配置,位于 **<i class="fa-solid fa-cubes"></i>
Extensions** 面板的 **Chat Translation** 抽屉

- 自动翻译用户输入、AI 回复或两者

**/translate** slash command

- 使用 `/translate [target=language_code] text` 翻译文本

## 配置

配置选项位于 **<i class="fa-solid fa-cubes"></i>
Extensions** 面板的 **Chat Translation** 抽屉。

#### Provider

- 选择您首选的[翻译服务](#translation-providers)
- 如果出现 **<i class="fa-solid fa-key"></i> API Key** 图标,请点击以输入 API key
- 如果出现 **<i class="fa-solid fa-link"></i> Custom URL** 图标,请点击以输入自定义 API URL

#### Target Language

选择您想要用来编写消息或阅读 AI 回复的语言。

#### Auto-mode

配置自动翻译行为。

- **None**: 不自动翻译
- **Translate responses**: 自动将 AI 回复翻译成目标语言
- **Translate inputs**: 自动将用户输入翻译成英语
- **Translate both**: 同时翻译用户输入和 AI 回复

#### Clear Translations

**<i class="fa-solid fa-trash-can"></i> Clear Translations** 按钮从当前聊天的消息中删除所有翻译。原始消息被保留。

### 配置示例:中文到英文聊天

要设置一个工作流,让说中文的用户可以用中文与以英语运行的 AI 聊天:

1. 将 Auto-mode 设置为 "Translate both"
2. 将 Target Language 设置为 "Chinese (Simplified)" 或 "Chinese (Traditional)"
3. 选择具有良好语言自动检测功能的翻译提供商(例如 Google 或 DeepL)

此设置将:

- 将用户的中文输入翻译成英文给 AI
- 将 AI 的英文回复翻译回中文给用户

此设置依赖于输入的自动语言检测。为了更精确的控制,未来的更新可能会包含明确的源语言选择。

## 翻译提供商

**:icon-cloud:** 基于云
**<i class="fa-solid fa-link"></i>** 本地,自定义 URL
**<i class="fa-solid fa-key"></i>** 需要 API key

| Provider                                                            | 位置                                                                               | 功能                                                        |
|---------------------------------------------------------------------|-----------------------------------------------------------------------------------|-------------------------------------------------------------|
| [Libre Translate](https://libretranslate.com/)                      | :icon-cloud: <i class="fa-solid fa-key"></i> <i class="fa-solid fa-link"></i>    | 专有翻译服务的自托管(AGPL-3.0)替代方案,具有云托管 Pro 层             |
| [Google Translate](https://cloud.google.com/translate)              | :icon-cloud:                                                                      | 广泛使用,支持多种语言,准确性好                                   |
| [Lingva Translate](https://lingva.ml/)                              | <i class="fa-solid fa-link"></i>                                                  | Google Translate 的替代前端,开源(AGPL-3.0),注重隐私              |
| [DeepL](https://www.deepl.com/)                                     | :icon-cloud: <i class="fa-solid fa-key"></i>                                      | 高质量翻译,特别是欧洲语言                                       |
| [DeepLX](https://github.com/OwO-Network/DeepLX)                     | <i class="fa-solid fa-link"></i>                                                  | 自托管 DeepL 代理,开源(MIT),免费但代理 DeepL Pro 需要 DeepL API key |
| [Bing Translator](https://www.bing.com/translator)                  | :icon-cloud:                                                                      | Microsoft 的翻译服务,与 Azure 服务集成                          |
| [OneRing Translator](https://github.com/janvarev/OneRingTranslator) | <i class="fa-solid fa-link"></i>                                                  | Google Translate 和其他提供商的自托管前端,注重隐私,开源(AGPL-3.0)   |
| [Yandex Translate](https://translate.yandex.com/)                   | :icon-cloud:                                                                      | 适合俄语和东欧语言                                            |

### DeepL 特定配置

- 可用于德语、法语、意大利语、西班牙语、荷兰语、日语和俄语的正式程度级别
- 通过 [config.yaml](/Administration/config-yaml.md#deepl-configuration) 中的 `deepl.formality` 配置

## Slash Commands

使用 `/translate` 命令进行快速翻译。语法:`/translate [target=language_code] text`。如果未提供目标语言,将使用扩展设置中的值。

### 基本用法

将文本翻译成当前目标语言并在弹出窗口中显示:

```
/translate Welcome to the Tavern | /echo
```

![中文(简体)弹出窗口,'欢迎来到酒馆/Welcome to the Tavern'](../static/extensions/translation/welcome-tavern.png)

将文本翻译成西班牙语并添加到聊天中:

```
/translate target=es Hello world | /send
```

![西班牙语用户消息,'Hola Mundo/Hello world'](/static/extensions/translation/hola-mundo.png)

### 测试、管道翻译、本地化

提示用户输入消息和语言,将消息翻译成该语言,然后将其重新翻译成配置的目标语言,并在弹出窗口中显示两种翻译。此示例使用 `/input` 和 `/buttons` 命令来收集用户输入:

```shell
/input default="Hello, world!" <span data-i18n="Test Message">Sample text</span> |
/let key=input ||
/buttons labels=["zh-CN", "zh-TW", "es", "hu", "en"] <span data-i18n="UI Language">Language</span> |
/let key=lang ||
/translate target={{var::lang}} {{var::input}} | /let key=tx_target |
/translate | /let key=tx_orig ||
/echo escapeHtml=false cssClass=wider_dialogue_popup
<b data-i18n="Test Message">Test message</b>: {{var::input}} <br/>
<b data-i18n="Output">Output</b> ({{var::lang}}): {{var::tx_target}} <br/>
<b data-i18n="Output">Output</b> (<span data-i18n="ext_translate_target_lang">target language</span>): {{var::tx_orig}} <br/>
```

这对于在将翻译写入重要位置之前检查您不会说的语言的翻译质量很有用。

![弹出窗口,'Welcome to the Tavern/欢迎来到酒馆/welcome to the pub', en, zh-CN, en](../static/extensions/translation/welcome-tavern-en-cn.png)
![弹出窗口,'My hovercraft is full of eels/我的氣墊船裡裝滿了鰻魚/My hovercraft is filled with eels', en, zh-TW, en](../static/extensions/translation/eels-out-zh-tw.png)

UI 控件以当前语言环境显示,与配置的目标语言无关。

| `/input`                                                                                        | `/buttons`                                                                          |
|-------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------|
| ![输入对话框,'发送测试消息/Send Test Message'](../static/extensions/translation/eels-input-zh.png) | ![按钮对话框,'语言/Language'](../static/extensions/translation/eels-lang-zh.png) |

![弹出窗口,'我的氣墊船裡裝滿了鰻魚/My hovercraft is full of eels', zh-TW -> en -> zh-TW](../static/extensions/translation/eels-out-tw-en.png)

输入语言检测在以下示例中相对有效:

![弹出窗口,'(My hovercraft is full of eels)/A légpárnás hajóm tele van angolnával/我的氣墊船裡裝滿了鰻魚', zh-TW -> hu -> zh-TW](../static/extensions/translation/eels-out-tw-hu.png)
![弹出窗口,'我的氣墊船裡裝滿了鰻魚/Mi aerodeslizador está lleno de anguilas/My hovercraft is full of eels', zh-TW -> es -> en](../static/extensions/translation/eels-out-tw-es-en.png)
![弹出窗口,'Il mio hovercraft è pieno di anguille/我的气垫船里装满了鳗鱼/My hovercraft is filled with eels', it -> zh-CN -> en](../static/extensions/translation/eels-out-it-zhCN-en.png)

## 技术说明

- 支持 UTF-8 编码、特殊字符和表情符号
- 在需要时通过拆分为块来处理大型消息
- 保留消息中的格式和嵌入图像
- 缓存翻译以避免冗余 API 调用

### AI 输入语言

`internal_language` 控制用户消息在发送到 AI 之前自动翻译成的语言。在默认设置中,它硬编码为 'en',无法通过 UI 更改。因此,*发送到 AI* 的消息的翻译目标语言始终是英语。先前的测试表明,AI 在接收英语消息时性能更好,但随着更多 LLM 在更多样化的语言数据上进行训练,这种情况可能会发生变化。我想可以在 `settings.json` 中更改 `internal_language` 并找出答案。

### 中文变体处理

该扩展支持简体和繁体中文,但并非所有翻译提供商都支持。UI 将这些分别显示为 'Chinese (Simplified)' 和 'Chinese (Traditional)',语言代码为 'zh-CN' 和 'zh-TW'。它们被映射到翻译提供商的以下语言代码:

* Libre Translate:'zh-CN' 映射到 'zh','zh-TW' 映射到 'zt'。
* DeepL 和 DeepLX:两种变体都映射到 'ZH'。
* Bing:'zh-CN' 映射到 'zh-Hans','zh-TW' 保持原样。
* 其他提供商按提供的方式使用 'zh-CN' 和 'zh-TW'。

### 文本长度限制

某些提供商对每个请求有字符限制:

- Yandex:5000 个字符
- DeepLX:1500 个字符
- Bing:1000 个字符
- Google:5000 个字符

较长的文本会自动拆分为块进行翻译。
