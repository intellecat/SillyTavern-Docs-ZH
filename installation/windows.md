# Windows 安装

!!!warning
请勿安装到任何 Windows 受控文件夹（Program Files、System32 等）。

请勿以管理员权限运行 START.BAT

Windows 7 上无法安装，因为该系统无法运行 NodeJS 20
!!!

## 通过 Git 安装

1. 安装 [NodeJS](https://nodejs.org/en)（推荐使用最新 LTS 版本）
2. 安装 [Git for Windows](https://gitforwindows.org/)
3. 打开 Windows 资源管理器（`Win+E`）
4. 浏览到或创建一个不受 Windows 管控或监视的文件夹（例如：C:\MySpecialFolder\）
5. 在该文件夹内打开命令提示符：点击顶部地址栏，输入 `cmd`，然后按 Enter。
6. 在弹出的黑色窗口（命令提示符）中，输入以下**其中一条**命令并按 Enter：

   - 正式版分支：`git clone https://github.com/SillyTavern/SillyTavern -b release`
   - 预览版分支：`git clone https://github.com/SillyTavern/SillyTavern -b staging`

7. 克隆完成后，双击 `Start.bat`，让 NodeJS 安装所需依赖。
8. 服务器启动后，SillyTavern 将在浏览器中自动弹出。

## 通过 SillyTavern 启动器安装

1. 在键盘上按 **`WINDOWS + R`** 打开运行对话框，然后执行以下命令安装 git：
    ```shell
    cmd /c winget install -e --id Git.Git
    ```
2. 在键盘上按 **`WINDOWS + E`** 打开文件资源管理器，导航到你想安装启动器的文件夹。到达目标文件夹后，在地址栏输入 `cmd` 并按 Enter，然后执行以下命令：
   ```shell
    git clone https://github.com/SillyTavern/SillyTavern-Launcher.git && cd SillyTavern-Launcher && start installer.bat
    ```

## 通过 GitHub Desktop 安装
（此方式**仅**支持在 GitHub Desktop 中使用 git；如需在命令行中也使用 `git`，还需额外安装 [Git for Windows](https://gitforwindows.org/)）

1. 安装 [NodeJS](https://nodejs.org/en)（推荐使用最新 LTS 版本）
2. 安装 [GitHub Desktop](https://central.github.com/deployments/desktop/desktop/latest/win32)
3. 安装 GitHub Desktop 后，点击 `Clone a repository from the internet....`（注意：**无需**创建 GitHub 账户即可执行此步骤）
  
    ![image](/static/windows-1.png)

4. 在菜单中点击 URL 选项卡，输入 `https://github.com/SillyTavern/SillyTavern`，然后点击 Clone。你可以修改本地路径以更改 SillyTavern 的下载位置。

    ![image](/static/windows-2.png)

5. 打开 Windows 资源管理器，浏览到你克隆仓库的文件夹。默认情况下，仓库将被克隆到：`C:\Users\[你的 Windows 用户名]\Documents\GitHub\SillyTavern`
  
6. 双击 `start.bat` 文件。（注意：操作系统可能隐藏文件扩展名 `.bat`，此时该文件会显示为名为"`Start`"的文件，双击它即可运行 SillyTavern）

    ![image](/static/windows-3.png)

7. 双击后，将弹出一个大型黑色命令控制台窗口，SillyTavern 开始安装所需组件。
  
8. 安装完成后，如果一切正常，命令控制台窗口将如下所示，并且浏览器中将打开 SillyTavern 标签页：

    ![image](/static/windows-4.png)

9. 连接到任意[支持的 API](/Usage/API_Connections/index.md) 并开始聊天！
