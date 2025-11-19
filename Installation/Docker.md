---
# icon: container
label: Docker
route: /installation/docker/
---


# Docker 安装指南

!!! info
本指南假设您在非 root（非管理员）文件夹中安装 SillyTavern。如果您在 root 文件夹中安装 SillyTavern，您可能需要使用管理员权限运行某些命令[`sudo`、`doas`、Command Prompt (Administrator)]。
!!!

## 安装

### Linux

1. 请按照[此处](https://docs.docker.com/engine/install/)的 Docker 安装指南安装 Docker。
   !!! danger
   **不要**安装 Docker Desktop。
   !!!
2. 按照 Docker [安装后配置指南](https://docs.docker.com/engine/install/linux-postinstall/)中的**以非 root 用户身份管理 Docker**部分进行操作。
3. 使用包管理器安装 [Git](https://git-scm.com/download/linux)。

    - Debian (Ubuntu/Pop! OS/等)

        ```sh
        sudo apt install git
        ```

    - Arch Linux (Manjaro/EndeavourOS/等)

        ```sh
        sudo pacman -S git
        ```

    - Fedora, Red Hat Enterprise Linux (RHEL), 等
        ```sh
        sudo dnf install git
        ```

4. 克隆 SillyTavern 仓库。

    - Release (稳定分支)

        ```sh
        git clone https://github.com/SillyTavern/SillyTavern && cd SillyTavern/docker
        ```

    - Staging (开发分支)
        ```sh
        git clone https://github.com/SillyTavern/SillyTavern -b staging && cd SillyTavern/docker
        ```

5. 在 Docker 文件夹中运行以下命令执行 `docker compose`。

    ```sh
    docker compose up -d
    ```

6. 执行以下 Docker 命令获取 SillyTavern Docker 容器的 IP 地址。

    ```sh
    docker network inspect docker_default
    ```

    您应该会看到类似下面的输出：

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

    记下您在 _Gateway_ 中看到的 IP 地址，这很重要。

7. 使用 `sudo` 打开 `nano` 并运行以下命令：

    ```sh
    sudo nano config/config.yaml
    ```

    在 `nano` 中，找到 `whitelist`。您应该会看到类似下面的内容：

    ```yaml
    whitelist:
        - 127.0.0.1
    ```

    在 _127.0.0.1_ 下面添加一行，输入您从 Docker 复制的 IP 地址。完成后应该类似这样：

    ```yaml
    whitelist:
        - 127.0.0.1
        - 172.18.0.1
    ```

    按 _Ctrl+S_ 保存文件，然后按 _Ctrl+X_ 退出 `nano`。

    !!! info
    注意，如果您将 Docker 网络配置为桥接模式，您也可以像往常一样将外部 IP 地址添加到白名单中。
    !!!

8. 重启 Docker 容器以应用新配置。

    ```sh
    docker compose restart sillytavern
    ```

9. 打开新的浏览器并访问 [http://localhost:8000](http://localhost:8000)。几分钟后您应该就能看到 SillyTavern 加载完成。

10. 尽情享用！ :D

### Windows

!!! warning 关于 Windows 上的 Docker
在 Windows 上使用 Docker **非常**复杂。您不仅需要在"启用或关闭 Windows 功能"中激活 _Windows Subsystem for Linux_，还需要为虚拟化配置系统（Intel VT-d/AMD SVM），这因 PC 制造商（或主板制造商）而异。有时，某些系统上甚至没有这个选项。

强烈建议您按照我们的 [Windows](/Installation/Windows.md) 指南安装 SillyTavern。本节仅是在 Windows 上安装的一个_粗略_思路。
!!!

1.  按照[此处](https://docs.docker.com/desktop/setup/install/windows-install/)的 Docker 安装指南安装 Docker Desktop。
2.  安装 [Git for Windows](https://git-scm.com/download/win)。
3.  克隆 SillyTavern 仓库。

    -   Release (稳定分支)

        ```sh
        git clone https://github.com/SillyTavern/SillyTavern && cd SillyTavern/docker
        ```

    -   Staging (开发分支)
        ```sh
        git clone https://github.com/SillyTavern/SillyTavern -b staging && cd SillyTavern/docker
        ```

4.  在 Docker 文件夹中运行以下命令执行 `docker compose`。

    ```sh
    docker compose up -d
    ```

5.  执行以下 Docker 命令获取 SillyTavern Docker 容器的 IP 地址。

    ```sh
    docker network inspect docker_default
    ```

    您应该会看到类似下面的输出：

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

    记下您在 _Gateway_ 中看到的 IP 地址，这很重要。

6.  以管理员权限运行 _Notepad_ 或您选择的代码编辑器，进入 `config` 目录并打开 _config.yaml_。

    在您选择的编辑器中，您应该会看到类似下面的内容：

    ```yaml
    whitelist:
        - 127.0.0.1
    ```

    在 _127.0.0.1_ 下面添加一行，输入您从 Docker 复制的 IP 地址。完成后应该类似这样：

    ```yaml
    whitelist:
        - 127.0.0.1
        - 172.18.0.1
    ```

    按 _Ctrl+S_ 保存文件，然后退出编辑器。

    !!! info
    注意，如果您将 Docker 网络配置为桥接模式，您也可以像往常一样将外部 IP 地址添加到白名单中。
    !!!

7.  重启 Docker 容器以应用新配置。

    ```sh
    docker compose restart sillytavern
    ```

8.  打开新的浏览器并访问 [http://localhost:8000](http://localhost:8000)。几分钟后您应该就能看到 SillyTavern 加载完成。

9.  尽情享用！ :D

### macOS

!!!
尽管 macOS 与 Linux 类似，但它没有 Docker Engine。您需要像 Windows 一样安装 Docker Desktop。
您还需要安装 [Homebrew](https://brew.sh/) 以在 Mac 上安装 Git。本节仅是在 macOS 上安装的一个_粗略_思路。
!!!

1.  按照[此处](https://docs.docker.com/desktop/setup/install/mac-install/)的 Docker 安装指南安装 Docker Desktop。
2.  使用 Homebrew 安装 `git`。

    ```sh
    brew install git
    ```

3.  克隆 SillyTavern 仓库。

    -   Release (稳定分支)

        ```sh
        git clone https://github.com/SillyTavern/SillyTavern && cd SillyTavern/docker
        ```

    -   Staging (开发分支)
        ```sh
        git clone https://github.com/SillyTavern/SillyTavern -b staging && cd SillyTavern/docker
        ```

4.  在 Docker 文件夹中运行以下命令执行 `docker compose`。

    ```sh
    docker compose up -d
    ```

5.  执行以下 Docker 命令获取 SillyTavern Docker 容器的 IP 地址。

    ```sh
    docker network inspect docker_default
    ```

    您应该会看到类似下面的输出：

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

    记下您在 _Gateway_ 中看到的 IP 地址，这很重要。

6.  使用 `sudo` 打开 `nano` 并运行以下命令。

    ```sh
    sudo nano config/config.yaml
    ```

    !!!
    If you can't run `nano`, either install it via Homebrew or use TextEdit.
    !!!

    Within `nano`, go down to `whitelist`. You should see something similar to the following below.

    ```yaml
    whitelist:
        - 127.0.0.1
    ```

    Add a new line below _127.0.0.1_ and put in the IP you copied from Docker. It should look something similar to the following afterwards.

    ```yaml
    whitelist:
        - 127.0.0.1
        - 172.18.0.1
    ```

    Save the file by pressing _Ctrl+S_ then exit `nano` by pressing _Ctrl+X_.

    !!! info
    Note that if you configured Docker network as a bridge, you could also add external IP addresses to the whitelist as usual.
    !!!

7.  Restart the Docker Container to apply the new configuration.

    ```sh
    docker compose restart sillytavern
    ```

8.  Open an new browser and go to [http://localhost:8000](http://localhost:8000). You should see SillyTavern load in a few moments.

9.  Enjoy! :D

## Configuring SillyTavern

SillyTavern's configuration file (config.yaml) will be located within the `config` folder. Configuring the config file should be no different than configuring it without Docker, however you will need to run `nano` or a code editor with administrator rights in order to save your changes.

!!! warning
Don't forget to restart the Docker container for SillyTavern in order to apply your changes! Make sure you execute this command within the `docker` folder.

```sh
docker compose restart sillytavern
```

!!!

## Locating User Data

SillyTavern's data folder will be within the `data` folder. Backing up your files should be easy to do, however, restoring or adding content into it may require you to do so with administrator rights.

## Running Server Plugins

Running plugins like [HoYoWiki-Scraper-TS](https://github.com/Bronya-Rand/HoYoWiki-Scraper-TS) or [SillyTavern-Fandom-Scraper](https://github.com/SillyTavern/SillyTavern-Fandom-Scraper) within Docker is no different from running it on your system without Docker, however we will need to do a slight modification to the Docker Compose script in order to do so.

!!! Note
If you already see a _plugins_ folder within the `docker` folder, you can skip Steps 1-2.
!!!

1. Using `nano` or a code editor, open _docker-compose.yml_ and add the following line below `volumes`.

    ```sh
        volumes:
            - "./config:/home/node/app/config"
            - "./data:/home/node/app/data"
            - "./plugins:/home/node/app/plugins"
    ```

2. Create a new folder within the `docker` folder called _plugins_.
3. Follow your plugin's instructions on installing the plugin.
4. Using `nano` or a code editor with administrator rights, open _config.yaml_ (within the `config` folder) and enable `enableServerPlugins`

    ```sh
    enableServerPlugins: true
    ```

5. Restart the Docker container.

    ```sh
    docker compose restart sillytavern
    ```

6. Profit.
