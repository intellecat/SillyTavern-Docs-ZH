# Docker 安装

!!!
本说明假设你已安装 Docker，能够访问命令行来安装容器，并熟悉其基本操作。
!!!

## 使用 GitHub 容器注册表

使用预构建镜像是在 Docker 中运行 SillyTavern 最快捷、最简单的方式。你可以从 GitHub 容器注册表拉取最新镜像。

### Docker Compose（推荐）

从 [GitHub 仓库](https://github.com/SillyTavern/SillyTavern/blob/release/docker/docker-compose.yml) 下载 `docker-compose.yml` 文件，然后在文件所在目录运行以下命令。这将从 GitHub 容器注册表拉取最新发布镜像并启动容器，同时自动创建所需的卷。

```sh
docker compose up
```

你可以编辑该文件，根据需要进行额外自定义：

- 默认端口为 8000。你可以通过修改 `ports` 部分来更改它。
- 如果想使用开发分支而非稳定版本，将 `image` 标签改为 `staging`。
- 如果想通过环境变量调整服务器配置，请查看[环境变量](/Administration/config-yaml.md#environment-variables)页面。

### Docker CLI（进阶）

你需要两个必填的目录映射和一个端口映射才能使 SillyTavern 正常运行。在命令中，请替换以下变量的值：

#### 容器变量

##### 卷映射

- `CONFIG_PATH` - 宿主机上存储 SillyTavern 配置文件的目录
- `DATA_PATH` - 宿主机上存储 SillyTavern 用户数据（包括角色）的目录
- `PLUGINS_PATH` - （可选）宿主机上存储 SillyTavern 服务器插件的目录
- `EXTENSIONS_PATH` - （可选）宿主机上存储全局 UI 扩展的目录

##### 端口映射

- `PUBLIC_PORT` - 对外暴露流量的端口。此项为必填，因为你需要从虚拟机容器外部访问该实例。**请勿**在没有独立安全服务的情况下将其暴露到互联网。

##### 附加设置

- `SILLYTAVERN_VERSION` - 在 [GitHub Packages 页面](https://github.com/SillyTavern/SillyTavern/pkgs/container/sillytavern) 上可以看到已标记的镜像版本列表。镜像标签 "latest" 会保持与当前发布版本同步。你也可以使用 "staging"，它指向相应分支的每日构建镜像。

#### 运行容器

1. 打开命令行
2. 在你希望存储配置和数据文件的文件夹中运行以下命令：

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
默认情况下，容器将在前台运行。如果你想在后台运行，请在 `docker run` 命令中添加 `-d` 标志。
!!!

## 构建 Docker 镜像

!!!info
以下部分假设你已将 SillyTavern 安装在非 root（非管理员）文件夹中。如果你将 SillyTavern 安装在 root 文件夹，则可能需要以管理员权限运行其中部分命令 [`sudo`、`doas`、命令提示符（管理员）]。
!!!

如果你想自行构建 Docker 镜像，可以按照以下步骤操作。这适用于想要自定义镜像或用于开发目的的情况。

### Linux

1. 按照 [Docker 安装指南](https://docs.docker.com/engine/install/) 安装 Docker。
   !!!danger
   **不要**安装 Docker Desktop。
   !!!
2. 按照 Docker [安装后指南](https://docs.docker.com/engine/install/linux-postinstall/) 中的**以非 root 用户管理 Docker** 步骤进行操作。
3. 使用包管理器安装 [Git](https://git-scm.com/download/linux)。

    - Debian（Ubuntu/Pop! OS 等）

        ```sh
        sudo apt install git
        ```

    - Arch Linux（Manjaro/EndeavourOS 等）

        ```sh
        sudo pacman -S git
        ```

    - Fedora、Red Hat Enterprise Linux（RHEL）等
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

5. 在 Docker 文件夹中执行以下命令运行 `docker compose`。

    ```sh
    docker compose up -d
    ```

6. 打开浏览器，访问 [http://localhost:8000](http://localhost:8000)。片刻后即可看到 SillyTavern 加载完成。

### Windows

!!!warning 关于在 Windows 上使用 Docker
在 Windows 上使用 Docker **_非常_**复杂。不仅需要在"启用或关闭 Windows 功能"中激活 _Windows Subsystem for Linux_，还需要为虚拟化配置系统（Intel VT-d/AMD SVM），而这因 PC 制造商（或主板制造商）不同而有所差异，有些系统上甚至没有此选项。

强烈建议按照我们的 [Windows](/Installation/Windows.md) 指南安装 SillyTavern。本节仅提供在 Windows 上操作的_大致_思路。
!!!

1.  按照 [Docker 安装指南](https://docs.docker.com/desktop/setup/install/windows-install/) 安装 Docker Desktop。
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

4.  在 Docker 文件夹中执行以下命令运行 `docker compose`。

    ```sh
    docker compose up -d
    ```

5.  打开浏览器，访问 [http://localhost:8000](http://localhost:8000)。片刻后即可看到 SillyTavern 加载完成。

### macOS

!!!
尽管 macOS 与 Linux 类似，但它没有 Docker Engine。你需要像 Windows 一样安装 Docker Desktop。你还需要安装 [Homebrew](https://brew.sh/) 来在 Mac 上安装 Git。本节仅提供在 macOS 上操作的_大致_思路。
!!!

1.  按照 [Docker 安装指南](https://docs.docker.com/desktop/setup/install/mac-install/) 安装 Docker Desktop。
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

4.  在 Docker 文件夹中执行以下命令运行 `docker compose`。

    ```sh
    docker compose up -d
    ```

5.  打开浏览器，访问 [http://localhost:8000](http://localhost:8000)。片刻后即可看到 SillyTavern 加载完成。

## 配置 SillyTavern

SillyTavern 的配置文件（config.yaml）位于 `config` 文件夹中。配置方式与不使用 Docker 时基本相同，但你需要以管理员权限运行 `nano` 或代码编辑器才能保存更改。

!!!warning
别忘了重启 SillyTavern 的 Docker 容器以应用更改！请确保在 `docker` 文件夹中执行此命令。

```sh
docker compose restart sillytavern
```

!!!

## 定位用户数据

SillyTavern 的数据文件夹位于 `data` 文件夹中。备份文件应该很容易，但恢复或添加内容可能需要以管理员权限进行操作。

## 运行服务器插件

在 Docker 中运行 [HoYoWiki-Scraper-TS](https://github.com/Bronya-Rand/HoYoWiki-Scraper-TS) 或 [SillyTavern-Fandom-Scraper](https://github.com/SillyTavern/SillyTavern-Fandom-Scraper) 等插件与在系统上直接运行并无不同，但需要对 Docker Compose 脚本进行少量修改。

!!! Note
如果你已经在 `docker` 文件夹中看到 _plugins_ 文件夹，可以跳过步骤 1-2。
!!!

1. 使用 `nano` 或代码编辑器打开 _docker-compose.yml_，在 `volumes` 下方添加以下内容。

    ```sh
        volumes:
            - "./config:/home/node/app/config"
            - "./data:/home/node/app/data"
            - "./plugins:/home/node/app/plugins"
    ```

2. 在 `docker` 文件夹中新建一个名为 _plugins_ 的文件夹。
3. 按照插件的安装说明进行安装。
4. 使用 `nano` 或以管理员权限打开代码编辑器，打开（`config` 文件夹中的）_config.yaml_ 并启用 `enableServerPlugins`。

    ```sh
    enableServerPlugins: true
    ```

5. 重启 Docker 容器。

    ```sh
    docker compose restart sillytavern
    ```

## 非 root 用户模式

默认情况下，容器以 root 身份运行。如果你希望挂载卷中创建的文件归特定宿主机用户所有（例如，避免出现 root 拥有的文件），可以启用非 root 模式。

### 选项 1：PUID/PGID（推荐）

将 `PUID` 和 `PGID` 环境变量设置为你希望容器使用的 UID/GID。入口脚本将更新所需目录的所有权，然后以映射的用户身份运行服务器。

Docker Compose 示例：

```yaml
services:
  sillytavern:
    environment:
      - PUID=1000
      - PGID=1000
```

Docker CLI 示例：

```bash
docker run \
  --name="sillytavern" \
  -e PUID=1000 \
  -e PGID=1000 \
  -p "$PUBLIC_PORT:8000/tcp" \
  -v "$CONFIG_PATH:/home/node/app/config:rw" \
  -v "$DATA_PATH:/home/node/app/data:rw" \
  -v "$EXTENSIONS_PATH:/home/node/app/public/scripts/extensions/third-party:rw" \
  -v "$PLUGINS_PATH:/home/node/app/plugins:rw" \
  ghcr.io/sillytavern/sillytavern:"$SILLYTAVERN_VERSION"
```

### 选项 2：Docker `--user` 标志

你也可以使用 Docker 的 `--user` 标志以特定用户身份运行容器。在此模式下，容器无法自动修复权限，因此请确保挂载的卷已对你提供的 UID/GID 可写。

```bash
docker run \
  --name="sillytavern" \
  --user 1000:1000 \
  -p "$PUBLIC_PORT:8000/tcp" \
  -v "$CONFIG_PATH:/home/node/app/config:rw" \
  -v "$DATA_PATH:/home/node/app/data:rw" \
  -v "$EXTENSIONS_PATH:/home/node/app/public/scripts/extensions/third-party:rw" \
  -v "$PLUGINS_PATH:/home/node/app/plugins:rw" \
  ghcr.io/sillytavern/sillytavern:"$SILLYTAVERN_VERSION"
```

## 容器健康检查

Docker 镜像内置了健康检查机制，用于监控 SillyTavern 服务器的响应状态。这对于容器编排系统（如 Docker Compose、Kubernetes 或 Docker Swarm）检测并自动重启无响应的容器非常有用。

### 工作原理

健康检查使用心跳文件机制：

1. 启用后，SillyTavern 服务器会定期将时间戳写入数据目录中的 `heartbeat.json` 文件。
2. 健康检查脚本（`src/healthcheck.js`）验证心跳文件是否存在且已在近期更新。
3. 如果心跳文件缺失或过旧（超过 2 个间隔未更新），容器将被标记为不健康。

### 配置

!!!warning
健康检查脚本不支持通过命令行参数覆盖数据目录。如果你将数据目录从默认的 `/home/node/app/data` 修改为其他路径，请确保相应地设置 `SILLYTAVERN_DATAROOT` 环境变量。
!!!

健康检查由 `SILLYTAVERN_HEARTBEATINTERVAL` 环境变量（或 config.yaml 中的 `heartbeatInterval`）控制。该值指定心跳写入的间隔秒数。

- **默认值：** `0`（禁用）
- **推荐值：** 使用 Docker 健康检查时建议设为 `30` 秒

默认的 `docker-compose.yml` 文件已包含启用心跳的健康检查配置：

```yaml
services:
  sillytavern:
    environment:
      - SILLYTAVERN_HEARTBEATINTERVAL=30
    healthcheck:
      test: ["CMD", "node", "src/healthcheck.js"]
      interval: 30s
      timeout: 10s
      start_period: 20s
      retries: 3
```

### 检查容器健康状态

你可以使用以下命令检查容器的健康状态：

```sh
docker inspect --format='{{.State.Health.Status}}' sillytavern
```

或查看包含健康状态在内的完整容器状态：

```sh
docker ps
```

`STATUS` 列将在运行时长旁边显示 `healthy`、`unhealthy` 或 `starting`。

### 禁用健康检查

如果不需要健康检查功能，可以通过以下方式禁用：

1. 将环境变量设置为 `0`：

    ```yaml
    environment:
      - SILLYTAVERN_HEARTBEATINTERVAL=0
    ```

2. 删除或注释掉 `docker-compose.yml` 中的 `healthcheck` 部分。

## Docker 常见问题

### 挂载卷的 SELinux 权限问题

启用了 SELinux 的 Linux 发行版（如 RHEL、CentOS、Fedora 等）可能因安全策略而阻止 Docker 容器访问挂载的卷，导致容器在读写挂载目录时出现权限拒绝错误。

可以在卷挂载后添加 `:z` 或 `:Z` 后缀，告知 Docker 对共享卷上的文件对象重新打标签。

- `z` 选项用于卷内容将在多个容器间共享的情况。
- `Z` 选项用于卷内容仅供当前容器使用的情况。

示例：

```yaml
# docker-compose.yml
volumes:
  ## 共享卷
  - ./config:/home/node/app/config:z
  ## 私有卷
  - ./data:/home/node/app/data:Z
```

### 被白名单禁止访问

!!!warning Docker Desktop 与 Docker CE 的区别
[whitelistDockerHosts](/Administration/config-yaml.md#ip-whitelisting) 配置选项（默认启用）通过解析 `host.docker.internal` 和 `gateway.docker.internal` 主机名来工作。这些主机名**仅在 Docker Desktop**（Windows/Mac）中可用。如果你在 **Linux 上使用 Docker CE**，这些主机名将无法解析，自动白名单功能将失败，容器日志中会出现如下错误：

```
Failed to resolve whitelist hostname host.docker.internal: getaddrinfo ENOTFOUND host.docker.internal
Failed to resolve whitelist hostname gateway.docker.internal: getaddrinfo ENOTFOUND gateway.docker.internal
```

在这种情况下，你需要按照以下步骤手动将 Docker 网关 IP 添加到白名单。
!!!

1. 执行以下 Docker 命令以获取 SillyTavern Docker 容器的 IP。

    ```sh
    docker network inspect docker_default
    ```

    你将看到类似如下的输出。

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

    记下 _Gateway_ 中显示的 IP，这将在后续步骤中用到。

2. 以管理员权限运行你选择的文本编辑器，进入 `config` 文件夹并打开 `config.yaml`。

    在编辑器中找到 `whitelist` 部分，你将看到类似如下的内容。

    ```yaml
    whitelist:
        - 127.0.0.1
    ```

    在 _127.0.0.1_ 下方添加一行，填入从 Docker 复制的 IP。添加后应类似如下。

    ```yaml
    whitelist:
        - 127.0.0.1
        - 172.18.0.1
    ```

    保存文件并退出文本编辑器。

    !!!info
    注意，如果你将 Docker 网络配置为桥接模式，也可以像往常一样将外部 IP 地址添加到白名单中。
    !!!

3. 重启 Docker 容器以应用新配置。

    ```sh
    docker compose restart sillytavern
    ```
