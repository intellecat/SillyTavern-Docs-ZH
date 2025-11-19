---
label: MacOS & Linux
order: 5
route: /installation/linuxmacos/
---

# Linux/MacOS 安装

## 手动 Git 安装

对于 MacOS / Linux，所有这些操作都将在 Terminal 中完成。

1. 安装 git 和 nodeJS（执行此操作的方法将因您的操作系统而异）
2. 克隆仓库

   - Release 分支：`git clone https://github.com/SillyTavern/SillyTavern -b release`
   - Staging 分支：`git clone https://github.com/SillyTavern/SillyTavern -b staging`

3. 执行 `cd SillyTavern` 导航到安装文件夹。
4. 使用以下命令之一运行 `start.sh` 脚本：

- `./start.sh`
- `bash start.sh`

## SillyTavern Launcher

### Linux 用户
1. 打开您喜欢的终端并安装 git
2. 使用以下命令下载 Sillytavern Launcher：`git clone https://github.com/SillyTavern/SillyTavern-Launcher.git`
3. 使用以下命令导航到 SillyTavern-Launcher：`cd SillyTavern-Launcher`
4. 使用以下命令启动安装启动器：`chmod +x install.sh && ./install.sh` 并选择您想要安装的内容
5. 安装后使用以下命令启动启动器：`chmod +x launcher.sh && ./launcher.sh`

### Mac 用户
1. 打开终端并使用以下命令安装 brew：`/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`
2. 然后使用以下命令安装 git：`brew install git`
3. 使用以下命令下载 Sillytavern Launcher：`git clone https://github.com/SillyTavern/SillyTavern-Launcher.git`
4. 使用以下命令导航到 SillyTavern-Launcher：`cd SillyTavern-Launcher`
5. 使用以下命令启动安装启动器：`chmod +x install.sh && ./install.sh` 并选择您想要安装的内容
6. 安装后使用以下命令启动启动器：`chmod +x launcher.sh && ./launcher.sh`
