---
order: 10
label: Windows
route: /installation/windows/
---
# Windows 安装

!!!warning
不要安装到任何 Windows 控制的文件夹（Program Files、System32 等）。

不要使用管理员权限运行 START.BAT

Windows 7 无法安装，因为它不能运行 NodeJS 18.16
!!!

## 通过 Git 安装

1. 安装 [NodeJS](https://nodejs.org/en)（推荐使用最新的 LTS 版本）
2. 安装 [Git for Windows](https://gitforwindows.org/)
3. 打开 Windows 资源管理器（`Win+E`）
4. 浏览到或创建一个不受 Windows 控制或监控的文件夹。（例如：C:\MySpecialFolder\）
5. 在该文件夹内打开命令提示符，方法是单击顶部的"地址栏"，输入 `cmd`，然后按 Enter。
6. 当黑色窗口（命令提示符）弹出后，在其中输入以下命令之一并按 Enter：

   - Release 分支：`git clone https://github.com/SillyTavern/SillyTavern -b release`
   - Staging 分支：`git clone https://github.com/SillyTavern/SillyTavern -b staging`

7. 克隆完成后，双击 `Start.bat` 让 NodeJS 安装其依赖项。
8. 然后服务器将启动，SillyTavern 将在您的浏览器中弹出。

## 通过 SillyTavern Launcher 安装

1.  在键盘上：按 **`WINDOWS + R`** 打开运行对话框。然后，运行以下命令安装 git：
    ```shell
    cmd /c winget install -e --id Git.Git
    ```
2. 在键盘上：按 **`WINDOWS + E`** 打开文件资源管理器，然后导航到要安装启动器的文件夹。进入所需文件夹后，在地址栏中输入 `cmd` 并按 Enter。然后，运行以下命令：
   ```shell
    git clone https://github.com/SillyTavern/SillyTavern-Launcher.git && cd SillyTavern-Launcher && start installer.bat
    ```

## 通过 GitHub Desktop 安装
（这仅允许在 GitHub Desktop 中使用 git，如果您还想在命令行中使用 `git`，还需要安装 [Git for Windows](https://gitforwindows.org/)）

1. 安装 [NodeJS](https://nodejs.org/en)（推荐使用最新的 LTS 版本）
2. 安装 [GitHub Desktop](https://central.github.com/deployments/desktop/desktop/latest/win32)
3. 安装 GitHub Desktop 后，点击 `Clone a repository from the internet....`（注意：您**不需要**为此步骤创建 GitHub 账户）

    ![image](/static/windows-1.png)

4. 在菜单中，点击 URL 选项卡，输入此 URL `https://github.com/SillyTavern/SillyTavern`，然后点击 Clone。您可以更改本地路径以更改 SillyTavern 的下载位置。

    ![image](/static/windows-2.png)

5. 要打开 SillyTavern，使用 Windows 资源管理器浏览到克隆仓库的文件夹。默认情况下，仓库将被克隆到这里：`C:\Users\[您的 Windows 用户名]\Documents\GitHub\SillyTavern`

6. 双击 `start.bat` 文件。（注意：文件名的 `.bat` 部分可能被您的操作系统隐藏，在这种情况下，它看起来像一个名为 "`Start`" 的文件。这就是您双击运行 SillyTavern 的文件）

    ![image](/static/windows-3.png)

7. 双击后，将打开一个大的黑色命令控制台窗口，SillyTavern 将开始安装运行所需的组件。

8. 安装过程结束后，如果一切正常，命令控制台窗口应该如下所示，并且 SillyTavern 选项卡应该在您的浏览器中打开：

    ![image](/static/windows-4.png)

9. 连接到任何[支持的 API](/Usage/API_Connections/index.md) 并开始聊天！
