---
route: /extensions/vrm/
---

# VRM

本指南将引导您完成 SillyTavern 的 VRM 扩展的设置和自定义过程。此扩展允许您为角色使用 VRM 动画模型,为虚拟角色提供动态和交互式元素。

## 前置条件

开始之前,请确保您已满足以下前置条件:

1. **分支选择**:确保您使用的是最新版本的 SillyTavern,以访问最新功能和更新。

2. **扩展安装**:从扩展面板(由堆叠方块图标表示)中的"下载扩展和资源"菜单安装"VRM"扩展。

3. **模型文件夹放置**:将您的 VRM 模型文件(.vrm)放入 `/data/<user-handle>/assets/vrm/model` 目录,将动画文件放入 `/data/<user-handle>/assets/vrm/animation` 目录。当前支持的动画文件格式是与 VRM 模型兼容的 .fbx 和 .bvh。这包括您可以从 Mixamo (https://www.mixamo.com/) 获得的任何动画以及您可以从 XR Animator (https://github.com/ButzYung/SystemAnimatorOnline) 等工具导出的任何动画。

## 扩展设置

VRM 扩展提供各种设置来自定义动画模型的行为。以下是关键设置:

![UI 全局设置](/static/extensions/vrm-global.png)

### 全局设置

1. **启用**:
   - 启用此复选框以激活扩展,允许您的 VRM 模型在 SillyTavern 中交互。
   - 如果您只想使用普通精灵图,可以禁用扩展。

2. **看向相机**:
   - 启用此复选框可使 VRM 模型的眼睛看向相机。

3. **眨眼**:
   - 启用此复选框可使 VRM 模型的眼睛随机间隔眨眼。模型表情应正确定义眨眼权重属性,否则模型可能会闭着眼睛眨眼,如果发生这种情况:
    - 如果您有 .vroid 文件,请更正模型
    - 不要使用该不正确的面部表情
    - 使用此复选框完全禁用眨眼

4. **TTS 唇同步**
    - 启用此复选框可使 VRM 嘴巴动作在播放 TTS 时跟随声音。仅适用于 SillyTavern 本身播放声音的 TTS,如 XTTS(非流式模式)。如果禁用,收到新角色消息时,嘴巴将根据消息文本长度进行动画处理。

5. **自动发送交互**:
   - 启用此复选框可在您单击带有映射消息的区域时自动触发角色交互(有关详细信息,请参阅命中区域部分)。

### 性能设置

1. **身体碰撞箱**
    - 启用此复选框可激活对 VRM 模型多个部位的点击检测,根据模型可以检测到以下区域:头部/胸部/手/腹股沟/臀部/腿/脚。碰撞箱位置在每帧计算并跟随身体动画,禁用此选项可以提高性能。

2. **使用模型缓存**
    - 启用此复选框可在切换模型时将 VRM 模型保留在内存中,允许更快地切换回以前的模型。如果您为同一角色使用不同的模型来更改服装或形态,这很有用。可能会影响性能。

3. **使用动画缓存**
    - 启用此复选框可在内存中保留会话期间播放的所有动画。分配给模型的所有动画也将在模型首次出现时加载。将增加首次加载模型的时间,但使所有动画切换即时。可能会影响性能。

### 调试设置

1. **显示网格**
    - 启用此复选框可可视化 3d 网格、模型拖动框和身体碰撞箱。

2. **重新加载按钮**
    - 单击此按钮可重新加载 3d 场景、清除缓存和所有 VRM 模型。如果发生某些错误或缓存开始影响性能时使用它。

### 场景设置

![UI 场景设置](/static/extensions/vrm-scene.png)

1. **光照颜色**
    - 设置 3d 场景中光照的颜色。单击重置按钮将其设置回默认的白色。根据您的浏览器,您可以使用颜色选择器,例如,您可以选择背景图像的颜色以增加沉浸感。

2. **光照强度**
    - 使用滑块以百分比设置光照强度。单击重置按钮将其设置回默认值 100%。VRM 模型可能会根据模型中烘焙的着色器对光照产生不同的反应,尝试不同的值看看效果如何。

![UI 模型设置](/static/extensions/vrm-model.png)

## 角色选择

这些设置允许您管理角色并为其分配 VRM 模型。

1. **刷新按钮**:
   - 单击刷新按钮可更新当前聊天中的角色列表。

2. **选择角色**:
   - 使用下拉列表选择要分配 VRM 模型的角色。

3. **删除按钮**:
   - 单击此按钮可删除角色的分配模型。

## 模型选择

1. **刷新按钮**:
   - 如果您的 VRM 模型未出现在列表中,请单击刷新按钮。

2. **选择模型**:
   - 从列表中选择一个模型以将其分配给所选角色。
   - 模型必须位于 `/data/<user-handle>/assets/vrm/model` 目录中。

3. **重置按钮**
    - 单击此按钮可将模型设置重置为默认值。如果您有与默认值对应的动画文件,它们将自动映射。请参阅本 README 末尾的命名映射。

## 模型设置

1. **模型缩放**:
   - 使用滑块调整模型的大小,使其变大或变小。

2. **模型中心 X/Y 偏移**:
   - 使用这些滑块更改模型相对于窗口中心的水平/垂直位置。

3. **模型 X/Y 旋转**
    - 使用这些滑块更改模型相对于模型臀部的水平/垂直旋转。

### 备注
    - 设置按模型而非按角色保存,并在不同的聊天中保留。
    - 如果您想为两个不同的角色使用具有不同设置的相同模型,请复制 .vrm 文件。
    - 您也可以用鼠标拖动模型,这些设置将被更新并保存。左键单击并按住以在屏幕上拖动模型。中键单击并按住以旋转模型或使用 shift+左键单击。使用鼠标滚轮将光标放在模型上以放大或缩小,或使用 ctrl+左键单击。
    - 如果您以某种方式使模型移出视图,请使用这些 UI 设置将模型带回屏幕。此外,勾选"显示框架"复选框可以清楚地看到您可以在哪里单击以拖动模型。

![UI 碰撞箱设置](/static/extensions/vrm-hitboxes.png)

## 碰撞箱映射

    - 根据模型骨骼定义,可以生成一些碰撞箱区域,它们将在 UI 的这部分列出,您可以为每个区域分配表情/动画/消息,单击该区域时将触发。

![UI 分类设置](/static/extensions/vrm-classify.png)

## 分类表情映射

1. **要求**
    - 需要使用分类表情扩展;否则,它将回退到默认动画。

2. **映射**
    - 对于分类扩展检测到的每种情绪,您可以分配表情/动作/消息。消息可以包含命令。

## 命令

1. **/vrmlightcolor**
    - 设置光照颜色
    - 参数:颜色
    - 示例:"/vrmlightcolor white" 或 "/vrmlightcolor purple"。
2. **/vrmlightintensity**
    - 设置光照强度百分比
    - 参数:强度
    - 示例:"/vrmlightintensity 0" 或 "/vrmlightintensity 100"
3. **/vrmmodel**
    - 为角色分配 vrm 模型
    - 参数:角色,模型
    - 示例:在单独聊天中使用 "/vrmmodel Seraphina.vrm",在群聊中使用 "/vrmmodel character=Seraphina model=Seraphina.vrm"
4. **/vrmexpression**
    - 更改模型的表情
    - 参数:角色,表情
    - 示例:在单独聊天中使用 "/vrmexpression happy",在群聊中使用 "/vrmexpression character=Seraphina expression=happy"

5. **/vrmmotion**
    - 更改模型的动画
    - 参数:角色,动作,循环,随机
    - "/vrmmotion idle" 或 "/vrmmotion character=Seraphina motion=idle loop=true random=false"

## 动画默认映射
如果您的动画文件按以下方式命名,它们将在重置模型设置时自动映射。例如,名为 "assets/vrm/animation/neutral.bvh" 和 "assets/vrm/animation/neutral1.fbx" 的文件将自动映射为默认和中性分类动画的组。碰撞箱也是如此。

    // 后备
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

感谢您遵循本指南!您的 SillyTavern 体验现在通过动画和交互式 3D 模型得到了丰富。

## 备注
    - 此扩展加载的 VRM 模型是 .vrm 文件,而不是 .vroid 文件。
    - 动画文件应与 VRM 兼容,您可以使用 XR animation (https://github.com/ButzYung/SystemAnimatorOnline) 等工具转换 fbx/bvh 动画文件。
    - 您可以通过使用以不同数字结尾的相同名称的文件来创建动画组,例如:"idle1.bvh"、"idle2.bhv"、"idle3.bvh" 将被视为一个组 "idle",在映射中选择时,触发时将播放随机一个,可用于为动画添加多样性。
    - 您可以从此存储库获取精选动画:https://github.com/test157t/VRM-Animations-Pack-For-Silly-Tavern
    - Nitral 有一些关于如何使用扩展和动画存储库的教程视频:https://www.youtube.com/@nitralai