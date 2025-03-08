---
order: 100
icon: person-fill
---

# 角色

角色是您可以创建和管理的 AI 身份，用于塑造 AI 在对话中的角色。每个角色都有名字、个性和对话历史。您可以创建任意数量的角色，并随时在它们之间切换。

角色可以用于单独聊天，或者添加多个角色到群组聊天中，让它们相互交流。

## 角色管理面板

从导航栏打开 <i class="fa-solid fa-address-card"></i> **角色** 面板以访问角色列表。点击一个角色或群组来与其聊天或编辑，或选择 <i class="fa-solid fa-user-plus"></i> **创建新角色** 来添加新角色。

### 面板控制

* <i class="fa-solid fa-lock"></i> **固定面板**：在交互时保持面板打开
* <i class="fa-solid fa-list-ul"></i> **角色列表**：返回角色列表视图
* **快速切换栏**：快速访问收藏的角色

### 角色列表

* <i class="fa-solid fa-user-plus"></i> **创建新角色**：添加新角色
* <i class="fa-solid fa-file-import"></i> **导入角色**：从文件加载角色
* <i class="fa-solid fa-cloud-arrow-down"></i> **外部导入**：从 URL 导入
* <i class="fa-solid fa-users-gear"></i> **创建群组**：开始新的群组聊天

#### 搜索和排序

* **搜索栏**：按名称或属性筛选角色
* **排序下拉菜单**：多种排序选项：
    - 字母顺序（A-Z，Z-A）
    - 时间顺序（最新，最旧）
    - 使用情况（最近，最多/最少聊天）
    - 大小（最多/最少 token）
    - 特殊（收藏，随机）

#### 按类型或标签筛选角色

* <i class="fa-solid fa-star"></i> **收藏筛选**：显示收藏的角色
* <i class="fa-solid fa-users"></i> **群组筛选**：仅显示群组聊天
* <i class="fa-solid fa-folder-plus"></i> **标签作为文件夹**：按标签层级组织
* <i class="fa-solid fa-gear"></i> **管理标签**：[标签配置](/Usage/Characters/Tags.md)
* <i class="fa-solid fa-tags"></i> **标签列表**：查看所有可用标签
* <i class="fa-solid fa-filter-circle-xmark"></i> **清除筛选**：重置所有筛选器

### 角色创建/编辑面板

* **头像图片**：上传和预览角色头像
* **Token 数量**：角色的 [Token 使用量](characterdesign.md#角色-token)
* <i class="fa-solid fa-ranking-star"></i> **统计**：聊天历史和使用统计
* [标签管理](/Usage/Characters/Tags.md)

#### 快速操作

- <i class="fa-solid fa-star"></i> 收藏切换
- <i class="fa-solid fa-book"></i> 高级定义
- <i class="fa-solid fa-globe"></i> 角色设定
- <i class="fa-solid fa-passport"></i> 聊天设定：将聊天链接到 [世界信息](/Usage/worldinfo.md)
- <i class="fa-solid fa-file-export"></i> 导出角色
- <i class="fa-solid fa-clone"></i> 复制
- <i class="fa-solid fa-skull"></i> 删除

#### 扩展选项

* 世界信息链接
* 卡片设定导入
* 场景覆盖
* 个性转换
* 角色重命名
* 源链接
* 替换/更新
* 标签导入
* 图库视图

#### 内容字段

* **[角色描述](characterdesign.md#角色描述)**：简短的角色概述
* **[第一条消息](characterdesign.md#第一条消息)**：开始新聊天时的初始问候或提示
* **替代问候语**：定义多个第一条消息，在开始聊天时可以滑动切换

### 高级定义面板

点击 <i class="fa-solid fa-book"></i> **高级定义** 按钮访问扩展角色设置。

#### 提示词覆盖（聊天补全/指令模式）

* **主提示词**：替换默认的 [主/系统提示词](/Usage/Prompts/prompts.md#主提示词系统提示词)，可以使用 \{\{original\}\} 占位符包含原始提示词
* **历史后指令**：覆盖默认的 [历史后指令](/Usage/Prompts/prompts.md#历史后指令)

#### 创建者元数据

关于角色的非提示词信息：

- 创建者名称/联系方式
- 角色版本
- 创建者注释
- 嵌入的标签列表

#### 角色个性

* **[个性概述](characterdesign.md#个性概述)**：角色特征的简要概述
* **[场景](characterdesign.md#场景)**：对话的背景和情境
* **角色注释**：自定义消息，可选择深度和消息角色（另见 [作者注释](/Usage/Characters/Author's-Note.md)）
* **健谈程度**（群组聊天）：滑块从 害羞 → 正常 → 健谈
* **示例消息**：角色写作风格的示例

### 群组聊天管理

如果这是一个群组聊天，您可以从此面板管理群组成员和设置。

更多详情请参见 [群组聊天](/Usage/Characters/groupchats.md)。
