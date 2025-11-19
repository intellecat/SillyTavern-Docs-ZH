---
# icon: container
label: Docker
route: /installation/docker/
---

# Docker 安装

!!!
这些说明假定您已经安装了 Docker，能够访问命令行以安装 container，并熟悉它们的一般操作。
!!!

## 使用 GitHub Container Registry

使用预构建的镜像是在 Docker 中开始使用 SillyTavern 最快速、最简单的方法。您可以从 GitHub Container Registry 拉取最新镜像。

### Docker Compose（推荐）

从 [GitHub 仓库](https://github.com/SillyTavern/SillyTavern/blob/release/docker/docker-compose.yml) 下载 `docker-compose.yml` 文件，并在文件所在的目录中运行以下命令。这将从 GitHub Container Registry 拉取最新的 release 镜像并启动 container，自动创建必要的 volumes。

```sh
docker compose up
```

您可以编辑文件并应用其他自定义设置以满足您的需求：

- 默认端口是 8000。您可以通过修改 `ports` 部分来更改它。
- 如果您想使用开发分支而不是稳定 release，请将 `image` 标签更改为 `staging`。
- 如果您想使用环境变量调整服务器配置，请查看[环境变量](/Administration/config-yaml.md#environment-variables)页面。

### Docker CLI（高级）

您需要两个必需的目录映射和一个端口映射才能让 SillyTavern 正常运行。在命令中，在以下位置替换您的选择：

#### Container 变量

##### Volume 映射

- `CONFIG_PATH` - 主机上存储 SillyTavern 配置文件的目录
- `DATA_PATH` - 主机上存储 SillyTavern 用户数据（包括角色）的目录
- `PLUGINS_PATH` - （可选）主机上存储 SillyTavern 服务器插件的目录
- `EXTENSIONS_PATH` - （可选）主机上存储全局 UI 扩展的目录

##### 端口映射

- `PUBLIC_PORT` - 用于公开流量的端口。这是必需的，因为您将从 container 的虚拟机外部访问实例。在没有实施单独的安全服务的情况下，请勿将其暴露到互联网。

##### 附加设置

- `SILLYTAVERN_VERSION` - 在 [GitHub Packages 页面](https://github.com/SillyTavern/SillyTavern/pkgs/container/sillytavern) 上，您将看到标记的镜像版本列表。镜像标签 "latest" 将使您保持最新的 release 版本。您也可以使用 "staging"，它指向相应分支的每日镜像。

#### 运行 container

1. 打开您的命令行
2. 在您想要存储配置和数据文件的文件夹中运行以下命令：

```bash
SILLYTAVERN_VERSION="latest"
PUBLIC_PORT="8000"
CONFIG_PATH="./config"
DATA_PATH="./data"
PLUGINS_PATH="./plugins"
EXTENSIONS_PATH="./extensions"

docker run \
  --name="sillytavern" \
  -p "$PUBLIC_PORT:8000/tcp" \
  -v "$CONFIG_PATH:/home/node/app/config:rw" \
  -v "$DATA_PATH:/home/node/app/data:rw" \
  -v "$EXTENSIONS_PATH:/home/node/app/public/scripts/extensions/third-party:rw" \
  -v "$PLUGINS_PATH:/home/node/app/plugins:rw" \
  ghcr.io/sillytavern/sillytavern:"$SILLYTAVERN_VERSION"
```

!!!tip
默认情况下，container 将在前台运行。如果您想在后台运行它，请在 `docker run` 命令中添加 `-d` 标志。
!!!

## 构建 Docker 镜像

!!!info
以下部分假定您在非 root（非管理员）文件夹中安装了 SillyTavern。如果您在 root 文件夹中安装了 SillyTavern，您可能需要使用管理员权限运行其中一些命令 [`sudo`、`doas`、Command Prompt (Administrator)]。
!!!

如果您想自己构建 Docker 镜像，可以按照以下步骤操作。如果您想自定义镜像或将其用于开发目的，这很有用。

### Linux

1. 按照 [这里](https://docs.docker.com/engine/install/) 的 Docker 安装指南安装 Docker。
   !!!danger
   **不要**安装 Docker Desktop。
   !!!
2. 按照 Docker [安装后指南](https://docs.docker.com/engine/install/linux-postinstall/) 中的**以非 root 用户身份管理 Docker**中的步骤进行操作。
3. 使用您的包管理器安装 [Git](https://git-scm.com/download/linux)。

    - Debian（Ubuntu/Pop! OS/等）

        ```sh
        sudo apt install git
        ```

    - Arch Linux（Manjaro/EndeavourOS/等）

        ```sh
        sudo pacman -S git
        ```

    - Fedora、Red Hat Enterprise Linux (RHEL) 等
        ```sh
        sudo dnf install git
        ```

4. 克隆 SillyTavern 仓库。

    - Release（稳定分支）

        ```sh
        git clone https://github.com/SillyTavern/SillyTavern && cd SillyTavern/docker
        ```

    - Staging（开发分支）
        ```sh
        git clone https://github.com/SillyTavern/SillyTavern -b staging && cd SillyTavern/docker
        ```

5. 在 Docker 文件夹中运行以下命令来执行 `docker compose`。

    ```sh
    docker compose up -d
    ```

6. 打开新浏览器并访问 [http://localhost:8000](http://localhost:8000)。您应该在几分钟内看到 SillyTavern 加载。

### Windows

!!!warning 关于 Windows 上的 Docker
在 Windows 上使用 Docker **_非常_**复杂。您不仅需要在_启用或关闭 Windows 功能_中激活 _Windows Subsystem for Linux_，还需要为虚拟化（Intel VT-d/AMD SVM）配置系统，这因 PC 制造商或主板制造商而异。有时，此选项在某些系统上不存在。

强烈建议您按照我们的 [Windows](/Installation/Windows.md) 指南安装 SillyTavern。本节只是_大致_介绍如何在 Windows 上完成此操作。
!!!

1.  按照 [这里](https://docs.docker.com/desktop/setup/install/windows-install/) 的 Docker 安装指南安装 Docker Desktop。
2.  安装 [Git for Windows](https://git-scm.com/download/win)。
3.  克隆 SillyTavern 仓库。

    -   Release（稳定分支）

        ```sh
        git clone https://github.com/SillyTavern/SillyTavern && cd SillyTavern/docker
        ```

    -   Staging（开发分支）
        ```sh
        git clone https://github.com/SillyTavern/SillyTavern -b staging && cd SillyTavern/docker
        ```

4.  在 Docker 文件夹中运行以下命令来执行 `docker compose`。

    ```sh
    docker compose up -d
    ```

5.  打开新浏览器并访问 [http://localhost:8000](http://localhost:8000)。您应该在几分钟内看到 SillyTavern 加载。

### macOS

!!!
尽管 macOS 与 Linux 类似，但它没有 Docker Engine。您需要像 Windows 一样安装 Docker Desktop。
您还需要安装 [Homebrew](https://brew.sh/) 才能在 Mac 上安装 Git。本节只是_大致_介绍如何在 macOS 上完成此操作。
!!!

1.  按照 [这里](https://docs.docker.com/desktop/setup/install/mac-install/) 的 Docker 安装指南安装 Docker Desktop。
2.  使用 Homebrew 安装 `git`。

    ```sh
    brew install git
    ```

3.  克隆 SillyTavern 仓库。

    -   Release（稳定分支）

        ```sh
        git clone https://github.com/SillyTavern/SillyTavern && cd SillyTavern/docker
        ```

    -   Staging（开发分支）
        ```sh
        git clone https://github.com/SillyTavern/SillyTavern -b staging && cd SillyTavern/docker
        ```

4.  在 Docker 文件夹中运行以下命令来执行 `docker compose`。

    ```sh
    docker compose up -d
    ```

5.  打开新浏览器并访问 [http://localhost:8000](http://localhost:8000)。您应该在几分钟内看到 SillyTavern 加载。

## 配置 SillyTavern

SillyTavern 的配置文件（config.yaml）将位于 `config` 文件夹中。配置该文件应该与不使用 Docker 时配置它没有什么不同，但是您需要使用管理员权限运行 `nano` 或代码编辑器才能保存更改。

!!!warning
不要忘记重启 SillyTavern 的 Docker container 以应用您的更改！确保在 `docker` 文件夹中执行此命令。

```sh
docker compose restart sillytavern
```

!!!

## 定位用户数据

SillyTavern 的数据文件夹将位于 `data` 文件夹中。备份文件应该很容易，但是，恢复或向其中添加内容可能需要您使用管理员权限执行此操作。

## 运行服务器插件

在 Docker 中运行像 [HoYoWiki-Scraper-TS](https://github.com/Bronya-Rand/HoYoWiki-Scraper-TS) 或 [SillyTavern-Fandom-Scraper](https://github.com/SillyTavern/SillyTavern-Fandom-Scraper) 这样的插件与在没有 Docker 的系统上运行它没有什么不同，但是我们需要对 Docker Compose 脚本进行轻微修改才能这样做。

!!! Note
如果您已经在 `docker` 文件夹中看到了 _plugins_ 文件夹，则可以跳过步骤 1-2。
!!!

1. 使用 `nano` 或代码编辑器，打开 _docker-compose.yml_ 并在 `volumes` 下方添加以下行。

    ```sh
        volumes:
            - "./config:/home/node/app/config"
            - "./data:/home/node/app/data"
            - "./plugins:/home/node/app/plugins"
    ```

2. 在 `docker` 文件夹中创建一个名为 _plugins_ 的新文件夹。
3. 按照您的插件说明安装插件。
4. 使用 `nano` 或具有管理员权限的代码编辑器，打开 _config.yaml_（在 `config` 文件夹中）并启用 `enableServerPlugins`

    ```sh
    enableServerPlugins: true
    ```

5. 重启 Docker container。

    ```sh
    docker compose restart sillytavern
    ```

## Docker 常见问题

### SELinux 挂载 Volume 的权限问题

启用了 SELinux 的 Linux 发行版（如 RHEL、CentOS、Fedora 等）可能会由于安全策略而阻止 Docker container 访问挂载的 volume。这可能会导致 container 尝试读取或写入挂载的目录时出现权限被拒绝错误。

可以在 volume 挂载中添加两个后缀 `:z` 或 `:Z`。这些后缀告诉 Docker 重新标记共享 volume 上的文件对象。

- 当 volume 内容将在 container 之间共享时，使用 `z` 选项。
- 当 volume 内容仅应由当前 container 使用时，使用 `Z` 选项。

示例：

```yaml
# docker-compose.yml
volumes:
  ## 共享 volume
  - ./config:/home/node/app/config:z
  ## 私有 volume
  - ./data:/home/node/app/data:Z
```

### 被白名单禁止

!!!
如果 [whitelistDockerHosts](/Administration/config-yaml.md#ip-whitelisting) 配置值设置为 `true`，Docker gateway IP 应该会自动加入白名单。

如果您仍然无法访问 SillyTavern，请按照以下说明手动更新白名单。
!!!

1. 执行以下 Docker 命令以获取您的 SillyTavern Docker container 的 IP。

    ```sh
    docker network inspect docker_default
    ```

    您应该会收到类似于以下内容的输出。

    ```json
    [
        {
            "Name": "docker_default",
            "IPAM": {
                "Config": [
                    {
                        "Subnet": "172.18.0.0/16",
                        "Gateway": "172.18.0.1"
                    }
                ]
            }
        }
    ]
    ```

    记下您在 _Gateway_ 中看到的 IP，因为这很重要。

2. 使用具有管理员权限的文本编辑器，转到 `config` 并打开 `config.yaml`。

    在编辑器中，向下滚动到 `whitelist` 部分。您应该会看到类似于以下内容。

    ```yaml
    whitelist:
        - 127.0.0.1
    ```

    在 _127.0.0.1_ 下方添加新行，并输入您从 Docker 复制的 IP。之后应该类似于以下内容。

    ```yaml
    whitelist:
        - 127.0.0.1
        - 172.18.0.1
    ```

    保存文件并退出文本编辑器。

    !!!info
    请注意，如果您将 Docker 网络配置为 bridge，您也可以像往常一样将外部 IP 地址添加到白名单中。
    !!!

3. 重启 Docker Container 以应用新配置。

    ```sh
    docker compose restart sillytavern
    ```
