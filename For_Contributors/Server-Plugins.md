---
order: -30
icon: server
route: /for-contributors/server-plugins/
---


# 服务器插件

服务器插件是一种扩展 SillyTavern 服务器功能的方式。它们可以添加新的 API 端点、处理程序和其他功能。

## 创建插件

1. 在 `plugins` 目录中创建一个新目录。目录名称将成为插件的名称。
2. 在插件目录中创建一个 `index.js` 文件。这是插件的入口点。
3. 在 `index.js` 中导出一个包含以下方法的对象：

```javascript
module.exports = {
    // 在服务器启动时调用
    async init(app) {
        // app 是 Express 应用程序实例
        // 在这里添加您的路由和中间件
    },

    // 在服务器关闭时调用
    async stop() {
        // 在这里清理资源
    }
}
```

## 示例插件

```javascript
const express = require('express');

module.exports = {
    async init(app) {
        // 创建一个新的路由器
        const router = express.Router();

        // 添加一个新的 API 端点
        router.get('/hello', (req, res) => {
            res.json({ message: 'Hello from plugin!' });
        });

        // 将路由器挂载到 /api/plugins/[plugin-name]
        app.use('/api/plugins/example', router);
    },

    async stop() {
        // 清理代码
    }
}
```

## 启用插件

要启用插件，请在 `config.yaml` 中添加以下内容：

```yaml
plugins:
  # 插件目录的名称
  - example
```

## 插件 API

插件可以访问以下 API：

- `app` - Express 应用程序实例
- `express` - Express 模块
- `config` - 服务器配置对象
- `jsonwebtoken` - JWT 模块，用于身份验证
- `path` - Node.js path 模块
- `fs` - Node.js fs 模块
- `logger` - Winston 日志记录器实例
