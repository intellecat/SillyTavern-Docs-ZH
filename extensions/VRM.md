# VRM

本指南将指导您为 SillyTavern 设置和自定义 VRM 扩展。此扩展允许您为角色使用 VRM 动画模型，为您的虚拟角色添加动态和互动元素。

## 前提条件

在开始之前，请确保满足以下前提条件：

1. **分支选择**：确保您使用的是最新版本的 SillyTavern，以访问最新功能和更新。

2. **扩展安装**：从扩展面板（堆叠块图标）的"Download Extensions & Assets"菜单中安装"VRM"扩展。

3. **模型文件夹放置**：将您的 VRM 模型文件（.vrm）放入 `/data/<user-handle>/assets/vrm/model` 目录，将动画文件放入 `/data/<user-handle>/assets/vrm/animation` 目录。目前支持的动画文件格式是与 VRM 模型兼容的 .fbx 和 .bvh。这包括您可以从 Mixamo (https://www.mixamo.com/) 获取的任何动画，以及您可以从 XR Animator (https://github.com/ButzYung/SystemAnimatorOnline) 等工具导出的任何动画。

## 扩展设置

VRM 扩展提供了各种设置来自定义您的动画模型的行为。以下是主要设置：

![UI 全局设置](/static/extensions/vrm-global.png)

### 全局设置

1. **启用**：
   - 选中此复选框以激活扩展，允许您的 VRM 模型在 SillyTavern 中互动。
   - 如果您只想使用普通精灵图，可以禁用此扩展。

2. **看向摄像机**：
   - 选中此复选框可使 VRM 模型的眼睛看向摄像机。

3. **眨眼**：
   - 选中此复选框可使 VRM 模型的眼睛在随机时间间隔眨眼。模型表情应该正确定义眨眼权重属性，否则模型可能会出现闭着眼睛眨眼等情况，如果发生这种情况，您可以：
    - 如果您有 .vroid 文件，请修正模型
    - 不使用那个不正确的面部表情
    - 使用此复选框完全禁用眨眼

4. **TTS 口型同步**：
    - 选中此复选框可使 VRM 嘴部动作跟随 TTS 播放的声音。仅适用于由 Sillytavern 本身播放声音的 TTS，如 XTTS（非流式模式）。如果禁用，当收到新的角色消息时，嘴部将根据消息文本长度进行动画。

5. **自动发送互动**：
   - 选中此复选框可在您点击带有映射消息的区域时自动触发角色互动（详见点击区域部分）。

### 性能设置

1. **身体碰撞箱**：
    - 选中此复选框可激活对 VRM 模型多个部位的点击检测，根据模型可以检测以下区域：头部/胸部/手部/腹股沟/臀部/腿部/脚部。碰撞箱位置在每帧计算并跟随身体动画，禁用此选项可以提高性能。

2. **使用模型缓存**：
    - 选中此复选框可在切换模型时将 VRM 模型保留在内存中，允许更快地切换回之前的模型。如果您为同一角色使用不同的模型来更换装扮或形态，这很有用。可能会影响性能。

3. **使用动画缓存**：
    - 选中此复选框可将会话期间播放的所有动画保留在内存中。所有分配给模型的动画也会在模型首次出现时加载。这会增加首次加载模型的时间，但使所有动画切换变得即时。可能会影响性能。

### 调试设置

1. **显示网格**：
    - 选中此复选框可以可视化 3D 网格、模型拖动框和身体碰撞箱。

2. **重新加载按钮**：
    - 点击此按钮可重新加载 3D 场景，清除缓存和所有 VRM 模型。如果出现错误或缓存开始影响性能时使用。

### 场景设置

![UI 场景设置](/static/extensions/vrm-scene.png)

1. **灯光颜色**：
    - 设置 3D 场景中的灯光颜色。点击重置按钮将其恢复为默认的白色。根据您的浏览器，您可以使用颜色选择器，例如，您可以选取背景图片的颜色以增加沉浸感。

2. **灯光强度**：
    - 使用滑块以百分比设置灯光强度。点击重置按钮将其恢复为默认值 100%。VRM 模型对光线的反应可能因模型中烘焙的着色器而异，请尝试不同的值看看效果如何。

![UI 模型设置](/static/extensions/vrm-model.png)

## 角色选择

这些设置允许您管理角色并为其分配 VRM 模型。

1. **刷新按钮**：
   - 点击刷新按钮以更新当前聊天中的角色列表。

2. **选择角色**：
   - 使用下拉列表选择要分配 VRM 模型的角色。

3. **移除按钮**：
   - 点击此按钮可删除角色的已分配模型。

## 模型选择

1. **刷新按钮**：
   - 如果您的 VRM 模型未出现在列表中，请点击刷新按钮。

2. **选择模型**：
   - 从列表中选择一个模型以分配给所选角色。
   - 模型必须位于 `/data/<user-handle>/assets/vrm/model` 目录中。

3. **重置按钮**：
    - 点击此按钮可将模型设置重置为默认值。如果您有与默认值对应的动画文件，它们将自动映射。请参阅本文档末尾的命名映射。

## 模型设置

1. **模型缩放**：
   - 使用滑块调整模型的大小，使其变大或变小。

2. **模型中心 X/Y 偏移**：
   - 使用这些滑块更改模型相对于窗口中心的水平/垂直位置。

3. **模型 X/Y 旋转**：
    - 使用这些滑块更改模型相对于模型臀部的水平/垂直旋转。

### 备注
    - 设置是按模型而不是按角色保存的，并在不同聊天中保持。
    - 如果您想为两个不同的角色使用相同的模型但使用不同的设置，请复制 .vrm 文件。
    - 您也可以用鼠标拖动模型，这些设置会更新并保存。左键单击并按住可以在屏幕上拖动模型。中键单击并按住或使用 shift+左键单击可以旋转模型。将鼠标光标放在模型上使用鼠标滚轮或使用 ctrl+左键单击可以放大或缩小。
    - 如果您不小心将模型移出视图，请使用这些 UI 设置将其带回屏幕。另外，选中"显示框架"复选框以清楚地看到可以点击拖动模型的位置。

![UI 碰撞箱设置](/static/extensions/vrm-hitboxes.png)

## 碰撞箱映射

    - 根据模型骨骼定义，可以生成一些碰撞箱区域，它们将在 UI 的这部分列出，您可以为每个区域分配表情/动画/消息，当您点击该区域时会触发。

![UI 分类设置](/static/extensions/vrm-classify.png)

## 分类表情映射

1. **要求**：
    - 需要使用分类表情扩展；否则，将回退到默认动画。

2. **映射**：
    - 对于分类扩展检测到的每种情绪，您可以分配表情/动作/消息。消息可以包含命令。

## 命令

1. **/vrmlightcolor**
    - 设置灯光颜色
    - 参数：color
    - 示例："/vrmlightcolor white" 或 "/vrmlightcolor purple"
2. **/vrmlightintensity**
    - 以百分比设置灯光强度
    - 参数：intensity
    - 示例："/vrmlightintensity 0" 或 "/vrmlightintensity 100"
3. **/vrmmodel**
    - 为角色分配 vrm 模型
    - 参数：character, model
    - 示例：在单人聊天中 "/vrmmodel Seraphina.vrm" 或在群组聊天中 "/vrmmodel character=Seraphina model=Seraphina.vrm"
4. **/vrmexpression**
    - 更改模型的表情
    - 参数：character, expression
    - 示例：在单人聊天中 "/vrmexpression happy" 或在群组聊天中 "/vrmexpression character=Seraphina expression=happy"

5. **/vrmmotion**
    - 更改模型的动画
    - 参数：character, motion, loop, random
    - "/vrmmotion idle" 或 "/vrmmotion character=Seraphina motion=idle loop=true random=false"

## 动画默认映射
如果您的动画文件按以下方式命名，它们将在重置模型设置时自动映射。例如，名为 "assets/vrm/animation/neutral.bvh" 和 "assets/vrm/animation/neutral1.fbx" 的文件将自动映射为默认和中性分类动画的组。碰撞箱也是如此。

    // 回退
    "default": "assets/vrm/animation/neutral",

    // 分类类别
    "admiration": "assets/vrm/animation/admiration",
    "amusement": "assets/vrm/animation/amusement",
    "anger": "assets/vrm/animation/anger",
    "annoyance": "assets/vrm/animation/annoyance",
    "approval": "assets/vrm/animation/approval",
    "caring": "assets/vrm/animation/caring",
    "confusion": "assets/vrm/animation/confusion",
    "curiosity": "assets/vrm/animation/curiosity",
    "desire": "assets/vrm/animation/desire",
    "disappointment": "assets/vrm/animation/disappointment",
    "disapproval": "assets/vrm/animation/disapproval",
    "disgust": "assets/vrm/animation/disgust",
    "embarrassment": "assets/vrm/animation/embarrassment",
    "excitement": "assets/vrm/animation/excitement",
    "fear": "assets/vrm/animation/fear",
    "gratitude": "assets/vrm/animation/gratitude",
    "grief": "assets/vrm/animation/grief",
    "joy": "assets/vrm/animation/joy",
    "love": "assets/vrm/animation/love",
    "nervousness": "assets/vrm/animation/nervousness",
    "neutral": "assets/vrm/animation/neutral",
    "optimism": "assets/vrm/animation/optimism",
    "pride": "assets/vrm/animation/pride",
    "realization": "assets/vrm/animation/realization",
    "relief": "assets/vrm/animation/relief",
    "remorse": "assets/vrm/animation/remorse",
    "sadness": "assets/vrm/animation/sadness",
    "surprise": "assets/vrm/animation/surprise",

    // 碰撞箱
    "head": "assets/vrm/animation/hitarea_head",
    "chest": "assets/vrm/animation/hitarea_chest",
    "groin": "assets/vrm/animation/hitarea_groin",
    "butt": "assets/vrm/animation/hitarea_butt",
    "leftHand": "assets/vrm/animation/hitarea_hands",
    "rightHand": "assets/vrm/animation/hitarea_hands",
    "leftLeg": "assets/vrm/animation/hitarea_leg",
    "rightLeg": "assets/vrm/animation/hitarea_leg",
    "rightFoot": "assets/vrm/animation/hitarea_foot",
    "leftFoot": "assets/vrm/animation/hitarea_foot"

感谢您阅读本指南！您的 SillyTavern 体验现在已通过动画和互动的 3D 模型得到增强。

## 备注
    - 此扩展加载的是 .vrm 文件而不是 .vroid 文件。
    - 动画文件应该与 VRM 兼容，您可以使用像 XR animation (https://github.com/ButzYung/SystemAnimatorOnline) 这样的工具来转换 fbx/bvh 动画文件。
    - 您可以通过使用以不同数字结尾的相同名称文件来创建动画组，例如："idle1.bvh"、"idle2.bhv"、"idle3.bvh" 将被视为一个"idle"组，当在映射中选择时，触发时将随机播放其中一个，可用于增加动画的多样性。
    - 您可以从此仓库获取精选动画：https://github.com/test157t/VRM-Animations-Pack-For-Silly-Tavern
    - Nitral 有一些关于如何使用扩展和动画仓库的教程视频：https://www.youtube.com/@nitralai
