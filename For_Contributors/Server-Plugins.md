---
order: -30
icon: server
route: /for-contributors/server-plugins/
---

# Server Plugins

这些插件允许添加仅使用 UI 扩展无法实现的功能,例如创建新的 API 端点或使用浏览器环境中不可用的 Node.JS 包。

插件包含在 SillyTavern 的 `plugins` 目录中,并在服务器启动时加载,但*仅*在 `config.yaml` 文件中将 `enableServerPlugins` 设置为 `true` 时才会加载。

!!!warning 警告
 **Server Plugins 没有沙箱隔离。这意味着它们可能会访问您的整个文件系统,或以普通 UI 扩展无法做到的方式引入各种安全漏洞。只安装来自您信任的开发者的 server plugins!**
!!!

有关所有官方 server plugins 的列表,请参阅 GitHub 组织列表: <https://github.com/search?q=topic%3Aplugin+org%3ASillyTavern&type=Repositories>

## 插件类型

### 文件

一个可执行的 JavaScript 文件,扩展名为 ".js"(用于 CommonJS 模块)或 ".mjs"(用于 ES 模块),包含一个导出 `init` 函数的模块。此函数接受一个 Express router(专门为您的插件创建)作为参数并返回一个 Promise。

模块还应该导出一个 `info` 对象,其中包含有关插件的信息(`id`、`name` 和 `description` 字符串)。这将向加载器提供有关插件的信息。

您可以通过 router 注册路由,这些路由将在 `/api/plugins/{id}/{route}` 路径下注册。例如,`example` 插件的 `router.get('/foo')` 将产生如下路由:`/api/plugins/example/foo`。

插件还可以*可选地*导出一个 `exit` 函数,在关闭服务器时执行清理工作。它应该没有参数,并且必须返回一个 Promise。

插件导出的 TypeScript 契约:

```ts
interface PluginInfo {
    id: string;
    name: string;
    description: string;
}

interface Plugin {
    init: (router: Router) => Promise<void>;
    exit: () => Promise<void>;
    info: PluginInfo;
}
```

请参阅下面的 "Hello world!" 插件示例:

```js
/**
 * Initialize plugin.
 * @param {import('express').Router} router Express router
 * @returns {Promise<any>} Promise that resolves when plugin is initialized
 */
async function init(router) {
    // Do initialization here...
    router.get('/foo', req, res, function () {
       res.send('bar');
    });
    console.log('Example plugin loaded!');
    return Promise.resolve();
}

async function exit() {
    // Do some clean-up here...
    return Promise.resolve();
}

module.exports = {
    init,
    exit,
    info: {
        id: 'example',
        name: 'Example',
        description: 'My cool plugin!',
    },
};
```

### 目录

您可以通过以下方式之一(按优先顺序)从 `plugins` 目录的子目录加载插件:

1. 包含 "main" 字段中指向可执行文件路径的 `package.json` 文件。
2. 用于 CommonJS 模块的 `index.js` 文件。
3. 用于 ES 模块的 `index.mjs` 文件。

生成的文件必须导出一个 `init` 函数和一个 `info` 对象,其要求与单个文件相同。

目录插件示例(带有 `index.js` 文件): <https://github.com/SillyTavern/SillyTavern-DiscordRichPresence-Server>

### 打包

最好使用打包工具(如 Webpack 或 Browserify)将所有要求打包到一个文件中。确保将 "Node" 设置为构建目标。

使用 Webpack 和 TypeScript 的插件模板仓库: <https://github.com/SillyTavern/Plugin-WebpackTemplate>
