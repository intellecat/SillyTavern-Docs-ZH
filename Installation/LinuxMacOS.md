---
label: MacOS & Linux
order: 5
route: /installation/linuxmacos/
---

# Linux/MacOS 安装

## 手动 Git 安装

在 MacOS / Linux 上，以下所有操作均在终端中完成。

1. 安装 git 和 nodeJS（具体方法因操作系统而异）
2. 克隆仓库

   - 正式版分支：`git clone https://github.com/SillyTavern/SillyTavern -b release`
   - 预览版分支：`git clone https://github.com/SillyTavern/SillyTavern -b staging`

3. 执行 `cd SillyTavern` 进入安装目录。
4. 使用以下命令之一运行 `start.sh` 脚本：

- `./start.sh`
- `bash start.sh`

## SillyTavern 启动器

### 适用于 Linux 用户
1. 打开你喜欢的终端并安装 git
2. 使用以下命令下载 SillyTavern 启动器：`git clone https://github.com/SillyTavern/SillyTavern-Launcher.git`
3. 使用以下命令进入启动器目录：`cd SillyTavern-Launcher`
4. 使用以下命令启动安装程序：`chmod +x install.sh && ./install.sh`，然后选择要安装的内容
5. 安装完成后，使用以下命令启动启动器：`chmod +x launcher.sh && ./launcher.sh`

### 适用于 Mac 用户
1. 打开终端并使用以下命令安装 brew：`/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`
2. 使用以下命令安装 git：`brew install git`
3. 使用以下命令下载 SillyTavern 启动器：`git clone https://github.com/SillyTavern/SillyTavern-Launcher.git`
4. 使用以下命令进入启动器目录：`cd SillyTavern-Launcher`
5. 使用以下命令启动安装程序：`chmod +x install.sh && ./install.sh`，然后选择要安装的内容
6. 安装完成后，使用以下命令启动启动器：`chmod +x launcher.sh && ./launcher.sh`
