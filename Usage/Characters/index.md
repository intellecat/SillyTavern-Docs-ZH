---
order: 100
icon: person-fill
route: /usage/characters/
---

# 角色

角色是您可以创建和管理的 AI 身份，用于塑造 AI 在对话中扮演的角色。每个角色都拥有名称、个性和对话历史。您可以创建任意数量的角色，并随时在它们之间切换。

角色可用于单人聊天，也可以将多个角色加入群组聊天，让它们相互交流。

## 角色管理面板

从导航栏打开 <i class="fa-solid fa-address-card"></i> **角色** 面板以访问角色列表。点击某个角色或群组即可与其聊天或进行编辑，或选择 <i class="fa-solid fa-user-plus"></i> **创建新角色** 来添加新角色。

### 面板控制

* <i class="fa-solid fa-lock"></i> **固定面板**：在交互时保持面板打开
* <i class="fa-solid fa-list-ul"></i> **角色列表**：返回角色列表视图
* **快速切换栏**：快速访问收藏的角色

### 角色列表

* <i class="fa-solid fa-user-plus"></i> **创建新角色**：添加新角色
* <i class="fa-solid fa-file-import"></i> **导入角色**：从文件加载角色
* <i class="fa-solid fa-cloud-arrow-down"></i> **外部导入**：从 URL 导入
* <i class="fa-solid fa-users-gear"></i> **创建群组**：开始新的群组聊天

#### 搜索与排序

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
* **Token 数量**：角色的 [Token 使用量](characterdesign.md#character-tokens)
* <i class="fa-solid fa-ranking-star"></i> **统计**：聊天历史和使用统计
* [标签管理](/Usage/Characters/Tags.md)

#### 快速操作

- <i class="fa-solid fa-star"></i> 收藏切换
- <i class="fa-solid fa-book"></i> 高级定义
- <i class="fa-solid fa-globe"></i> [角色知识库](/Usage/worldinfo.md#character-lore)
- <i class="fa-solid fa-passport"></i> [聊天知识库](/Usage/worldinfo.md#chat-lorebook)：将聊天链接到世界信息
- <i class="fa-solid fa-file-export"></i> 导出角色
- <i class="fa-solid fa-clone"></i> 复制
- <i class="fa-solid fa-skull"></i> 删除

#### 扩展选项

* 世界信息链接
* 卡片知识库导入
* 场景覆盖
* 个性转换
* 角色重命名
* 来源链接
* 替换/更新
* 标签导入
* 图库视图

#### 内容字段

* **[角色描述](characterdesign.md#character-description)**：简短的角色概述
* **[第一条消息](characterdesign.md#first-message)**：开始新聊天时的初始问候或提示
* **备用问候语**：定义多条第一消息，开始聊天时可滑动切换

### 高级定义面板

点击 <i class="fa-solid fa-book"></i> **高级定义** 按钮访问扩展角色设置。

#### 提示词覆盖（聊天补全/指令模式）

* **主提示词**：替换默认的[主/系统提示词](/Usage/Prompts/index.md#main-prompt-system-prompt)，可使用 \{\{original\}\} 占位符包含原始提示词
* **历史后指令**：覆盖默认的[历史后指令](/Usage/Prompts/index.md#post-history-instructions)

#### 创作者元数据

关于角色的非提示词信息：

- 创作者名称/联系方式
- 角色版本
- 创作者注释
- 嵌入标签列表

#### 角色个性

* **[个性概述](characterdesign.md#personality-summary)**：角色特征的简要概述
* **[场景](characterdesign.md#scenario)**：对话的背景与情境
* **角色注释**：可选择深度和消息角色的自定义消息（另见[作者注释](/Usage/Characters/Author's-Note.md)）
* **健谈程度**（群组聊天）：滑块，范围从害羞 → 正常 → 健谈
* **示例消息**：角色写作风格的示例

### 群组聊天管理

如果当前是群组聊天，可从此面板管理群组成员和设置。

详情请参见[群组聊天](/Usage/Characters/groupchats.md)。
