---
order: 100
icon: person-fill
route: /usage/characters/
---

# 角色

Characters 是您可以创建和管理的 AI 身份，用于塑造 AI 在对话中的角色。每个 character 都有名字、性格和对话历史。您可以创建任意数量的 characters，并随时在它们之间切换。

Characters 可用于单独聊天，或将多个 characters 添加到 group chat 中让它们相互交互。

## Character 管理面板

从导航栏打开 <i class="fa-solid fa-address-card"></i> **Characters** 面板以访问 character 列表。点击 character 或 group 与其聊天或编辑，或选择 <i class="fa-solid fa-user-plus"></i> **Create New Character** 以添加新 character。

### 面板控制

* <i class="fa-solid fa-lock"></i> **Pin Panel**：在交互时保持面板打开
* <i class="fa-solid fa-list-ul"></i> **Character List**：返回 character 列表视图
* **HotSwap Bar**：快速访问收藏的 characters

### Character 列表

* <i class="fa-solid fa-user-plus"></i> **Create New Character**：添加新 character
* <i class="fa-solid fa-file-import"></i> **Import Character**：从文件加载 character
* <i class="fa-solid fa-cloud-arrow-down"></i> **External Import**：从 URL 导入
* <i class="fa-solid fa-users-gear"></i> **Create Group**：开始新的 group chat

#### 搜索和排序

* **Search Bar**：按名称或属性筛选 characters
* **Sort Dropdown**：多种排序选项：
    - 字母顺序（A-Z，Z-A）
    - 时间顺序（最新，最旧）
    - 使用频率（最近，聊天最多/最少）
    - 大小（tokens 最多/最少）
    - 特殊（收藏，随机）

#### 按类型或 tag 筛选 characters

* <i class="fa-solid fa-star"></i> **Favorites Filter**：显示收藏的 characters
* <i class="fa-solid fa-users"></i> **Groups Filter**：仅显示 group chats
* <i class="fa-solid fa-folder-plus"></i> **Tags as Folders**：按 tag 层次结构组织
* <i class="fa-solid fa-gear"></i> **Manage Tags**：[Tag 配置](/Usage/Characters/Tags.md)
* <i class="fa-solid fa-tags"></i> **Tag List**：查看所有可用 tags
* <i class="fa-solid fa-filter-circle-xmark"></i> **Clear Filters**：重置所有筛选器

### Character 创建/编辑面板

* **Avatar Image**：上传和预览 character 头像
* **Token Count**：character 的 [Token 使用量](characterdesign.md#character-tokens)
* <i class="fa-solid fa-ranking-star"></i> **Stats**：聊天历史和使用统计
* [Tag 管理](/Usage/Characters/Tags.md)

#### 快速操作

- <i class="fa-solid fa-star"></i> 收藏开关
- <i class="fa-solid fa-book"></i> Advanced definitions
- <i class="fa-solid fa-globe"></i> Character lore
- <i class="fa-solid fa-passport"></i> Chat lore：将聊天链接到 [World Info](/Usage/worldinfo.md)
- <i class="fa-solid fa-file-export"></i> 导出 character
- <i class="fa-solid fa-clone"></i> 复制
- <i class="fa-solid fa-skull"></i> 删除

#### 扩展选项

* World Info 链接
* Card lore 导入
* Scenario 覆盖
* Persona 转换
* Character 重命名
* 来源链接
* 替换/更新
* Tag 导入
* 图库视图

#### 内容字段

* **[Character Description](characterdesign.md#character-description)**：简短的 character 摘要
* **[First Message](characterdesign.md#first-message)**：开始新聊天时的初始问候或提示
* **Alternative greetings**：定义多个 first messages，您可以在开始聊天时在它们之间切换

### Advanced Definitions 面板

点击 <i class="fa-solid fa-book"></i> **Advanced Definitions** 按钮以访问扩展的 character 设置。

#### Prompt 覆盖（Chat Completion/Instruct Mode）

* **Main Prompt**：替换默认的 [main/system prompt](/Usage/Prompts/index.md#main-prompt-system-prompt)，可以使用 \{\{original\}\} 占位符来包含原始 prompt
* **Post-History Instructions**：覆盖默认的 [post-history instructions](/Usage/Prompts/index.md#post-history-instructions)

#### 创作者元数据

关于 character 的非 prompt 信息：

- 创作者姓名/联系方式
- Character 版本
- 创作者备注
- 嵌入的 tags 列表

#### Character 性格

* **[Personality Summary](characterdesign.md#personality-summary)**：character 特征的简要概述
* **[Scenario](characterdesign.md#scenario)**：对话的背景和环境
* **Character's Note**：具有可选深度和消息角色的自定义消息（另见 [Author's Note](/Usage/Characters/Author's-Note.md)）
* **Talkativeness**（Group Chats）：滑块，从害羞 → 正常 → 健谈
* **Example Messages**：character 写作风格的示例

### Group Chat 管理

如果这是 group chat，您可以从此面板管理 group 成员和设置。

详见 [Group Chats](/Usage/Characters/groupchats.md)。
