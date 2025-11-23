---
label: 如何更新 SillyTavern
icon: repo-pull
order: -1
expanded: false
route: /installation/updating/
---


# 如何更新 SillyTavern

找到您的操作系统并按照说明更新 ST。

!!! 有关安装说明，请参阅[安装](/Installation/index.md)页面。

本指南假设您已经至少安装并运行过一次 SillyTavern。
!!!

----

## Linux/Termux 或 MacOS

您肯定是通过 git 安装的，所以只需在 SillyTavern 目录中执行 'git pull'。

- 使用 `cd SillyTavern` 进入正确的文件夹。
- 使用 `git pull` 获取更新。
- 使用 `./start.sh` 或 `bash start.sh` 启动 ST。

----

## Windows

>首先尝试使用位于 SillyTavern 安装根文件夹中的 `UpdateAndStart.bat`。

如果失败，请返回此处继续阅读。

### 方法 1 - GIT

我们始终建议用户使用 'git' 安装。原因如下：

当您通过 `git clone` 安装时，要更新只需[在 ST 文件夹的命令行中](https://www.google.com/search?q=how+to+open+command+prompt+in+a+folder)输入 `git pull`。
或者，如果命令提示符出现问题（并且您已安装 GitHub Desktop），您可以使用 `Repository` 菜单并选择 `Pull`。

更新会自动安全地应用。

#### "帮助！我最初是通过 Zip 安装的，现在想转换为 Git 安装"

您选择了一条明智的道路。

由于您的安装是通过 Zip 完成的，您需要使用 git 进行新的安装。

幸运的是，我们有[相关说明](/Installation/Windows.md)。

一旦您使用 git 在不同的文件夹中安装了新的 SillyTavern，请返回此页面并继续执行下面 'Zip 更新' 说明中的**第 4 步**。

### 方法 2 - ZIP

如果您坚持通过 zip 安装，以下是更新的繁琐过程：

1. 下载新的发布 zip。
2. 将其解压到当前 ST 安装目录之外的文件夹中。
3. 按照您的操作系统的常规设置程序安装 NodeJS 要求。

4. 根据需要(*)从旧的 ST 安装中复制以下文件/文件夹：

    (*) '根据需要' = "如果您创建了与这些文件夹相关的任何自定义内容"。
    
    #### 更新到 >=1.12.0
    
    将 `/data` 目录和 `config.yaml` 文件从一个安装复制到另一个安装。
    
    #### 从 <1.12.0 更新到 >1.12.0
    
    1.12.0 包含自动迁移程序。仅在迁移被中断或出错时才需要执行以下步骤。

5. 至少运行一次更新后的服务器安装以创建 `/data/default-user` 目录。
6. 根据需要将文件从旧的 `/public` 转移到新的 `/data/default-user`。
    
    所有文件夹都不是强制性的，只复制您需要的内容。
    
    **注意：不要复制整个 /PUBLIC/ 文件夹**
    
    这样做可能会破坏新安装并阻止新功能的出现。
    
    ```plaintext
    Assets
    Backgrounds
    Characters
    Chats
    Context
    Groups
    Group chats
    Instruct
    movingUI
    KoboldAI Settings
    NovelAI Settings
    OpenAI Settings
    QuickReplies
    TextGen Settings (textgen = ooba)
    Themes
    User Avatars
    Worlds
    User
    settings.json
    secrets.json <---- 这个在根文件夹中，不在 /public/ 中
    ```

7. 一旦这些文件夹/文件被复制，将它们粘贴到新安装的 /data/default-user 文件夹中（secrets.json 放在文件夹根目录中）。
8. 使用适合您操作系统的方法再次启动 SillyTavern，并祈祷一切正常。
9. 如果一切都显示正常，您可以安全地删除旧的 ST 文件夹。

### 常见更新问题

#### 我使用 Docker，更新后所有数据都消失了！

如果您使用的是 Docker 容器，请参阅[容器化 Docker 安装](/Installation/Updating/ST-1.12.0-Migration-Guide.md#容器化docker安装)。

#### "工作目录中有未解决的冲突。"

这意味着您修改了在远程仓库中已更改的默认文件（如设置预设）。

要修复此问题，请在终端中运行以下命令。请谨慎使用，因为它可能具有破坏性。如有需要，请确保有备份。

```bash
git merge --abort
git reset --hard
git pull --rebase --autostash
```

#### 文件更改阻止 git pull

- 如果您更改 SillyTavern 系统文件，`git pull` 可能无法工作。
- 有时更新可能需要我们更改重要文件，这可能导致相同的问题。
- 通常是默认预设文件或 `package-lock.json`。
- 在这种情况下，您可以尝试将文件移动到不同的文件夹（或删除文件），然后执行 `git pull`。
- 另一个解决方案是使用 `git pull --rebase --autostash`

#### 启动服务器时出现错误：找不到模块 "***"

- 这意味着 SillyTavern 添加了新的 npm 包要求。
- 在 SillyTavern 目录中运行 `npm install` 可以修复此问题。提供的 Start.bat 和 start.sh 脚本会自动执行此操作。
- 没有帮助？删除 node_modules 文件夹

**Windows**

```bash
rmdir /s /q node_modules
npm cache clean --force
npm install
```

**Unix/Linux**

```bash
rm -rf node_modules
npm cache clean --force
npm install
```
