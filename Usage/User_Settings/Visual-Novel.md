---
order: 10
tags:
    [
        visual novel,
        vn,
    ]
route: /usage/user-settings/visual-novel/
---

# Visual Novel (VN) 模式

Visual Novel 模式是 SillyTavern 中的一种特殊屏幕布局,允许你与带有精灵图(或其角色卡片图像)的角色聊天,类似于《心跳文学部》、《灰色的果实》、《Fate: Stay/night》等著名 VN 游戏的风格。

## 切换 Visual Novel 模式

### 启用 Visual Novel 模式

Visual Novel 模式是 SillyTavern 的内置功能,可以通过进入 *User Settings*(User Settings 图标)并在 *No Text Shadows* 下方勾选 **Visual Novel Mode** 来切换。

![User Settings](/static/vn/vn-mode-toggle.png)

### 禁用 Visual Novel 模式

禁用 Visual Novel 模式的步骤与启用相同。取消勾选 Visual Novel Mode,你就会回到正常的聊天界面。

!!!warning 关于带有 VN Extensions 的 VN Mode
某些扩展(如 Prome VN Extension)在使用其各自的 VN 模式时会自动开启'Visual Novel Mode'。从 *User Settings* 菜单启用/禁用 VN Mode 也会影响这些扩展。
!!!

## Visual Novel UI

![VN Display](/static/vn/vn-display.png)

在 Visual Novel 模式下,UI 略有改变以适应显示在中央的角色精灵图(或角色卡片图像)。然而,在多角色群聊中,角色精灵图会相互分散开来,如下图所示。

![Group VN Display](/static/vn/group-vn-display.png)

### VN Mode 与 MovingUI

!!!info
要切换 MovingUI,请进入 *User Settings* 并勾选 **MovingUI**。请注意,此功能**仅**适用于桌面设备。
!!!

如果在 *User Settings* 中启用了 **MovingUI**,可以移动精灵图(或角色卡片图像)到屏幕上你希望放置它们的位置或更特定的区域。

!!!warning 关于 Sprite 大小
如果你的角色精灵图相对较大,使用 MovingUI 移动某些精灵图可能会比较困难,因为拖动精灵图的按钮可能被现有精灵图覆盖。你可能需要多次移动它们,特别是当屏幕上有更多角色时,以获得更好的布局。
!!!

![Group VN Display (MovingUI)](/static/vn/vn-group-display-movingui.png)

## 获取角色 Sprites

获取角色精灵图可以通过浏览网络寻找现有精灵图,比如来自 Visual Novel 或使用 Visual Novel 功能的游戏(如 DDLC 或 CounterSide)中的现有角色。如果你想要的角色还没有精灵图,你还有以下几个选择。

1. 在角色帖子中搜索 sprite ZIP 包或精灵图包链接。
    !!!info
    一些机器人创建者可能会在发布机器人时附带精灵图包(可能在同一帖子中或在 sprites 频道中)。搜索这些帖子,看看是否有人已经制作了你想要的角色的精灵图。
    !!!
2. 使用 LoRAs 和 Stable Diffusion 创建自己的精灵图。
    !!!warning
    从头开始生成精灵图非常耗时(特别是如果你的角色没有 LoRAs 和/或你想使用的 Stable Diffusion 模型不存在),并且需要不错的硬件配置,如果你计划制作 28 个表情精灵图而不是 6 个,以及如果你使用 SDXL 和/或将精灵图提升到更高分辨率,这一点尤为重要。
    !!!
3. 使用角色卡片图像。虽然它可能不像精灵图,但至少你有东西可以在屏幕上看。但是,VN 模式下不能使用多个角色卡片。
    !!! 使用 Prome Visual Novel Extension 的角色卡片图像
    在 Prome Visual Novel Extension 1.0.6+ 版本中,有一个名为 `Emulate Character Card as Sprite` 的功能,允许你通过将角色卡片用作聊天中的精灵图来进行同时包含精灵图和非精灵图角色的群组聊天。

    ![Character Card Group Chat](/static/vn/extensions/prome/card-emulation.png)
    !!!

## VN Extensions

### Prome Visual Novel Extension

Prome Visual Novel Extension 是由 Bronya Rand 和 Prometheus 开发的认可第三方扩展,它进一步增强了 SillyTavern 中的 Visual Novel 体验,具有诸如 Letterbox Mode(使 Visual Novel UI 更"电影化")、带有 Darken Character Sprites 的 Focus Mode、Traditional VN Mode(聊天中只显示最后一条消息)等功能,并计划添加更多功能!

|                              Letterbox Mode                              |                          Traditional VN Mode                           |
|:------------------------------------------------------------------------:|:----------------------------------------------------------------------:|
| ![Horizontal Letterbox Mode](/static/vn/extensions/prome/horizontal.png) | ![Traditional VN Mode](/static/vn/extensions/prome/single-message.png) |

|                 Hide Sheld (Message Box)                  |                      Focus Mode (w/ Darken Sprites)                      |
|:---------------------------------------------------------:|:------------------------------------------------------------------------:|
| ![Sheld Hide](/static/vn/extensions/prome/sheld_hide.png) | ![Focus Mode w/ Darken Sprites](/static/vn/extensions/prome/defocus.png) |

要安装 Prome Visual Novel Extension,你可以通过进入 `Download Extensions & Assets` 并找到 *Prome Visual Novel Extension* 来安装,或按照 [Prome Visual Novel Extension](https://github.com/Bronya-Rand/Prome-VN-Extension?tab=readme-ov-file#installation-and-usage) Github 页面上的安装说明进行操作。调整 Prome 的设置可以在 *Extensions* -> **Prome (Visual Novel Extension)** 中找到,或通过 🪄 (Wand) 菜单访问。
