---
order: -20
icon: file-added
templating: false
route: /for-contributors/writing-extensions/
---

# UI Extensions

UI 扩展通过挂钩到 SillyTavern 的事件和 API 来扩展其功能。它们在浏览器环境中运行,几乎可以无限制地访问 DOM、JavaScript API 和 SillyTavern 上下文。扩展可以修改 UI、调用内部 API 并与聊天数据交互。本指南介绍如何创建您自己的扩展(需要 JavaScript 知识)。

!!!tip 只想安装扩展?
前往这里:[Extensions](../extensions/index.md)。
!!!

要扩展 Node.js 服务器的功能,请参阅 [Server Plugins](./Server-Plugins.md) 页面。

**不会写 JavaScript?**

* 考虑使用 [STscript](./st-script.md) 作为编写完整扩展的简单替代方案。
* 学习 [MDN 课程](https://developer.mozilla.org/en-US/docs/Learn/JavaScript),完成后再回来。

## 扩展提交

想要将您的扩展贡献到 [官方内容仓库](https://github.com/SillyTavern/SillyTavern-Content)? 联系我们!

为确保所有扩展安全且易于使用,我们有一些要求:

1. 您的扩展必须是开源的,并具有自由许可证(参见 [选择许可证](https://choosealicense.com/licenses/))。如果不确定,AGPLv3 是个不错的选择。
2. 扩展必须与最新发布版本的 SillyTavern 兼容。如果核心发生变化,请准备好更新您的扩展。
3. 扩展必须有完善的文档。这包括一个包含安装说明、使用示例和功能列表的 README 文件。
4. 需要 server plugin 才能运行的扩展将不被接受。

## 示例

查看简单 SillyTavern 扩展的实时示例:

* <https://github.com/city-unit/st-extension-example> - 基础扩展模板。展示 manifest 创建、本地脚本导入、添加设置 UI 面板和持久化扩展设置的使用。
* <https://github.com/search?q=topic%3Aextension+org%3ASillyTavern&type=Repositories> - GitHub 上所有官方 SillyTavern 扩展的列表。

## 打包

扩展还可以使用打包工具将自己与其他模块隔离,并使用 NPM 中的任何依赖项,包括 Vue、React 等 UI 框架。

* <https://github.com/SillyTavern/Extension-WebpackTemplate> - 使用 TypeScript 和 Webpack 的扩展模板仓库(无 React)。
* <https://github.com/SillyTavern/Extension-ReactTemplate> - 使用 React 和 Webpack 的基础扩展模板仓库。

要从打包的文件中使用相对导入,您可能需要创建一个导入包装器。以下是 Webpack 的示例:

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

每个扩展都必须在 `data/<user-handle>/extensions` 中有一个文件夹和一个 `manifest.json` 文件,该文件包含有关扩展的元数据以及作为扩展入口点的 JS 脚本文件的路径。

可下载的扩展在通过 HTTP 提供服务时会被挂载到 `/scripts/extensions/third-party` 文件夹中,因此应该基于此使用相对导入。为方便本地开发,建议将扩展仓库放在 `/scripts/extensions/third-party` 文件夹中("为所有用户安装"选项)。

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
    }
}
```

### Manifest 字段

* `display_name` 是必需的。它显示在"管理扩展"菜单中。
* `loading_order` 是可选的。数字越大,加载越晚。
* `js` 是主 JS 文件引用,是必需的。
* `css` 是可选的样式文件引用。
* `author` 是必需的。应包含作者的姓名或联系信息。
* `auto_update` 设置为 `true` 时,当 ST 包的版本更改时,扩展将自动更新。
* `i18n` 是一个可选对象,指定支持的语言环境及其对应的 JSON 文件(见下文)。
* `dependencies` 是一个可选的字符串数组,指定此扩展依赖的其他**扩展**。
* `generate_interceptor` 是一个可选字符串,指定在文本生成请求时调用的全局函数名称。
* `minimum_client_version` 是一个可选字符串,指定此扩展正常工作所需的最低 SillyTavern 版本。

### 依赖项

扩展也可以依赖其他 SillyTavern 扩展。如果缺少或禁用了这些依赖项中的任何一个,扩展将不会加载。

依赖项由其在 `public/extensions` 目录中显示的**文件夹名称**指定。

示例:

* 内置扩展: `"vectors"`, `"caption"`
* 第三方扩展: `"third-party/Extension-WebLLM"`, `"third-party/Extension-Mermaid"`

### 已弃用的字段

* `requires` 是一个可选的字符串数组,指定所需的 **Extras 模块**。如果连接的 Extras API 不提供所有列出的模块,扩展将不会加载。
* `optional` 是一个可选的字符串数组,指定可选的 **Extras 模块**。如果缺少这些模块,扩展仍将加载,扩展应优雅地处理它们的缺失。

要检查当前连接的 Extras API 提供哪些模块,请从 `scripts/extensions.js` 导入 `modules` 数组。

## 脚本编写

### 使用 getContext

`SillyTavern` 全局对象中的 `getContext()` 函数可以让您访问 SillyTavern 上下文,这是所有主要应用状态对象、有用函数和实用程序的集合。

```js
const context = SillyTavern.getContext();
context.chat; // Chat log - MUTABLE
context.characters; // Character list
context.characterId; // Index of the current character
context.groups; // Group list
context.groupId; // ID of the current group
// And many more...
```

您可以在 [SillyTavern 源代码](https://github.com/SillyTavern/SillyTavern/blob/staging/public/scripts/st-context.js)中找到可用属性和函数的完整列表。

!!!
如果 `getContext` 中缺少任何您需要的函数/属性,请联系开发人员或向我们发送 pull request!
!!!

### 共享库

SillyTavern 前端内部使用的大多数 npm 库都在 `SillyTavern` 全局对象的 `libs` 属性中共享。

* `lodash` - 实用程序库。[文档](https://lodash.com/)。
* `localforage` - 浏览器存储库。[文档](https://localforage.github.io/localForage/)。
* `Fuse` - 模糊搜索库。[文档](https://www.fusejs.io/)。
* `DOMPurify` - HTML 清理库。[文档](https://github.com/cure53/DOMPurify)。
* `Handlebars` - 模板库。[文档](https://handlebarsjs.com/)。
* `moment` - 日期/时间操作库。[文档](http://momentjs.com/)。
* `showdown` - Markdown 转换库。[文档](https://showdownjs.com/)。

您可以在 [SillyTavern 源代码](https://github.com/SillyTavern/SillyTavern/blob/staging/public/lib.js)中找到导出库的完整列表。

**示例:** 使用 DOMPurify 库。

```js
const { DOMPurify } = SillyTavern.libs;

const sanitizedHtml = DOMPurify.sanitize('<script>"dirty HTML"</script>');
```

### TypeScript 注意事项

如果您想要 `SillyTavern` 全局对象中所有方法的自动完成功能(您可能想要),包括 `getContext()` 和 `libs`,您应该添加一个 TypeScript `.d.ts` 模块声明。此声明应从 SillyTavern 的源代码导入全局类型,具体取决于您的扩展位置。以下示例适用于两种安装类型:"所有用户"和"当前用户"。

**global.d.ts** - 将此文件放在扩展目录的根目录中(`manifest.json` 旁边):

```ts
export {};

// 1. Import for user-scoped extensions
import '../../../../public/global';
// 2. Import for server-scoped extensions
import '../../../../global';

// Define additional types if needed...
declare global {
    // Add global type declarations here
}
```

### 从其他文件导入

!!!warning
从 SillyTavern 代码导入是不可靠的,如果 ST 模块的内部结构发生变化,可能会随时中断。`getContext` 提供了更稳定的 API。
!!!

除非您正在构建打包的扩展,否则您可以从其他 JS 文件导入变量和函数。

例如,此代码片段将在后台从当前选择的 API 生成回复:

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

当扩展需要持久化其状态时,它可以使用 `getContext()` 函数中的 `extensionSettings` 对象来存储和检索数据。扩展可以在设置对象中存储任何 JSON 可序列化的数据,并且必须使用唯一的键来避免与其他扩展冲突。

要持久化设置,请使用 `saveSettingsDebounced()` 函数,该函数将设置保存到服务器。

```js
const { extensionSettings, saveSettingsDebounced } = SillyTavern.getContext();

// Define a unique identifier for your extension
const MODULE_NAME = 'my_extension';

// Define default settings
const defaultSettings = Object.freeze({
    enabled: false,
    option1: 'default',
    option2: 5
});

// Define a function to get or initialize settings
function getSettings() {
    // Initialize settings if they don't exist
    if (!extensionSettings[MODULE_NAME]) {
        extensionSettings[MODULE_NAME] = structuredClone(defaultSettings);
    }

    // Ensure all default keys exist (helpful after updates)
    for (const key of Object.keys(defaultSettings)) {
        if (!Object.hasOwn(extensionSettings[MODULE_NAME], key)) {
            extensionSettings[MODULE_NAME][key] = defaultSettings[key];
        }
    }

    return extensionSettings[MODULE_NAME];
}

// Use the settings
const settings = getSettings();
settings.option1 = 'new value';

// Save the settings
saveSettingsDebounced();
```

### 聊天元数据

要将某些数据绑定到特定聊天,您可以使用 `getContext()` 函数中的 `chatMetadata` 对象。此对象允许您存储与聊天关联的任意数据,这对于存储特定于扩展的状态很有用。

要持久化元数据,请使用 `saveMetadata()` 函数,该函数将元数据保存到服务器。

!!!warning
不要在长期变量中保存对 `chatMetadata` 的引用,因为切换聊天时引用会改变。始终使用 `SillyTavern.getContext().chatMetadata` 来访问当前聊天元数据。
!!!

```js
const { chatMetadata, saveMetadata } = SillyTavern.getContext();

// Set some metadata for the current chat
chatMetadata['my_key'] = 'my_value';

// Get the metadata for the current chat
const value = chatMetadata['my_key'];

// Save the metadata to the server
await saveMetadata();
```

!!!tip
切换聊天时会发出 `CHAT_CHANGED` 事件,因此您可以监听此事件以相应地更新扩展的状态。请参阅[监听事件](#listening-to-events)部分了解更多信息。
!!!

### 角色卡片

SillyTavern 完全支持 [Character Cards V2 规范](https://github.com/malfoyslastname/character-card-spec-v2/blob/main/spec_v2.md),允许在角色卡片 JSON 数据中存储任意数据。

这对于需要存储与角色关联的附加数据并在导出角色卡片时使其可共享的扩展很有用。

要将数据写入角色卡片 [extensions](https://github.com/malfoyslastname/character-card-spec-v2/blob/main/spec_v2.md#extensions) 数据字段,请使用 `getContext()` 函数中的 `writeExtensionField` 函数。此函数接受角色 ID、字符串键和要写入的值。该值必须是 JSON 可序列化的。

!!!warning 注意奇怪的地方
尽管被称为 `characterId`,但它不是"真正的"唯一标识符,而是 `characters` 数组中角色的索引。

当前角色的索引由上下文中的 `characterId` 属性提供。如果要将数据写入当前选择的角色,请使用 `SillyTavern.getContext().characterId`。如果需要为另一个角色存储数据,请通过在 `characters` 数组中搜索角色来找到索引。

**注意:`characterId` 在群聊或未选择角色时为 `undefined`!**
!!!

```js
const { writeExtensionField, characterId } = SillyTavern.getContext();

// Write some data to the character card
await writeExtensionField(characterId, 'my_extension_key', {
    someData: 'value',
    anotherData: 42
});

// Read the data back from the character card
const character = SillyTavern.getContext().characters[characterId];
// The data is stored in the `extensions` object of the character's data
const myData = character.data?.extensions?.my_extension_key;
```

### 设置预设

任意 JSON 数据可以存储在主要 API 类型的设置预设中。它将与预设 JSON 一起导出和导入,因此您可以使用它来存储预设的扩展特定设置。以下 API 类型支持设置预设中的数据扩展:

* Chat Completion
* Text Completion
* NovelAI
* KoboldAI / AI Horde

要读取或写入数据,您首先需要从上下文获取 PresetManager 实例:

```js
const { getPresetManager } = SillyTavern.getContext();

// Get the preset manager for the current API type
const pm = getPresetManager();

// Write data to the preset extension field:
// - path: the path to the field in the preset data
// - value: the value to write
// - name (optional): the name of the preset to write to, defaults to the currently selected preset
await pm.writePresetExtensionField({ path: 'hello', value: 'world' });

// Read data from the preset extension field:
// - path: the path to the field in the preset data
// - name (optional): the name of the preset to read from, defaults to the currently selected preset
const value = pm.readPresetExtensionField({ path: 'hello' });
```

!!!tip
切换预设或主 API 时会发出 `PRESET_CHANGED` 和 `MAIN_API_CHANGED` 事件,因此您可以监听这些事件以相应地更新扩展的状态。请参阅[监听事件](#listening-to-events)部分了解更多信息。
!!!

## 国际化

!!!
有关提供翻译的一般信息,请参阅[国际化](/For_Contributors/i18n.md)页面。
!!!

扩展可以提供额外的本地化字符串,以便与 `t`、`translate` 函数和 HTML 模板中的 `data-i18n` 属性一起使用。

在此处查看支持的语言环境列表(`lang` 键): <https://github.com/SillyTavern/SillyTavern/blob/release/public/locales/lang.json>

### 直接调用 `addLocaleData`

将语言环境代码和包含翻译的对象传递给 `addLocaleData` 函数。*不*允许覆盖现有键。如果传递的语言环境代码不是当前选择的语言环境,数据将被静默忽略。

```js
SillyTavern.getContext().addLocaleData('fr-fr', { 'Hello': 'Bonjour' });
SillyTavern.getContext().addLocaleData('de-de', { 'Hello': 'Hallo' });
```

### 通过扩展 manifest

将包含支持的语言环境列表及其对应 JSON 文件路径(相对于扩展目录)的 i18n 对象添加到 manifest 中。

```json
{
  "display_name": "Foobar",
  "js": "index.js",
  // rest of the fields
  "i18n": {
    "fr-fr": "i18n/french.json",
    "de-de": "i18n/german.json"
  }
}
```

## 注册 slash 命令(新方法)

虽然 `registerSlashCommand` 仍然存在以实现向后兼容,但现在应该通过 `SlashCommandParser.addCommandObject()` 注册新的 slash 命令,以向解析器(进而向自动完成和命令帮助)提供有关命令及其参数的扩展详细信息。

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

使用 `eventSource.on(eventType, eventHandler)` 来监听事件:

```js
const { eventSource, event_types } = SillyTavern.getContext();

eventSource.on(event_types.MESSAGE_RECEIVED, handleIncomingMessage);

function handleIncomingMessage(data) {
    // Handle message
}
```

主要事件类型:

* `APP_READY`: 应用完全加载并准备使用。应用准备就绪后,每次附加新监听器时都会自动触发。
* `MESSAGE_RECEIVED`: LLM 消息已生成并记录到 `chat` 对象中,但尚未在 UI 中渲染。
* `MESSAGE_SENT`: 消息由用户发送并记录到 `chat` 对象中,但尚未在 UI 中渲染。
* `USER_MESSAGE_RENDERED`: 用户发送的消息在 UI 中渲染。
* `CHARACTER_MESSAGE_RENDERED`: 生成的 LLM 消息在 UI 中渲染。
* `CHAT_CHANGED`: 聊天已切换(例如,切换到另一个角色,或加载了另一个聊天)。
* `GENERATION_AFTER_COMMANDS`: 在处理 slash 命令后即将开始生成。
* `GENERATION_STOPPED`: 生成被用户停止。
* `GENERATION_ENDED`: 生成已完成或出错。
* `SETTINGS_UPDATED`: 应用程序设置已更新。

其余可以在[源代码](https://github.com/SillyTavern/SillyTavern/blob/staging/public/scripts/events.js)中找到。

!!!info 事件数据
每个事件将数据传递给监听器的方式不统一。有些事件不发出任何数据;有些传递对象或原始值。请参阅发出事件的源代码以查看它传递的数据,或使用调试器检查。
!!!

### 发出事件

您可以通过调用 `eventSource.emit(eventType, ...eventData)` 从扩展产生任何应用程序事件,包括自定义事件:

```js
const { eventSource } = SillyTavern.getContext();

// Can be a built-in event_types field or any string.
const eventType = 'myCustomEvent';

// Use `await` to ensure all event handlers complete before continuing execution.
await eventSource.emit(eventType, { data: 'custom event data' });
```

## Prompt Interceptors

Prompt Interceptors 提供了一种方式,允许扩展在发出文本生成请求之前执行任何活动,例如修改聊天数据、添加注入或中止生成。

来自不同扩展的 interceptors 按顺序运行。顺序由其各自 `manifest.json` 文件中的 `loading_order` 字段确定。`loading_order` 值较低的扩展较早运行。如果未指定 `loading_order`,则使用 `display_name` 作为后备。如果两者都未指定,则顺序未定义。

### 注册 Interceptor

要定义 prompt interceptor,请在扩展的 `manifest.json` 文件中添加 `generate_interceptor` 字段。该值应该是 SillyTavern 将调用的全局函数的名称。

```json
{
    "display_name": "My Interceptor Extension",
    "loading_order": 10, // Affects execution order
    "generate_interceptor": "myCustomInterceptorFunction",
    // ... other manifest properties
}
```

### Interceptor 函数

`generate_interceptor` 函数是一个全局函数,将在非试运行的生成请求时调用。它必须在全局作用域中定义(例如,`globalThis.myCustomInterceptorFunction = async function(...) { ... }`),并且如果需要执行任何异步操作,可以返回 `Promise`。

interceptor 函数接收以下参数:

* `chat`: 表示将用于构建 prompt 的聊天历史的消息对象数组。您可以直接修改此数组(例如,添加、删除或更改消息)。请注意,消息是可变的,因此您对数组所做的任何更改都将反映在实际聊天历史中。如果您希望更改是临时的,请使用 `structuredClone` 创建消息对象的深层副本。
* `contextSize`: 表示为即将进行的生成计算的当前上下文大小(以 tokens 为单位)的数字。
* `abort`: 调用时将发出信号以阻止文本生成继续进行的函数。它接受一个布尔参数,如果为 `true`,则阻止任何后续 interceptors 运行。
* `type`: 表示生成的类型或触发器的字符串(例如,`'quiet'`、`'regenerate'`、`'impersonate'`、`'swipe'` 等)。这有助于 interceptor 根据生成的启动方式有条件地应用逻辑。

**示例实现:**

```javascript
globalThis.myCustomInterceptorFunction = async function(chat, contextSize, abort, type) {
    // Example: Add a system note before the last user message
    const systemNote = {
        is_user: false,
        name: "System Note",
        send_date: Date.now(),
        mes: "This was added by my extension!"
    };
    // Insert before the last message
    chat.splice(chat.length - 1, 0, systemNote);
}
```

## 生成文本

SillyTavern 提供了几个函数,可以使用当前选择的 LLM API 在不同上下文中生成文本。这些函数允许您在聊天上下文中生成文本、无需任何上下文的原始生成,或使用结构化输出。

### 在聊天上下文中

`generateQuietPrompt()` 函数用于在后台(输出不在 UI 中渲染)在聊天上下文中使用添加的"quiet" prompt(历史后指令)生成文本。这对于在不中断用户体验的同时生成文本很有用,同时还保持相关的聊天和角色数据完整,例如生成摘要或图像 prompt。

```js
const { generateQuietPrompt } = SillyTavern.getContext();

const quietPrompt = 'Generate a summary of the chat history.';

const result = await generateQuietPrompt({
    quietPrompt,
});
```

### 原始生成

`generateRaw()` 函数用于在没有任何聊天上下文的情况下生成文本。当您想要完全控制 prompt 构建过程时,它很有用。

它接受 `prompt` 作为 Text Completion 字符串或 Chat Completion 对象数组,根据所选 API 类型以适当的格式构造请求,例如,在 chat/text 模式之间转换、应用 instruct 格式等。您还可以将额外的 `systemPrompt` 和 `prefill` 传递给函数,以更好地控制生成过程。

```js
const { generateRaw } = SillyTavern.getContext();

const systemPrompt = 'You are a helpful assistant.';
const prompt = 'Generate a story about a brave knight.';
const prefill = 'Once upon a time,';

/*
In Chat Completion mode, will produce a prompt like this:
[
  {role: 'system', content: 'You are a helpful assistant.'},
  {role: 'user', content: 'Generate a story about a brave knight.'},
  {role: 'assistant', content: 'Once upon a time,'}
]
*/

/*
In Text Completion mode (no instruct), will produce a prompt like this:
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
目前仅 Chat Completion API 支持。可用性因所选源和模型而异。如果所选模型不支持结构化输出,生成将失败或返回空对象(`'{}'`)。请查看您使用的特定 API 的文档以查看是否支持结构化输出。
!!!

您可以使用结构化输出功能来确保模型生成符合提供的 [JSON Schema](https://json-schema.org/learn) 的有效 JSON 对象。这对于需要结构化数据的扩展很有用,例如状态跟踪、数据分类等。

要使用结构化输出,您必须将 JSON schema 对象传递给 `generateRaw()` 或 `generateQuietPrompt()`。然后,模型将生成与 schema 匹配的响应,并将其作为字符串化的 JSON 对象返回。

!!!warning
输出不会根据 schema 进行验证,您必须自己处理生成输出的解析和验证。如果模型无法生成有效的 JSON 对象,函数将返回空对象(`'{}'`)。

[Zod](https://zod.dev/json-schema) 是一个流行的库,用于生成和验证 JSON schemas。这里不会涉及其使用。
!!!

```js
const { generateRaw, generateQuietPrompt } = SillyTavern.getContext();

// Define a JSON schema for the expected output
const jsonSchema = {
    // Required: a name for the schema
    name: 'StoryStateModel',
    // Optional: a description of the schema
    description: 'A schema for a story state with location, plans, and memories.',
    // Optional:  the schema will be used in strict mode, meaning that only the fields defined in the schema will be allowed
    strict: true,
    // Required: a definition of the schema
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

您可以注册自定义宏,这些宏可以在支持宏替换的任何地方使用,例如在角色卡片字段、STscript 命令、prompt 模板等中。

要注册宏,请使用 `SillyTavern.getContext()` 对象中的 `registerMacro()` 函数。该函数接受一个宏名称(应该是唯一的字符串)和一个字符串或返回字符串的函数。该函数将使用唯一的 `nonce` 字符串调用,该字符串在每次 `substituteParams` 调用之间都会不同。

```js
const { registerMacro } = SillyTavern.getContext();

// Simple string macro
registerMacro('fizz', 'buzz');
// Function macro
registerMacro('tomorrow', () => {
    return new Date(Date.now() + 24 * 60 * 60 * 1000).toLocaleDateString();
});
```

当不再需要自定义宏时,使用 `unregisterMacro()` 函数删除它:

```js
const { unregisterMacro } = SillyTavern.getContext();

// Unregister the 'fizz' macro
unregisterMacro('fizz');
```

**有关自定义宏的重要详细信息和已知限制:**

1. 目前仅支持简单的字符串替换宏。我们正在努力在未来添加对更复杂宏的支持。
2. 使用函数提供值的宏*必须*是同步的。返回 `Promise` 将不起作用。
3. 注册时不需要用双花括号(`{{ }}`)包装宏名称。SillyTavern 会为您执行此操作。
4. 由于宏是纯正则表达式替换,注册大量宏会导致性能问题,因此请谨慎使用。

## 执行 Extras 请求

!!!warning
Extras API 已弃用。不建议在新扩展中使用它。
!!!

`doExtrasFetch()` 函数允许您向 SillyTavern Extras API 服务器发出请求。

例如,要调用 `/api/summarize` 端点:

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
        // Request body
    })
});
```

`getApiUrl()` 返回 Extras 服务器的基本 URL。

`doExtrasFetch()` 函数:

* 添加 `Authorization` 和 `Bypass-Tunnel-Reminder` headers
* 处理获取结果
* 返回结果(响应对象)

这使得从扩展调用 Extras API 变得容易。

您可以指定:

* 请求方法: GET、POST 等。
* 附加 headers
* POST 请求的 body
* 任何其他 fetch 选项
