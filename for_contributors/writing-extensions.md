# UI 扩展

UI 扩展通过钩入 SillyTavern 的事件和 API 来扩展其功能。扩展运行在浏览器环境中，对 DOM、JavaScript API 和 SillyTavern 上下文拥有几乎不受限制的访问权限。扩展可以修改 UI、调用内部 API 并与聊天数据进行交互。本指南介绍如何创建自己的扩展（需要具备 JavaScript 知识）。

!!!tip 只是想安装扩展？
请前往：[扩展](../extensions/index.md)。
!!!

如需扩展 Node.js 服务器的功能，请参阅 [服务器插件](./Server-Plugins.md) 页面。

**不会写 JavaScript？**

* 可以考虑将 [STscript](./st-script.md) 作为编写完整扩展的简单替代方案。
* 先学习 [MDN 课程](https://developer.mozilla.org/en-US/docs/Learn/JavaScript)，完成后再回来。

## 扩展提交

想将你的扩展贡献到 [官方内容仓库](https://github.com/SillyTavern/SillyTavern-Content)？请联系我们！

为确保所有扩展的安全性和易用性，我们有以下要求：

1. 你的扩展必须是开源的，并拥有自由许可证（参见 [Choose a License](https://choosealicense.com/licenses/)）。如果不确定，AGPLv3 是个不错的选择。
2. 扩展必须与 SillyTavern 的最新发布版本兼容。如果核心功能发生变化，请准备好更新你的扩展。
3. 扩展必须有完善的文档，包含安装说明、使用示例和功能列表的 README 文件。
4. 需要服务器插件才能运行的扩展将不被接受。

## 示例

查看简单 SillyTavern 扩展的实例：

* <https://github.com/city-unit/st-extension-example> - 基础扩展模板，展示了清单文件创建、本地脚本导入、添加设置 UI 面板以及持久化扩展设置的用法。
* <https://github.com/search?q=topic%3Aextension+org%3ASillyTavern&type=Repositories> - GitHub 上所有官方 SillyTavern 扩展的列表。

## 打包

扩展也可以使用打包工具，将自身与其他模块隔离，并使用 NPM 上的任意依赖，包括 Vue、React 等 UI 框架。

* <https://github.com/SillyTavern/Extension-WebpackTemplate> - 使用 TypeScript 和 Webpack（无 React）的扩展模板仓库。
* <https://github.com/SillyTavern/Extension-ReactTemplate> - 使用 React 和 Webpack 的最简扩展模板仓库。

如需在打包产物中使用相对导入，你可能需要创建一个导入包装器。以下是 Webpack 的示例：

```js
/**
 * Import a member from a module by URL, bypassing webpack.
 * @param {string} url URL to import from
 * @param {string} what Name of the member to import
 * @param {any} defaultValue Fallback value
 * @returns {Promise<any>} Imported member
 */
export async function importFromUrl(url, what, defaultValue = null) {
    try {
        const module = await import(/* webpackIgnore: true */ url);
        if (!Object.hasOwn(module, what)) {
            throw new Error(`No ${what} in module`);
        }
        return module[what];
    } catch (error) {
        console.error(`Failed to import ${what} from ${url}: ${error}`);
        return defaultValue;
     }
}

// Import a function from 'script.js' module
const generateRaw = await importFromUrl('/script.js', 'generateRaw');
```

## manifest.json

每个扩展都必须在 `data/<user-handle>/extensions` 目录下拥有一个文件夹，以及一个 `manifest.json` 文件。该文件包含扩展的元数据，以及指向作为扩展入口点的 JS 脚本文件的路径。

可下载的扩展在通过 HTTP 提供服务时，会被挂载到 `/scripts/extensions/third-party` 文件夹，因此应基于该路径使用相对导入。为了便于本地开发，可以考虑将扩展仓库直接放在 `/scripts/extensions/third-party` 文件夹中（即"为所有用户安装"选项）。

```json
{
    "display_name": "The name of the extension",
    "loading_order": 1,
    "requires": [],
    "optional": [],
    "dependencies": [],
    "js": "index.js",
    "css": "style.css",
    "author": "Your name",
    "version": "1.0.0",
    "homePage": "https://github.com/your/extension",
    "auto_update": true,
    "minimum_client_version": "1.0.0",
    "i18n": {
        "de-de": "i18n/de-de.json"
    },
    "hooks": {
        "install": "onInstall",
        "update": "onUpdate",
        "delete": "onDelete",
        "enable": "onEnable",
        "disable": "onDisable",
        "activate": "onActivate"
    }
}
```

### 清单文件字段

* `display_name` 必填。显示在"管理扩展"菜单中。
* `loading_order` 可选。数值越大，加载越晚。
* `js` 必填，为主 JS 文件的引用路径。
* `css` 可选的样式文件引用路径。
* `author` 必填，应包含作者的姓名或联系信息。
* `auto_update` 设置为 `true` 时，当 ST 软件包版本变更时扩展会自动更新。
* `i18n` 可选对象，用于指定支持的区域设置及其对应的 JSON 文件（见下文）。
* `dependencies` 可选字符串数组，指定本扩展所依赖的其他**扩展**。
* `generate_interceptor` 可选字符串，指定在文本生成请求时调用的全局函数名。
* `minimum_client_version` 可选字符串，指定本扩展运行所需的最低 SillyTavern 版本。
* `hooks` 可选对象，指定从 JS 入口点模块导出的[生命周期钩子](#生命周期钩子)函数名。

### 依赖关系

扩展也可以依赖其他 SillyTavern 扩展。如果任何依赖项缺失或被禁用，该扩展将不会加载。

依赖项通过其在 `public/extensions` 目录中显示的**文件夹名称**来指定。

示例：

* 内置扩展：`"vectors"`、`"caption"`
* 第三方扩展：`"third-party/Extension-WebLLM"`、`"third-party/Extension-Mermaid"`

### 已弃用字段

* `requires` 可选字符串数组，指定所需的 **Extras 模块**。如果已连接的 Extras API 未提供所有列出的模块，扩展将不会加载。
* `optional` 可选字符串数组，指定可选的 **Extras 模块**。即使这些模块缺失，扩展仍会加载，扩展应妥善处理其缺失的情况。

要检查当前已连接的 Extras API 提供了哪些模块，可从 `scripts/extensions.js` 导入 `modules` 数组。

## 脚本编写

### 扩展初始化的最佳实践

* 使用 `activate` 钩子进行同步设置，该设置需要在 SillyTavern 的加载阶段、阻塞式加载器处于活动状态时运行。
* 使用 `APP_INITIALIZED` 事件进行在所有扩展和 UI 元素加载完毕后、但加载器仍在阻塞时需要执行的设置。
* 使用 `APP_READY` 事件进行不需要阻塞 SillyTavern 就绪的异步设置。此事件的处理函数会被等待（await），因此应使用定时器或类似机制来延迟处理。

### 使用 getContext

`SillyTavern` 全局对象中的 `getContext()` 函数可让你访问 SillyTavern 上下文，这是一个包含所有主要应用状态对象、实用函数和工具的集合。

```js
const context = SillyTavern.getContext();
context.chat; // 聊天记录 - 可变对象
context.characters; // 角色列表
context.characterId; // 当前角色的索引
context.groups; // 群组列表
context.groupId; // 当前群组的 ID
// 以及更多...
```

完整的可用属性和函数列表可在 [SillyTavern 源代码](https://github.com/SillyTavern/SillyTavern/blob/staging/public/scripts/st-context.js)中找到。

!!!
如果 `getContext` 中缺少你需要的函数/属性，请联系开发者或向我们提交 pull request！
!!!

### 共享库

SillyTavern 前端内部使用的大多数 npm 库都通过 `SillyTavern` 全局对象的 `libs` 属性共享。

* `lodash` - 实用工具库。[文档](https://lodash.com/)。
* `Fuse` - 模糊搜索库。[文档](https://www.fusejs.io/)。
* `DOMPurify` - HTML 净化库。[文档](https://github.com/cure53/DOMPurify)。
* `hljs` - 语法高亮库。[文档](https://highlightjs.org/)。
* `localforage` - 浏览器存储库（IndexedDB/localStorage 抽象层）。[文档](https://localforage.github.io/localForage/)。
* `Handlebars` - 模板库。[文档](https://handlebarsjs.com/)。
* `css` - CSS 解析/序列化工具。[文档](https://github.com/nicolo-ribaudo/css-tools)。
* `Bowser` - 浏览器/平台检测库。[文档](https://github.com/bowser-js/bowser)。
* `DiffMatchPatch` - 文本差异、匹配和补丁库。[文档](https://github.com/google/diff-match-patch)。
* `Readability` / `isProbablyReaderable` - Mozilla 的文章提取库。[文档](https://github.com/mozilla/readability)。
* `SVGInject` - 内联 SVG 注入库。[文档](https://github.com/nicolo-ribaudo/svg-inject)。
* `showdown` - Markdown 转换库。[文档](https://showdownjs.com/)。
* `moment` - 日期/时间操作库。[文档](http://momentjs.com/)。
* `seedrandom` - 种子随机数生成器。[文档](https://github.com/davidbau/seedrandom)。
* `Popper` - 工具提示/弹出框定位引擎。[文档](https://popper.js.org/)。
* `droll` - 掷骰子库。[文档](https://github.com/thebinarypenguin/droll)。
* `morphdom` - 快速 DOM 差异/补丁库。[文档](https://github.com/patrick-steele-iber/morphdom)。
* `slideToggle` - 原生 JS 滑动切换动画。[文档](https://github.com/nicolo-ribaudo/slidetoggle)。
* `chalk` - 终端字符串样式（在浏览器中用途有限）。[文档](https://github.com/chalk/chalk)。
* `yaml` - YAML 解析器和序列化工具。[文档](https://eemeli.org/yaml/)。
* `chevrotain` - 解析器构建工具包。[文档](https://chevrotain.io/)。
* `gzipSync` / `gzip` - 来自 fflate 的快速压缩工具。[文档](https://github.com/101arrowz/fflate)。

完整的导出库列表可在 [SillyTavern 源代码](https://github.com/SillyTavern/SillyTavern/blob/staging/public/lib.js)中找到。

**示例：** 使用 DOMPurify 库。

```js
const { DOMPurify } = SillyTavern.libs;

const sanitizedHtml = DOMPurify.sanitize('<script>"dirty HTML"</script>');
```

### TypeScript 说明

如果你想为 `SillyTavern` 全局对象中的所有方法（包括 `getContext()` 和 `libs`）获得自动补全功能（你大概率会需要），应添加一个 TypeScript `.d.ts` 模块声明文件。根据扩展的位置，该声明应从 SillyTavern 的源代码中导入全局类型。以下示例适用于"所有用户"和"当前用户"两种安装类型。

**global.d.ts** - 将此文件放在扩展的根目录中（与 `manifest.json` 同级）：

```ts
export {};

// 1. 为用户范围扩展的导入
import '../../../../public/global';
// 2. 为服务器范围扩展的导入
import '../../../../global';

// 如需定义其他类型...
declare global {
    // 在此添加全局类型声明
}
```

### HTML 模板

扩展可以使用 Handlebars HTML 模板来构建 UI。将 `.html` 模板文件放在扩展目录中，并使用 `getContext()` 中的 `renderExtensionTemplateAsync()` 函数进行渲染。

该函数接受扩展的文件夹名、模板文件名（不含 `.html`）以及一个可选的 Handlebars 模板变量数据对象。返回的 HTML 会自动通过 DOMPurify 净化，并通过 `data-i18n` 属性进行本地化处理。

```js
const { renderExtensionTemplateAsync } = SillyTavern.getContext();

// 使用给定数据渲染 'third-party/my-extension/settings.html'
const settingsHtml = await renderExtensionTemplateAsync(
    'third-party/my-extension',
    'settings',
    { title: 'My Extension', version: '1.0', defaultValue: 'test' }
);

// 追加到扩展设置面板
$('#extensions_settings2').append(settingsHtml);
```

**模板文件示例**（`settings.html`）：

```html
<div class="my-extension-settings">
    <div class="inline-drawer">
        <div class="inline-drawer-toggle inline-drawer-header">
            <b data-i18n="{{title}}">{{title}}</b>
            <div class="inline-drawer-icon fa-solid fa-circle-chevron-down down"></div>
        </div>
        <div class="inline-drawer-content">
            <label for="my_ext_option">
                <span data-i18n="Option">Option</span>
            </label>
            <input id="my_ext_option" type="text" value="{{defaultValue}}" />
        </div>
    </div>
</div>
```

!!!warning
`renderExtensionTemplate()`（同步版本）已弃用。请始终使用 `renderExtensionTemplateAsync()`。
!!!

### 从其他文件导入

!!!warning
从 SillyTavern 代码中直接导入是不可靠的，如果 ST 内部模块结构发生变化，可能随时失效。`getContext` 提供了更稳定的 API。
!!!

除非你正在构建打包扩展，否则可以从其他 JS 文件导入变量和函数。

例如，以下代码片段将使用当前选定的 API 在后台生成回复：

```js
import { generateQuietPrompt } from "../../../../script.js";

async function handleMessage(data) {
    const text = data.message;
    const translated = await generateQuietPrompt({ quietPrompt: text });
    // ...
}
```

## 状态管理

### 持久化设置

当扩展需要持久保存其状态时，可以使用 `getContext()` 中的 `extensionSettings` 对象来存储和检索数据。扩展可以在设置对象中存储任何 JSON 可序列化的数据，且必须使用唯一键以避免与其他扩展冲突。

要持久化设置，请使用 `saveSettingsDebounced()` 函数，它将把设置保存到服务器。

```js
const { extensionSettings, saveSettingsDebounced } = SillyTavern.getContext();

// 为你的扩展定义一个唯一标识符
const MODULE_NAME = 'my_extension';

// 定义默认设置
const defaultSettings = Object.freeze({
    enabled: false,
    option1: 'default',
    option2: 5
});

// 定义一个获取或初始化设置的函数
function getSettings() {
    // 如果设置不存在则初始化
    if (!extensionSettings[MODULE_NAME]) {
        extensionSettings[MODULE_NAME] = structuredClone(defaultSettings);
    }

    // 确保所有默认键都存在（更新后很有用）
    for (const key of Object.keys(defaultSettings)) {
        if (!Object.hasOwn(extensionSettings[MODULE_NAME], key)) {
            extensionSettings[MODULE_NAME][key] = defaultSettings[key];
        }
    }

    return extensionSettings[MODULE_NAME];
}

// 使用设置
const settings = getSettings();
settings.option1 = 'new value';

// 保存设置
saveSettingsDebounced();
```

### 聊天元数据

要将某些数据绑定到特定聊天，可以使用 `getContext()` 中的 `chatMetadata` 对象。该对象允许你存储与聊天相关联的任意数据，对于存储扩展特定状态非常有用。

要持久化元数据，请使用 `saveMetadata()` 函数，它将把元数据保存到服务器。

!!!warning
不要在长期变量中保存 `chatMetadata` 的引用，因为当切换聊天时该引用会发生变化。请始终使用 `SillyTavern.getContext().chatMetadata` 来访问当前聊天的元数据。
!!!

```js
const { chatMetadata, saveMetadata } = SillyTavern.getContext();

// 为当前聊天设置一些元数据
chatMetadata['my_key'] = 'my_value';

// 获取当前聊天的元数据
const value = chatMetadata['my_key'];

// 将元数据保存到服务器
await saveMetadata();
```

!!!tip
切换聊天时会触发 `CHAT_CHANGED` 事件，你可以监听此事件来相应地更新扩展的状态。更多信息请参见[监听事件](#监听事件)部分。
!!!

### 角色卡

SillyTavern 完整支持 [Character Cards V2 规范](https://github.com/malfoyslastname/character-card-spec-v2/blob/main/spec_v2.md)，该规范允许在角色卡 JSON 数据中存储任意数据。

这对于需要存储与角色相关联的附加数据、并在导出角色卡时使其可共享的扩展非常有用。

要向角色卡的 [extensions](https://github.com/malfoyslastname/character-card-spec-v2/blob/main/spec_v2.md#extensions) 数据字段写入数据，请使用 `getContext()` 中的 `writeExtensionField` 函数。该函数接受角色 ID、字符串键和要写入的值，值必须是 JSON 可序列化的。

!!!warning 注意事项
尽管叫做 `characterId`，它并不是真正意义上的唯一标识符，而是角色在 `characters` 数组中的索引。

当前角色的索引由上下文中的 `characterId` 属性提供。如果要向当前选定的角色写入数据，请使用 `SillyTavern.getContext().characterId`。如果需要为其他角色存储数据，请通过在 `characters` 数组中搜索角色来找到其索引。

**注意：在群组聊天中或未选择角色时，`characterId` 为 `undefined`！**
!!!

```js
const { writeExtensionField, characterId } = SillyTavern.getContext();

// 向角色卡写入一些数据
await writeExtensionField(characterId, 'my_extension_key', {
    someData: 'value',
    anotherData: 42
});

// 从角色卡读回数据
const character = SillyTavern.getContext().characters[characterId];
// 数据存储在角色数据的 `extensions` 对象中
const myData = character.data?.extensions?.my_extension_key;
```

### 设置预设

任意 JSON 数据可以存储在主要 API 类型的设置预设中。它会随预设 JSON 一同导出和导入，因此你可以用它为预设存储扩展特定的设置。以下 API 类型在设置预设中支持数据扩展：

* Chat Completion
* Text Completion
* NovelAI
* KoboldAI / AI Horde

要读取或写入数据，首先需要从上下文中获取 PresetManager 实例：

```js
const { getPresetManager } = SillyTavern.getContext();

// 获取当前 API 类型的预设管理器
const pm = getPresetManager();

// 向预设扩展字段写入数据：
// - path: 预设数据中字段的路径
// - value: 要写入的值
// - name（可选）: 要写入的预设名称，默认为当前选定的预设
await pm.writePresetExtensionField({ path: 'hello', value: 'world' });

// 从预设扩展字段读取数据：
// - path: 预设数据中字段的路径
// - name（可选）: 要读取的预设名称，默认为当前选定的预设
const value = pm.readPresetExtensionField({ path: 'hello' });
```

!!!tip
切换预设或主要 API 时会分别触发 `PRESET_CHANGED` 和 `MAIN_API_CHANGED` 事件，你可以监听这些事件来相应地更新扩展的状态。更多信息请参见[监听事件](#监听事件)部分。
!!!

## 国际化

!!!
有关提供翻译的一般信息，请参阅[国际化](/For_Contributors/i18n.md)页面。
!!!

扩展可以提供额外的本地化字符串，供 HTML 模板中的 `t`、`translate` 函数和 `data-i18n` 属性使用。

支持的区域设置列表（`lang` 键）参见：<https://github.com/SillyTavern/SillyTavern/blob/release/public/locales/lang.json>

### 直接调用 `addLocaleData`

将区域代码和包含翻译内容的对象传递给 `addLocaleData` 函数。**不允许**覆盖已有的键。如果传入的区域代码不是当前选定的区域，数据将被静默忽略。

```js
SillyTavern.getContext().addLocaleData('fr-fr', { 'Hello': 'Bonjour' });
SillyTavern.getContext().addLocaleData('de-de', { 'Hello': 'Hallo' });
```

### 通过扩展清单文件

在清单文件中添加 i18n 对象，列出支持的区域设置及其对应的 JSON 文件路径（相对于扩展目录）。

```json
{
  "display_name": "Foobar",
  "js": "index.js",
  // 其他字段...
  "i18n": {
    "fr-fr": "i18n/french.json",
    "de-de": "i18n/german.json"
  }
}
```

## 注册斜杠命令（新方式）

虽然 `registerSlashCommand` 仍然存在以保持向后兼容性，但新的斜杠命令现在应通过 `SlashCommandParser.addCommandObject()` 来注册，以便向解析器（进而向自动补全和命令帮助）提供关于命令及其参数的详细信息。

```javascript
SlashCommandParser.addCommandObject(SlashCommand.fromProps({ name: 'repeat',
    callback: (namedArgs, unnamedArgs) => {
        return Array(namedArgs.times ?? 5)
            .fill(unnamedArgs.toString())
            .join(isTrueBoolean(namedArgs.space.toString()) ? ' ' : '')
        ;
    },
    aliases: ['example-command'],
    returns: 'the repeated text',
    namedArgumentList: [
        SlashCommandNamedArgument.fromProps({ name: 'times',
            description: 'number of times to repeat the text',
            typeList: ARGUMENT_TYPE.NUMBER,
            defaultValue: '5',
        }),
        SlashCommandNamedArgument.fromProps({ name: 'space',
            description: 'whether to separate the texts with a space',
            typeList: ARGUMENT_TYPE.BOOLEAN,
            defaultValue: 'off',
            enumList: ['on', 'off'],
        }),
    ],
    unnamedArgumentList: [
        SlashCommandArgument.fromProps({ description: 'the text to repeat',
            typeList: ARGUMENT_TYPE.STRING,
            isRequired: true,
        }),
    ],
    helpString: `
        <div>
            Repeats the provided text a number of times.
        </div>
        <div>
            <strong>Example:</strong>
            <ul>
                <li>
                    <pre><code class="language-stscript">/repeat foo</code></pre>
                    returns "foofoofoofoofoo"
                </li>
                <li>
                    <pre><code class="language-stscript">/repeat times=3 space=on bar</code></pre>
                    returns "bar bar bar"
                </li>
            </ul>
        </div>
    `,
}));
```

所有注册的命令都可以在 [STscript](/For_Contributors/st-script.md) 中以任何可能的方式使用。

## 事件

### 监听事件

使用 `eventSource.on(eventType, eventHandler)` 来监听事件：

```js
const { eventSource, event_types } = SillyTavern.getContext();

eventSource.on(event_types.MESSAGE_RECEIVED, handleIncomingMessage);

function handleIncomingMessage(data) {
    // 处理消息
}
```

主要事件类型如下：

**应用生命周期：**

* `APP_INITIALIZED`：应用已初始化并接近就绪状态，但加载器仍可见。可以在此处进行 UI 修改。每次在应用初始化后附加新监听器时，此事件会自动触发。
* `APP_READY`：应用已完全加载并准备好使用。每次在应用就绪后附加新监听器时，此事件会自动触发。

**消息：**

* `MESSAGE_SENT`：用户发送的消息已记录到 `chat` 对象中，但尚未在 UI 中渲染。
* `MESSAGE_RECEIVED`：LLM 生成的消息已记录到 `chat` 对象中，但尚未在 UI 中渲染。
* `USER_MESSAGE_RENDERED`：用户发送的消息已在 UI 中渲染。
* `CHARACTER_MESSAGE_RENDERED`：LLM 生成的消息已在 UI 中渲染。
* `MESSAGE_EDITED`：用户编辑了一条消息。
* `MESSAGE_DELETED`：一条消息被删除。
* `MESSAGE_SWIPED`：触发了消息滑动操作。
* `STREAM_TOKEN_RECEIVED`：流式生成过程中收到了新的 token。

**生成：**

* `GENERATION_AFTER_COMMANDS`：处理斜杠命令后，生成即将开始。
* `GENERATION_STARTED`：生成已开始。
* `GENERATION_STOPPED`：生成被用户停止。
* `GENERATION_ENDED`：生成已完成或发生错误。

**聊天：**

* `CHAT_CHANGED`：聊天已切换（例如，切换到另一个角色，或加载了另一个聊天）。
* `CHAT_CREATED`：新聊天已创建。
* `CHAT_DELETED`：聊天已删除。

**角色：**

* `CHARACTER_EDITED`：角色数据已更改。
* `CHARACTER_DELETED`：角色已删除。
* `CHARACTER_DUPLICATED`：角色已复制。

**人设：**

* `PERSONA_CHANGED`：当前活动人设已更改。
* `PERSONA_CREATED`：新人设已创建。
* `PERSONA_UPDATED`：人设已更新。
* `PERSONA_RENAMED`：人设已重命名。
* `PERSONA_DELETED`：人设已删除。

**设置与预设：**

* `SETTINGS_UPDATED`：应用设置已更新。
* `PRESET_CHANGED`：当前活动预设已更改。
* `MAIN_API_CHANGED`：主要 API 类型已切换。
* `CHATCOMPLETION_SOURCE_CHANGED`：对话补全来源已更改。
* `CHATCOMPLETION_MODEL_CHANGED`：对话补全模型已更改。
* `CONNECTION_PROFILE_LOADED`：连接配置文件已加载。

**世界信息：**

* `WORLDINFO_UPDATED`：世界信息数据已更新。
* `WORLDINFO_SETTINGS_UPDATED`：世界信息设置已更改。

**工具调用：**

* `TOOL_CALLS_PERFORMED`：工具调用已执行。
* `TOOL_CALLS_RENDERED`：工具调用结果已在聊天中渲染。

**文本转语音：**

* `TTS_JOB_STARTED`：TTS 任务已开始。
* `TTS_AUDIO_READY`：TTS 音频数据已准备好播放。
* `TTS_JOB_COMPLETE`：TTS 任务已完成。

完整的事件类型列表可在[源代码](https://github.com/SillyTavern/SillyTavern/blob/staging/public/scripts/events.js)中找到。

!!!info 事件数据
每个事件向监听器传递数据的方式并不统一。有些事件不发送任何数据，有些则传递对象或原始值。请参考事件触发处的源代码以了解其传递的数据，或使用调试器进行检查。
!!!

### 触发事件

你可以从扩展中触发任何应用事件，包括自定义事件，只需调用 `eventSource.emit(eventType, ...eventData)`：

```js
const { eventSource } = SillyTavern.getContext();

// 可以是内置的 event_types 字段，也可以是任意字符串。
const eventType = 'myCustomEvent';

// 使用 `await` 确保所有事件处理程序在继续执行前完成。
await eventSource.emit(eventType, { data: 'custom event data' });
```

## 提示词拦截器

提示词拦截器为扩展提供了一种在发出文本生成请求之前执行各种操作的方式，例如修改聊天数据、添加注入内容或中止生成。

来自不同扩展的拦截器按顺序依次运行。执行顺序由各自 `manifest.json` 文件中的 `loading_order` 字段决定。`loading_order` 值越低，运行越早。若未指定 `loading_order`，则使用 `display_name` 作为备用。若两者都未指定，则顺序不确定。

### 注册拦截器

要定义提示词拦截器，请在扩展的 `manifest.json` 文件中添加 `generate_interceptor` 字段。其值应为一个全局函数的名称，SillyTavern 将调用该函数。

```json
{
    "display_name": "My Interceptor Extension",
    "loading_order": 10, // 影响执行顺序
    "generate_interceptor": "myCustomInterceptorFunction",
    // ... 其他清单属性
}
```

### 拦截器函数

`generate_interceptor` 函数是一个全局函数，将在非干跑（dry run）的生成请求时被调用。它必须定义在全局作用域中（例如，`globalThis.myCustomInterceptorFunction = async function(...) { ... }`），并且如果需要执行任何异步操作，可以返回一个 `Promise`。

拦截器函数接收以下参数：

* `chat`：一个消息对象数组，代表将用于提示词构建的聊天历史。你可以直接修改此数组（例如，添加、删除或修改消息）。请注意，消息是可变对象，因此你对数组所做的任何修改都会反映到实际的聊天历史中。如果你希望修改是临时的，请使用 `structuredClone` 创建消息对象的深度副本。
* `contextSize`：一个数字，表示为即将到来的生成计算的当前上下文大小（以 token 为单位）。
* `abort`：一个函数，调用时将发出信号以阻止文本生成继续进行。它接受一个布尔参数，如果为 `true`，则阻止任何后续拦截器运行。
* `type`：一个字符串，表示生成的类型或触发方式（例如，`'quiet'`、`'regenerate'`、`'impersonate'`、`'swipe'` 等）。这帮助拦截器根据生成的发起方式有条件地应用逻辑。

**实现示例：**

```javascript
globalThis.myCustomInterceptorFunction = async function(chat, contextSize, abort, type) {
    // 示例：在最后一条用户消息之前添加一条系统说明
    const systemNote = {
        is_user: false,
        name: "System Note",
        send_date: Date.now(),
        mes: "This was added by my extension!"
    };
    // 在最后一条消息之前插入
    chat.splice(chat.length - 1, 0, systemNote);
}
```

## 生命周期钩子

扩展可以在 `manifest.json` 中定义生命周期钩子，这些钩子在扩展生命周期的特定时间点被调用。每个钩子映射到扩展 JS 入口点模块（`js` 字段指定的文件）中**导出的函数**。

所有钩子都是可选的。钩子函数可以返回一个 `Promise`，该 `Promise` 将被等待（有 5 秒超时）。如果钩子超时，将记录警告并继续执行。钩子中的错误会被捕获并记录，不会阻塞操作。

### 可用钩子

| 钩子 | 调用时机 |
|------|---------|
| `activate` | 页面加载期间扩展成功激活时 |
| `install` | 扩展安装完成且设置加载后 |
| `update` | 扩展成功更新后（在重载通知提示之前） |
| `delete` | 扩展从服务器删除之前 |
| `enable` | 扩展启用且设置保存之前 |
| `disable` | 扩展禁用且设置保存之前 |
| `clean` | 用户在扩展管理器中点击"清理扩展数据"按钮时，或在删除扩展时选择清理选项时 |

### 清单文件配置

在 `manifest.json` 中添加 `hooks` 对象，将钩子名称映射到导出的函数名称：

```json
{
    "display_name": "My Extension",
    "js": "index.js",
    // 其他字段...
    "hooks": {
        "install": "onInstall",
        "update": "onUpdate",
        "delete": "onDelete",
        "enable": "onEnable",
        "disable": "onDisable",
        "activate": "onActivate",
        "clean": "onClean"
    }
}
```

名称可以自由选择，只要是有效的 JS 函数名即可。
可以配置任意数量的钩子，不必填写并实现所有钩子。

### 实现

从主 JS 入口点导出钩子函数。每个函数不接受参数，可以选择性地返回 `Promise`：

```js
// index.js - 你的扩展入口点

export async function onInstall() {
    console.log('Extension installed! Performing first-time setup...');
    // 例如：初始化默认数据、创建存储条目
}

export async function onActivate() {
    console.log('Extension activated during page load');
}

export async function onUpdate() {
    console.log('Extension updated! Running migrations...');
    // 例如：将数据从旧格式迁移到新格式
}

export async function onDelete() {
    console.log('Extension about to be deleted. Cleaning up...');
    // 例如：删除存储的数据、清理 localStorage
    const { localforage } = SillyTavern.libs;
    await localforage.removeItem('my_extension_data');
}

export function onEnable() {
    console.log('Extension enabled');
}

export function onDisable() {
    console.log('Extension disabled');
}

export async function onClean() {
    console.log('Extension data cleaned');
    // 例如：在此处清理扩展数据
}
```

!!!warning
钩子函数有 **5 秒超时**限制。如果钩子执行时间超过此限制，将继续执行并记录警告。请保持钩子逻辑快速轻量。
!!!

## 生成文本

SillyTavern 提供了几个函数，可以使用当前选定的 LLM API 在不同上下文中生成文本。这些函数允许你在聊天上下文中生成文本、进行不带任何上下文的原始生成，或生成结构化输出。

### 在聊天上下文中

`generateQuietPrompt()` 函数用于在聊天上下文中，以添加"静默"提示词（后置历史指令）的方式在后台生成文本（输出不会在 UI 中渲染）。这对于在不打扰用户体验的同时保留相关聊天和角色数据（例如生成摘要或图像提示词）非常有用。

```js
const { generateQuietPrompt } = SillyTavern.getContext();

const quietPrompt = 'Generate a summary of the chat history.';

const result = await generateQuietPrompt({
    quietPrompt,
});
```

### 原始生成

`generateRaw()` 函数用于不带任何聊天上下文地生成文本。当你想要完全控制提示词构建过程时，它非常有用。

它接受一个 `prompt`，作为 Text Completion 字符串或 Chat Completion 对象数组，并根据所选 API 类型（例如在聊天/文本模式之间转换、应用指令格式等）以适当的格式构建请求。你还可以向函数传递额外的 `systemPrompt` 和 `prefill`，以更精细地控制生成过程。

```js
const { generateRaw } = SillyTavern.getContext();

const systemPrompt = 'You are a helpful assistant.';
const prompt = 'Generate a story about a brave knight.';
const prefill = 'Once upon a time,';

/*
在 Chat Completion 模式下，将生成如下提示词：
[
  {role: 'system', content: 'You are a helpful assistant.'},
  {role: 'user', content: 'Generate a story about a brave knight.'},
  {role: 'assistant', content: 'Once upon a time,'}
]
*/

/*
在 Text Completion 模式下（无指令格式），将生成如下提示词：
"You are a helpful assistant.\nGenerate a story about a brave knight.\nOnce upon a time,"
*/

const result = await generateRaw({
    systemPrompt,
    prompt,
    prefill,
});
```

### 结构化输出

!!!info
目前仅支持 Chat Completion API。可用性取决于所选的来源和模型。如果所选模型不支持结构化输出，生成将失败或返回空对象（`'{}'`）。请查阅你所使用的具体 API 的文档，以了解是否支持结构化输出。
!!!

你可以使用结构化输出功能，确保模型生成符合所提供 [JSON Schema](https://json-schema.org/learn) 的有效 JSON 对象。这对于需要结构化数据的扩展（如状态追踪、数据分类等）非常有用。

要使用结构化输出，必须向 `generateRaw()` 或 `generateQuietPrompt()` 传递一个 JSON schema 对象。模型将生成符合该 schema 的响应，并以序列化 JSON 对象字符串的形式返回。

!!!warning
输出不会根据 schema 进行验证，你必须自己处理生成输出的解析和验证。如果模型未能生成有效的 JSON 对象，函数将返回一个空对象（`'{}'`）。

[Zod](https://zod.dev/json-schema) 是一个用于生成和验证 JSON schema 的常用库，此处不作介绍。
!!!

```js
const { generateRaw, generateQuietPrompt } = SillyTavern.getContext();

// 为预期输出定义 JSON schema
const jsonSchema = {
    // 必填：schema 的名称
    name: 'StoryStateModel',
    // 可选：schema 的描述
    description: 'A schema for a story state with location, plans, and memories.',
    // 可选：schema 将以严格模式使用，意味着只允许 schema 中定义的字段
    strict: true,
    // 必填：schema 的定义
    value: {
        '$schema': 'http://json-schema.org/draft-04/schema#',
        'type': 'object',
        'properties': {
            'location': {
                'type': 'string'
            },
            'plans': {
                'type': 'string'
            },
            'memories': {
                'type': 'string'
            }
        },
        'required': [
            'location',
            'plans',
            'memories'
        ],
    },
};

const prompt = 'Generate a story state with location, plans, and memories. Output as a JSON object.';

const rawResult = await generateRaw({
    prompt,
    jsonSchema,
});

const quietResult = await generateQuietPrompt({
    quietPrompt: prompt,
    jsonSchema,
});
```

## 注册自定义宏

你可以注册自定义宏，在任何支持宏替换的地方使用，例如角色卡字段、STscript 命令、提示词模板等。

### 新宏系统

注册宏的推荐方式是通过 `SillyTavern.getContext()` 提供的 `macros.register()` 函数。该系统支持参数、分类、描述和丰富的文档元数据。

```js
const { macros } = SillyTavern.getContext();

// 带处理函数的简单宏
macros.register('tomorrow', {
    description: 'Returns tomorrow\'s date',
    handler: () => {
        return new Date(Date.now() + 24 * 60 * 60 * 1000).toLocaleDateString();
    },
});

// 带无名参数和分类的宏
macros.register('greet', {
    description: 'Generates a greeting for the given name',
    category: macros.category.UTILITY,
    unnamedArgs: [
        { name: 'name', description: 'The name to greet' },
    ],
    handler: ({ unnamedArgs }) => {
        const [name] = unnamedArgs;
        return `Hello, ${name}!`;
    },
});
```

`handler` 函数接收一个 [MacroExecutionContext](https://github.com/SillyTavern/SillyTavern/blob/staging/public/scripts/macros/engine/MacroRegistry.js) 对象，包含：

* `args` - 传递给宏的所有无名参数。
* `unnamedArgs` - 与已定义参数列表匹配的位置参数。
* `list` - 列表参数（在无名参数之后），如果未启用列表则为 `null`。
* `env` - 宏环境，可访问角色数据、聊天状态等。
* `resolve(text)` - 用于解析文本中嵌套宏的函数（当 `delayArgResolution` 为 `true` 时）。

以及更多。

处理函数同步运行，因此不能返回 `Promise` 或同步调用异步操作。

取消注册宏：

```js
const { macros } = SillyTavern.getContext();

macros.registry.unregisterMacro('greet');
```

你还可以为已有宏注册别名：

```js
const { macros } = SillyTavern.getContext();

macros.registerAlias('greet', 'hello', { visible: true });
```

### 旧版宏系统（已弃用）

!!!warning
`getContext()` 中的 `registerMacro()` 和 `unregisterMacro()` 已**弃用**。请改用 `macros.register()` 和 `macros.registry.unregisterMacro()`。
!!!

旧版 API 仍可用以保持向后兼容性，但将在未来版本中移除：

```js
const { registerMacro, unregisterMacro } = SillyTavern.getContext();

// 简单字符串宏
registerMacro('fizz', 'buzz');
// 函数宏（必须是同步的）
registerMacro('tomorrow', () => {
    return new Date(Date.now() + 24 * 60 * 60 * 1000).toLocaleDateString();
});

// 取消注册
unregisterMacro('fizz');
```

## 消息格式化钩子

!!!warning Staging 功能
此功能目前仅在 SillyTavern 的 `staging` 分支上可用，尚未包含在最新版本中。
!!!

扩展可以钩入消息格式化流水线，在消息文本到达 DOM 之前对其进行转换。这对于添加注解（ruby 标签、工具提示）、高亮显示或自定义文本转换非常有用。

!!!warning
钩子同步运行且**必须返回字符串**。异步函数和非字符串返回值会在注册时抛出 `TypeError`，或在运行时被静默忽略并输出控制台警告。不要在这些钩子中执行耗时操作——它们在每次消息渲染时都会运行。
!!!

### 流水线阶段

钩子可以注册到三个流水线阶段。所有阶段都在 DOMPurify 净化**之前**运行，因此输出始终是安全的：

| 阶段 | 运行时机 | 文本格式 |
|------|---------|---------|
| `beforeRegex` | 剥离提示词偏置后、自定义正则规则前 | 纯文本 |
| `afterRegex` | 自定义正则规则后、Markdown 转换前 | 纯文本 |
| `afterMarkdown` | Markdown 转 HTML（showdown）后、DOMPurify 前 | HTML 字符串 |

`afterMarkdown` 阶段是默认选项，也是扩展注解渲染 HTML 时最常用的插入点。

### 注册钩子

从 `getContext()` 中访问 `messageFormatter`：

```js
const { messageFormatter } = SillyTavern.getContext();

// 简单钩子 - 转换消息文本
messageFormatter.addHook((mes, ctx) => {
    // 跳过用户消息
    if (ctx.isUser) return mes;

    // 为日文文本添加振假名
    return addFurigana(mes);
});

// 带有明确阶段和顺序的钩子
messageFormatter.addHook((mes, ctx) => {
    // 在 Markdown 转换后、净化前进行转换
    return mes.replace(/\*\*(.+?)\*\*/g, '<mark>$1</mark>');
}, {
    stage: messageFormatter.stage.AFTER_MARKDOWN,
    order: messageFormatter.order.EARLY,
});
```

### 钩子上下文

钩子接收一个不可变的上下文对象，包含消息元数据：

| 属性 | 类型 | 描述 |
|------|------|------|
| `characterName` | `string` | 与消息关联的角色名称 |
| `isSystem` | `boolean` | 消息是否为系统消息 |
| `isUser` | `boolean` | 消息是否由用户发送 |
| `messageId` | `number` | 消息在聊天数组中的索引，瞬态消息（流式预览）为 `-1` |
| `isReasoning` | `boolean` | 消息是否为推理/思考输出 |
| `stage` | `string` | 当前正在执行的流水线阶段 |

上下文对象通过 `Object.freeze()` 冻结——尝试修改它将不会生效。

### 钩子排序

同一阶段内的钩子按升序运行。使用 `order` 选项控制执行顺序：

```js
const { hook_order } = messageFormatter;

// 预定义常量
hook_order.EARLIEST;  // 0
hook_order.EARLY;     // 10
hook_order.NORMAL;    // 50（默认值）
hook_order.LATE;      // 90
hook_order.LATEST;    // 100

// 自定义数值
messageFormatter.addHook(myHook, { order: 25 });
```

数值越小，越先运行。当多个扩展转换同一文本时，这非常有用——例如，一个扩展可能先提取数据，另一个扩展再对其进行格式化。

### 错误处理

钩子执行包裹在 try/catch 中。如果钩子抛出异常，该钩子会被跳过并记录控制台错误——流水线继续执行剩余的钩子。

如果钩子返回非字符串值（包括 `undefined` 或 `Promise`），将发出控制台警告并忽略返回值。流水线继续使用之前的文本继续执行。

### 完整流水线顺序

以下是完整的消息格式化流水线顺序供参考：

1. 提示词偏置剥离（仅消息 0）
2. 注释/隐藏消息规范化
3. `beforeRegex` 扩展钩子
4. 自定义正则规则（`getRegexedString`）
5. `afterRegex` 扩展钩子
6. Markdown 自动修复（`fixMarkdown`）
7. HTML 标签编码（`encode_tags`）
8. Showdown Markdown → HTML 转换
9. `afterMarkdown` 扩展钩子
10. 名称前缀剥离（`allow_name2_display`）
11. DOMPurify 净化

所有扩展钩子（步骤 3、5、9）均在 DOMPurify **之前**运行，因此其输出始终会被净化。

## 函数工具调用

扩展可以注册自定义函数工具，LLM 可以在聊天补全期间调用这些工具。这让你的扩展能够对来自模型的结构化数据作出响应——例如，查询 API、执行计算或触发扩展功能。

完整指南（包括前提条件、支持的 API、注册字段和技巧）请参阅专门的[函数调用](./Function-Calling.md)页面。

**快速示例：**

```js
const { registerFunctionTool } = SillyTavern.getContext();

registerFunctionTool({
    name: 'get_weather',
    displayName: 'Get Weather',
    description: 'Get the current weather for a given location',
    parameters: {
        $schema: 'http://json-schema.org/draft-04/schema#',
        type: 'object',
        properties: {
            location: { type: 'string', description: 'City name' },
        },
        required: ['location'],
    },
    action: async ({ location }) => {
        const data = await fetchWeatherData(location);
        return JSON.stringify(data);
    },
});
```

## 操作加载器

操作加载器为长时间运行的操作提供加载覆盖层和通知提示系统。它替代了已弃用的 `showLoader()` / `hideLoader()` 函数。

通过 `getContext()` 中的 `loader` 访问它：

```js
const { loader } = SillyTavern.getContext();

// 基本阻塞加载器，带有可停止的通知提示
const handle = loader.show({ message: 'Processing data...' });
try {
    const result = await someExpensiveOperation();
} finally {
    await handle.hide();
}
```

### 选项

| 选项 | 默认值 | 描述 |
|------|--------|------|
| `blocking` | `true` | 显示阻止交互的全屏覆盖层 |
| `message` | `'Generating...'` | 通知提示中显示的消息 |
| `title` | `''` | 通知提示的可选标题 |
| `toastMode` | `'stoppable'` | `'stoppable'`（带停止按钮）、`'static'`（无按钮）或 `'none'`（无通知提示） |
| `stopTooltip` | `'Stop'` | 停止按钮的工具提示文本 |
| `onStop` | `null` | 自定义停止处理函数，默认为 `stopGeneration()` |
| `onHide` | `null` | 加载器隐藏时（非停止时）调用 |
| `overlayContent` | `null` | 替换默认旋转器的自定义 HTML 元素或字符串 |

### 叠加加载器

多个加载器可以同时激活。只要至少有一个阻塞加载器处于活动状态，覆盖层就会保持可见：

```js
const { loader } = SillyTavern.getContext();

const loader1 = loader.show({ message: 'Task 1...' });
const loader2 = loader.show({ message: 'Task 2...' });
await loader1.hide(); // 覆盖层保持——loader2 仍在活动
await loader2.hide(); // 现在覆盖层隐藏
```

### 非阻塞加载器

对于不应阻塞 UI 的后台任务：

```js
const { loader } = SillyTavern.getContext();

const handle = loader.show({
    blocking: false,
    message: 'Downloading in background...',
    onStop: () => abortDownload(),
});
```

## 弹出窗口与用户反馈

### 弹出窗口辅助函数

SillyTavern 通过 `getContext()` 中的 `Popup.show` 提供便捷的弹出窗口辅助函数：

```js
const { Popup } = SillyTavern.getContext();

// 确认对话框——返回 POPUP_RESULT.AFFIRMATIVE 或 POPUP_RESULT.NEGATIVE
const confirmed = await Popup.show.confirm('Confirm Action', 'Are you sure you want to proceed?');

// 文本输入对话框——返回输入的字符串，取消则返回 null
const userInput = await Popup.show.input('Enter Name', 'Please provide a name:', 'default value');

// 信息显示——返回点击的按钮结果
await Popup.show.text('Info', 'Operation completed successfully.');
```

### 自定义弹出窗口

对于更复杂的弹出窗口，可以直接实例化 `Popup` 并传入完整选项：

```js
const { Popup, POPUP_TYPE, POPUP_RESULT } = SillyTavern.getContext();

const popup = new Popup(
    '<div>Custom HTML content here</div>',
    POPUP_TYPE.TEXT,
    '',
    {
        wide: true,              // 宽屏显示模式
        okButton: 'Save',       // 自定义确认按钮文本
        cancelButton: 'Discard', // 自定义取消按钮文本
        customButtons: [
            {
                text: 'Export',
                icon: 'fa-download',
                result: POPUP_RESULT.CUSTOM1,
            },
        ],
        customInputs: [
            {
                id: 'my_checkbox',
                label: 'Enable feature',
                type: 'checkbox',
                defaultState: false,
            },
        ],
        allowVerticalScrolling: true,
    }
);

const result = await popup.show();

if (result === POPUP_RESULT.AFFIRMATIVE) {
    // 点击了确认
} else if (result === POPUP_RESULT.CUSTOM1) {
    // 点击了导出按钮
}

// 读取自定义输入值
const checkboxValue = popup.inputResults?.get('my_checkbox');
```

### 弹出窗口类型

| 类型 | 描述 |
|------|------|
| `POPUP_TYPE.TEXT` | 带按钮的通用内容弹出窗口 |
| `POPUP_TYPE.CONFIRM` | 是/否确认对话框 |
| `POPUP_TYPE.INPUT` | 带文本输入框的弹出窗口 |
| `POPUP_TYPE.DISPLAY` | 仅含内容和关闭按钮的弹出窗口 |
| `POPUP_TYPE.CROP` | 图片裁剪弹出窗口 |

### 通知提示

对于轻量级反馈，使用全局可用的 `toastr`：

```js
toastr.success('Data saved successfully');
toastr.error('Failed to connect to API');
toastr.warning('This feature is experimental');
toastr.info('Processing...');
```

## 数据库抓取器

扩展可以为数据库功能注册自定义数据抓取器。抓取器提供了一种从自定义来源（如网页、API、文件格式）导入数据的方式：

```js
const { registerDataBankScraper } = SillyTavern.getContext();

await registerDataBankScraper({
    id: 'my_scraper',
    name: 'My Data Source',
    description: 'Import data from My Data Source',
    iconClass: 'fa-solid fa-database',
    iconAvailable: true,
    isAvailable: async () => true,
    scrape: async () => {
        // 返回 File 对象数组
        const content = await fetchDataFromSource();
        return [new File([content], 'data.txt', { type: 'text/plain' })];
    },
});
```

## 调试函数

扩展可以注册自定义调试函数，这些函数将出现在调试菜单（通过高级用户设置访问）中。这对于在开发期间暴露诊断工具、缓存/清理功能或手动触发器非常有用：

```js
const { registerDebugFunction } = SillyTavern.getContext();

registerDebugFunction(
    'my_ext_clear_cache',        // 唯一函数 ID
    'Clear My Extension Cache',   // 显示名称
    'Clears all cached data for My Extension', // 描述
    async () => {
        const { localforage } = SillyTavern.libs;
        await localforage.removeItem('my_extension_cache');
        toastr.success('Cache cleared');
    }
);
```

## Extras 请求

!!!warning
Extras API 已弃用。不建议在新扩展中使用它。
!!!

`doExtrasFetch()` 函数允许你向 SillyTavern Extras API 服务器发出请求。

例如，调用 `/api/summarize` 端点：

```js
import { getApiUrl, doExtrasFetch } from "../../extensions.js";

const url = new URL(getApiUrl());
url.pathname = '/api/summarize';

const apiResult = await doExtrasFetch(url, {
    method: 'POST',
    headers: {
        'Content-Type': 'application/json',
        'Bypass-Tunnel-Reminder': 'bypass',
    },
    body: JSON.stringify({
        // 请求体
    })
});
```

`getApiUrl()` 返回 Extras 服务器的基础 URL。

`doExtrasFetch()` 函数：

* 添加 `Authorization` 和 `Bypass-Tunnel-Reminder` 请求头
* 处理结果的获取
* 返回结果（响应对象）

这使得从扩展中调用 Extras API 变得非常简单。

你可以指定：

* 请求方法：GET、POST 等
* 额外的请求头
* POST 请求的请求体
* 任何其他 fetch 选项

## 最佳实践

### 安全性

**永远不要在 `extensionSettings` 中存储 API 密钥或机密信息**

扩展设置对所有其他扩展可见，并以明文形式存储。不要在客户端存储敏感数据：

```js
// 错误做法——请勿这样做！
extensionSettings[MODULE_NAME].apiKey = 'secret_key_123';

// 注意：没有安全的方式在客户端扩展中存储机密信息。
// 如果需要处理敏感数据，请改用服务器插件。
// 参见：https://docs.sillytavern.app/for-contributors/server-plugins/
```

**净化用户输入**

在将用户输入用于命令、API 调用或 DOM 操作之前，始终对其进行验证和净化：

```js
// 先验证输入类型
if (typeof userInput !== 'string') {
    toastr.error('Invalid input type');
    return;
}
// 使用 DOMPurify 净化 HTML 输入
const { DOMPurify } = SillyTavern.libs;
const cleanInput = DOMPurify.sanitize(userInput);
```

**避免使用 `eval()` 或 `Function()` 构造函数**

这些会执行任意代码并带来安全风险。如果需要动态求值，请使用更安全的替代方案或仔细限制输入。

### 性能

**不要在 `extensionSettings` 中存储大量数据**

扩展设置会加载到内存中并频繁保存。大量数据会导致性能问题：

```js
// 错误做法——不要存储大量数据
extensionSettings[MODULE_NAME].largeDataset = { /* 数 MB 的数据 */ };

// 正确做法——使用 localforage（IndexedDB/localStorage 的抽象层）
const { localforage } = SillyTavern.libs;
await localforage.setItem(`${MODULE_NAME}_data`, largeData);

// 或者使用 localStorage 存储较小的数据
localStorage.setItem(`${MODULE_NAME}_data`, JSON.stringify(smallData));
```

**清理事件监听器**

当事件监听器不再需要时，移除它们以防止内存泄漏：

```js
function cleanup() {
    eventSource.removeListener(event_types.MESSAGE_RECEIVED, handleMessage);
    document.getElementById('myElement').removeEventListener('click', handleClick);
}
```

**不要阻塞 UI 线程**

对于繁重操作，使用 async/await 或 Web Worker：

```js
// 对 I/O 操作使用 async
async function processData() {
    const result = await fetch('/api/process');
    return result.json();
}

// 拆分繁重的计算任务
async function heavyComputation(data) {
    for (let i = 0; i < data.length; i++) {
        // 处理数据块
        if (i % 1000 === 0) {
            await new Promise(resolve => setTimeout(resolve, 0)); // 让出 UI 控制权
        }
    }
}
```

### 兼容性

**优先使用 `getContext()` 而非直接导入**

context API 更稳定，随 SillyTavern 更新而失效的可能性更低：

```js
// 正确做法——稳定的 API
const { chat, characters, saveSettingsDebounced } = SillyTavern.getContext();

// 应避免——可能随内部变更而失效
import { chat, characters } from '../../../../script.js';
```

**使用唯一的模块名称**

通过使用描述性的唯一模块名称来防止与其他扩展冲突：

```js
// 正确做法——具体且唯一
const MODULE_NAME = 'my_extension_name';

// 错误做法——过于通用，容易冲突
const MODULE_NAME = 'settings';
```

### 用户体验

**提供清晰的反馈**

使用 `toastr` 进行轻量级通知，使用 `Popup` 进行重要的用户交互。完整详情请参阅[弹出窗口与用户反馈](#弹出窗口与用户反馈)部分。

对于长时间运行的操作，使用[操作加载器](#操作加载器)，而不是静默地阻塞 UI。

**提供有用的控制台消息**

为控制台日志使用一致的前缀，但不要在生产环境中输出过多日志：

```js
const MODULE_NAME = 'MyExtension';

console.log(`[${MODULE_NAME}] Extension loaded`);
console.debug(`[${MODULE_NAME}] Processing data:`, data);
console.error(`[${MODULE_NAME}] Error occurred:`, error);
```

### 代码质量

**使用 `lib.js` 中的打包库**

在添加新依赖之前，请查阅[共享库](#共享库)部分——SillyTavern 打包了许多常用库（lodash、Fuse、DOMPurify、moment、yaml 等），这些库可通过 `SillyTavern.libs` 访问。

**正确初始化设置**

始终提供默认值并处理缺失的键：

```js
function loadSettings() {
    // 与默认值合并，以处理更新后的新键并在不存在时初始化。
    extensionSettings[MODULE_NAME] = SillyTavern.libs.lodash.merge(
        structuredClone(defaultSettings),
        extensionSettings[MODULE_NAME]
    );
}
```
