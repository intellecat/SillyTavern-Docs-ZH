---
label: Android (Termux)
route: /installation/android-(termux)/
---

# Android (Termux) 安装

SillyTavern 可以使用 Termux 在 Android 设备上原生运行。

## 安装 Termux

!!!tip
避免从 Google Play Store 安装 Termux，该版本已不再维护。
建议使用 F-Droid（推荐）或 GitHub releases 获取最新版本。
!!!

1. 从 [F-Droid](https://f-droid.org/en/packages/com.termux/) 或 [GitHub releases](https://github.com/termux/termux-app/releases) 下载 Termux。
2. 安装下载的 APK 文件。
3. 打开 Termux 并运行您的第一个命令：

   ```bash
   termux-change-repo
   ```

4. 选择 "Mirror group" 并选择离您最近的服务器。您可以触摸屏幕或使用 [Unexpected Keyboard](https://play.google.com/store/apps/details?id=juloo.keyboard2&hl=en) 进行滑动手势操作。
5. 更新 Termux：

   ```bash
   pkg update && pkg upgrade
   ```

## 安装依赖项

安装所需的软件包：

```bash
pkg install git nodejs-lts nano
```

!!!warning
如果您运行的是 32-bit Android，请查看下面的[常见错误](#common-errors)部分以了解额外的步骤。
!!!

## 安装 SillyTavern

克隆 SillyTavern 仓库（[如何选择分支](/Installation/index.md#branches)）：

- **Release 分支：**

    ```bash
    git clone https://github.com/SillyTavern/SillyTavern -b release
    ```

- **Staging 分支：**

    ```bash
    git clone https://github.com/SillyTavern/SillyTavern -b staging
    ```

## 运行 SillyTavern

要运行 SillyTavern，导航到克隆的目录并运行启动脚本：

```bash
cd ~/SillyTavern
bash start.sh
```

要更新 SillyTavern，导航到 SillyTavern 目录并运行：

```bash
cd ~/SillyTavern
git pull --rebase --autostash
```

请参阅下面的[别名](#optional-create-aliases)部分以创建快捷方式来简化此过程。

## 常见错误

### Unsupported platform: android arm LEtime-web

32-bit Android 需要一个无法通过 npm 安装的外部依赖项。

使用以下命令安装它：

```bash
pkg install esbuild
```

然后继续执行上述安装步骤。

### 性能调优

!!!info
有关提高性能的一般提示，请参阅相应的 [FAQ 部分](/Usage/faq.md#performance-tips)。
!!!

由于 Android 设备的硬件限制，您可能需要调整以下 SillyTavern [config.yaml](/Administration/config-yaml.md) 设置以获得更好的内存、存储和 CPU 使用率：

```yaml
performance:
  # 避免加载所有角色数据直到需要时
  lazyLoadCharacters: true
  # 禁用磁盘缓存以减少存储使用
  useDiskCache: false
backups:
  chat:
    # 可选：禁用自动聊天备份以节省存储空间
    enabled: false
```

!!!tip
使用 Termux 附带的 `nano` 文本编辑器来编辑 `config.yaml` 文件：`nano ~/SillyTavern/config.yaml`
!!!

## 可选：创建别名

您可以为常用命令创建快捷方式，以便更轻松地工作。

1. 打开编辑器修改您的 `.bashrc` 文件：

   ```bash
   nano ~/.bashrc
   ```

2. 添加以下行来创建别名：

   ```bash
   # 更新 Termux 软件包
   alias pkgup="pkg update && pkg upgrade"
   # 启动 SillyTavern
   alias st='cd ~/SillyTavern && bash start.sh'
   # 更新 SillyTavern
   alias stup='cd ~/SillyTavern && git pull --rebase --autostash'
   ```

3. 保存文件并退出编辑器（在 nano 中，按 `CTRL + X`，然后按 `Y`，然后按 `Enter`）。

4. 要应用更改，运行：

   ```bash
   source ~/.bashrc
   ```

现在您可以使用以下命令：

- `st` 启动 SillyTavern
- `stup` 更新 SillyTavern
- `pkgup` 更新 Termux 软件包

## 延伸阅读

!!!info
以下链接的指南不由 SillyTavern 团队维护。
!!!

- ArroganceComplex#2659 的 SillyTavern in Termux 指南：<https://rentry.org/STAI-Termux>
- 使用 Material Files 访问 Termux 文件：<https://www.learntermux.tech/2020/10/Termux-File-Manager.html>
- 防止 Termux 进程深度睡眠：<https://wiki.termux.com/wiki/Termux-wake-lock>
