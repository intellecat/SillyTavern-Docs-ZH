---
label: 更新
icon: repo-pull
order: -1
expanded: false
route: /installation/updating/
---

# 如何更新 SillyTavern

在下方找到您的操作系统并按照说明更新 ST。

!!! 有关安装说明,请参阅 [安装](/Installation/index.md) 页面。

本指南假设您已经至少安装并运行过一次 SillyTavern。
!!!

----

## Linux/Termux 或 MacOS

您肯定是通过 git 安装的,因此只需在 SillyTavern 目录内执行 'git pull'。

- `cd SillyTavern` 进入正确的文件夹。
- `git pull` 获取更新。
- `./start.sh` 或 `bash start.sh` 启动 ST。

----

## Windows

>首先尝试使用位于 SillyTavern 安装根目录中的 `UpdateAndStart.bat`。

如果失败,请返回此处继续阅读。

### 方法 1 - GIT

我们始终建议用户使用 'git' 安装。原因如下:

当您通过 `git clone` 安装时,更新所需做的就是在 [ST 文件夹的命令行中](https://www.google.com/search?q=how+to+open+command+prompt+in+a+folder) 输入 `git pull`。
或者,如果命令提示符出现问题(并且您已安装 GitHub Desktop),您可以使用 `Repository` 菜单并选择 `Pull`。

更新会自动且安全地应用。

#### "救命!我最初是通过 Zip 安装的,现在想转换为 Git 安装"

您选择了明智的道路。

由于您的安装是通过 Zip 完成的,您需要使用 git 进行新的安装。

幸运的是,我们有关于如何操作的 [说明](/Installation/Windows.md)。

一旦您使用 git 将新的 SillyTavern 安装到不同的文件夹中,请返回此页面并继续执行下面 'Zip 更新' 说明的 **步骤 4**。

### 方法 2 - ZIP

如果您坚持通过 zip 安装,以下是执行更新的繁琐过程:

1. 下载新版本的 zip 文件。
2. 将其解压到当前 ST 安装目录之外的文件夹中。
3. 按照您的操作系统执行常规设置过程以安装 NodeJS 要求。

4. 根据需要(*) 从旧 ST 安装中复制以下文件/文件夹:

    (*) '根据需要' = "如果您制作了与这些文件夹相关的任何自定义内容"。

    #### 更新 >=1.12.0

    将 `/data` 目录和 `config.yaml` 文件从一个安装复制到另一个安装。如果您有想要保留的服务器范围扩展(为"所有用户"安装),还需复制 `/public/scripts/extensions/third-party` 目录。

    #### 从 <1.12.0 更新到 >1.12.0

    1.12.0 包含自动迁移程序。只有在迁移被中断或出错时才需要执行以下步骤。

5. 至少运行一次更新后的服务器安装以创建 `/data/default-user` 目录。
6. 根据需要将文件从旧的 `/public` 转移到新的 `/data/default-user`。

    这些文件夹都不是强制性的,所以只复制您需要的内容。

    **注意:不要复制整个 /PUBLIC/ 文件夹**

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
    secrets.json <---- 这个文件在根目录中,不在 /public/ 中
    ```

7. 复制这些文件夹/文件后,将它们粘贴到新安装的 /data/default-user 文件夹中(secrets.json 放在文件夹根目录)。
8. 使用适合您操作系统的方法再次启动 SillyTavern,并祈祷您做对了。
9. 如果一切正常显示,您可以安全地删除旧的 ST 文件夹。

### 常见更新问题

#### "工作目录中存在未解决的冲突。"

这意味着您修改了远程仓库中已更改的默认文件(例如设置预设)。

要解决此问题,请在终端中运行以下命令。请谨慎使用,因为它可能具有破坏性。如有需要,请确保进行备份。

```bash
git merge --abort
git reset --hard
git pull --rebase --autostash
```

#### 文件更改阻止 git pull

- 如果您更改了 SillyTavern 系统文件,`git pull` 可能无法工作。
- 有时更新可能需要我们更改重要文件,这可能导致同样的问题。
- 通常是默认预设文件或 `package-lock.json`。
- 在这种情况下,您可以尝试将文件移动到不同的文件夹(或删除文件),然后执行 `git pull`。
- 另一个解决方案是使用 `git pull --rebase --autostash`

#### 错误:启动服务器时找不到模块 "***"

- 这意味着 SillyTavern 添加了新的 npm 包要求。
- 在 SillyTavern 目录中运行 `npm install` 来解决此问题。提供的 Start.bat 和 start.sh 脚本会自动执行此操作。
- 没有帮助?删除 node_modules 文件夹

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

## Docker

1. 打开终端窗口并导航到您的 docker 目录 `cd SillyTavern/docker`
2. 使用 `docker compose down` 删除您的容器
3. 从缓存中删除 SillyTavern docker 镜像 `docker rmi ghcr.io/sillytavern/sillytavern:latest`(如果您针对的是 staging 分支,请将 `sillytavern:latest` 替换为 `sillytavern:staging`。)
4. 使用 `sudo docker compose up -d` 重建容器

如果一切顺利,docker 应该开始重新下载镜像,您很快就会启动并运行。如果遇到任何问题,请参阅本指南的下一部分。

### 常见更新问题
#### 我使用 Docker,更新后所有数据都消失了!

您必须遵循 [Docker 容器的迁移指南](/Installation/Updating/ST-1.12.0-Migration-Guide.md#containerized-docker-installs) 来更新 1.12.0 中引入的新数据模型的卷映射

#### 运行 docker 命令时权限被拒绝

这是 Linux 问题,意味着您的权限设置不正确。有两种方法可以解决此问题:

1. **简单方法**:如果您的用户具有 sudo 访问权限,只需在命令前加上 `sudo`(例如:`sudo docker compose down`)
2. **正确方法**:修复您的权限。这取决于您使用的 Linux 版本。网上有很多指南可以帮助您解决此问题。