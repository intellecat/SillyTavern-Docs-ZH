---
label: Reverse proxying
order: -50
icon: server
route: /usage/st-reverse-proxy-guide/
---

!!!danger 注意
本节内容**不涉及** OpenAI/Claude 反向代理。这里专指 **HTTP/HTTPS 反向代理**。
!!!

Termux 的配置是不是让你一头雾水？厌倦了在每台设备上反复更新和安装 SillyTavern？想统一管理你的聊天记录和角色？那你来对地方了。本指南将（希望能）介绍如何将 SillyTavern 托管在你的个人电脑上，让你从任何地方都能连接进来，并在同一台运行 AI 模型的机器上与角色聊天！

!!!warning 警告
本指南**不适合**新手。内容会涉及较多技术细节。
!!!

## 预先说明

!!!info 致 Windows 用户
本指南不适用于 Windows 用户。建议使用 Linux 虚拟机或 WSL2 来跟随本指南操作。
!!!

!!!info 致 Linux 用户
你需要具备以下知识：

- Linux 命令行操作
- DNS 记录
- 公网 IP 地址
- [Docker](https://www.docker.com)

!!!

**你需要自行购买一个域名，并为 SillyTavern 页面配置 `CNAME` 记录。我们建议通过 [Cloudflare](https://www.cloudflare.com) 添加或购买域名，本指南将以 Cloudflare 为例进行讲解。**

## 安装

### Linux（裸机安装 SillyTavern）

在 Linux 上，我们将通过 [Traefik](https://traefik.io/traefik/) 对 SillyTavern 进行反向代理。虽然也有 _NGINX_ 或 _Caddy_ 等其他选择，但本指南将使用 Traefik，因为这是我们自己在用的方案。

1. 使用 `ifconfig` 命令或从路由器管理页面获取本机的私网 IP 地址。
   !!!info 提示
   建议将私网 IP 设置为静态 IP。请参阅路由器手册或自行搜索如何配置静态 IP。
   !!!
2. 通过搜索 `what's my ip` 获取你的公网 IP 地址。
   !!!info 关于公网 IP
   大多数家用网络使用的是**动态 IP**，会在使用数月后更新。如果你的 IP 是动态的，可以使用 DDClient，或定期手动检查并在 Cloudflare 控制台更新公网 IP。
   !!!
3. 按照 Docker 安装指南[在此](https://docs.docker.com/engine/install/)安装 Docker。
   !!!danger 注意
   **不要**安装 Docker Desktop。
   !!!
4. 按照 Docker 安装后配置指南[在此](https://docs.docker.com/engine/install/linux-postinstall/)中的**以非 root 用户身份管理 Docker** 步骤进行操作。
5. 进入 Linux 根目录，创建名为 `docker` 的文件夹。
    ```sh
    cd /
    sudo mkdir docker && cd docker
    ```
6. 执行 `chown` 命令，将 _<USER>_ 替换为你的 Linux 用户名，以设置 docker 文件夹的权限。
    ```sh
    sudo chown -R <USER>:<USER> .
    ```
7. 在 _docker_ 文件夹中创建 `secrets` 子文件夹，并在 _secrets_ 内创建 `cloudflare` 子文件夹。
    ```sh
    mkdir secrets && mkdir secrets/cloudflare
    ```
8. 在 _docker_ 文件夹中创建 `appdata` 子文件夹，并在 _appdata_ 内创建 `traefik` 子文件夹，然后进入 `appdata/traefik`。
    ```sh
    mkdir appdata && mkdir appdata/traefik
    cd appdata/traefik
    ```
9. 使用 `touch` 创建 _acme.json_ 文件，并将其权限设置为 600。
    ```sh
    touch acme.json
    chmod 600 acme.json
    ```
10. 使用 `nano` 或其他编辑器，创建名为 _traefik.yml_ 的文件并粘贴以下内容。将示例邮箱替换为你自己的，然后保存文件。
    ```yml
    api:
        dashboard: true
        debug: true
        insecure: true
    entryPoints:
        http:
            address: ":80"
            http:
                redirections:
                    entryPoint:
                        to: https
                        scheme: https
        https:
            address: ":443"
    serversTransport:
        insecureSkipVerify: true
    providers:
        docker:
            endpoint: "unix:///var/run/docker.sock"
            exposedByDefault: false
        file:
            filename: /config.yml
            watch: true
    certificatesResolvers:
        cloudflare:
            acme:
                email: YOUR_CLOUDFLARE_EMAIL@DOMAIN.com
                storage: acme.json
                dnsChallenge:
                    provider: cloudflare
                    #disablePropagationCheck: true  # 如果通过 Cloudflare 拉取证书时遇到问题，请取消注释此行。设为 true 后无需等待 TXT 记录传播到所有权威名称服务器。
                    resolvers:
                        - "1.1.1.1:53"
                        - "1.0.0.1:53"
    ```
11. 返回 `docker` 文件夹。
    ```sh
    cd /docker
    ```
12. 使用 `nano` 或其他编辑器，创建名为 _docker-compose.yaml_ 的文件并粘贴以下内容，然后保存。

    ```yaml
    secrets:
        CF_DNS_API_KEY:
            file: ./secrets/cloudflare/CF_DNS_API_KEY

    services:
        traefik:
            image: traefik:latest
            container_name: traefik
            restart: unless-stopped
            secrets:
                - CF_DNS_API_KEY
            ports:
                - 80:80
                - 443:443
                - 8080:8080
            environment:
                CLOUDFLARE_DNS_API_TOKEN_FILE: /run/secrets/CF_DNS_API_KEY
                CLOUDFLARE_ZONE_API_TOKEN_FILE: /run/secrets/CF_DNS_API_KEY
            volumes:
                - /var/run/docker.sock:/var/run/docker.sock:ro
                - ./appdata/traefik/traefik.yml:/traefik.yml:ro
                - ./appdata/traefik/config.yml:/config.yml:ro
                - ./appdata/traefik/acme.json:/acme.json
                - /etc/localtime:/etc/localtime:ro

    networks:
        internal:
            driver: bridge
    ```

13. 登录 Cloudflare，点击你的域名，然后点击 **Get your API token**。
14. 点击 _Create Token_，再点击 _Create Custom Token_，确保为 token 赋予以下权限：
    !!!info Token 权限
    **Zone -> DNS -> Edit**

    **Zone -> Zone -> Read**
    !!!

    点击 _Continue to summary_，然后点击 _Create Token_。

15. 复制生成的 Token Key 并妥善保存。
16. `cd` 进入 `secrets/cloudflare`，使用 `nano` 或其他编辑器创建名为 **CF_DNS_API_KEY** 的文件，并将 Key 粘贴进去。
17. 返回域名页面，进入 **DNS**。点击 **Add record** 添加如下两条 _A_ 类型记录。将 `PUBLIC_IP` 替换为你的公网 IP，然后点击 _Save_。

    | Type | Name (required) | Target (required) | Proxy Status | TTL  |
    |------|-----------------|-------------------|--------------|------|
    | A    | DOMAIN.com      | PUBLIC_IP         | Proxied      | Auto |
    | A    | www             | PUBLIC_IP         | Proxied      | Auto |

18. 再添加一条 **`CNAME`** 类型的记录，然后点击 _Save_。以下是在 Cloudflare 控制台中的示例效果：

    | Type  | Name (required) | Target (required) | Proxy Status | TTL |
    |-------|-----------------|-------------------|--------------|-----|
    | CNAME | silly           | DOMAIN.com        | Proxied      | N/A |

19. `cd` 进入 _appdata/traefik_，使用 `nano` 或其他编辑器创建名为 _config.yml_ 的文件并粘贴以下内容。将 `PRIVATE_IP` 替换为你之前获取的私网 IP，将 `silly.DOMAIN.com` 替换为你的子域名和域名，然后保存。

    ```yml
    http:
        routers:
            sillytavern:
                entryPoints:
                    - "https"
                rule: "Host(`silly.DOMAIN.com`)"
                middlewares:
                    - https-redirectscheme
                tls: {}
                service: sillytavern

        services:
            sillytavern:
                loadBalancer:
                    servers:
                        - url: "http://PRIVATE_IP:8000"
                    passHostHeader: true

        middlewares:
            https-redirectscheme:
                redirectScheme:
                    scheme: https
                    permanent: true
    ```

20. 使用以下命令运行 Docker Compose：
    ```sh
    cd /docker
    docker compose up -d
    ```
21. 进入 SillyTavern 文件夹，编辑 `config.yaml`，启用监听模式和基本身份验证，同时禁用 `whitelistMode`。

    ```yaml
    listen: yes
    whitelistMode: false
    basicAuthMode: true
    ```

    !!!warning 提示
    请务必将默认用户名和密码修改为强密码并牢记。
    !!!

    或者，使用 SillyTavern 账户作为用户名和密码：

    ```yaml
    basicAuthMode: true
    enableUserAccounts: true
    perUserBasicAuth: true
    ```

    !!!warning 提示
    启用 perUserBasicAuth 之前，请确保你已正确配置多用户模式并设置了有效密码。
    !!!

22. 等待几分钟后，打开你为 ST 创建的域名页面。最终，你将能够通过一个 URL 和一个账户从任何地方访问 SillyTavern。
    !!!info 提示
    若等待数分钟后仍无响应，请查看 Traefik 容器日志以排查错误。
    !!!
23. 尽情享用吧！:D

### Linux（Docker 安装 SillyTavern）

!!!warning 注意
请注意，我们自己运行的是裸机安装而非 Docker 安装。以下是我们在 Docker 环境下配合其他容器使用 ST 的大致方案。
!!!

1. 按照**Linux（裸机安装 SillyTavern）**的第 1-11 步操作。
2. 登录 Cloudflare，点击你的域名，然后点击 **Get your API token**。
3. 点击 _Create Token_，再点击 _Create Custom Token_，确保为 token 赋予以下权限：
   !!!info Token 权限
   **Zone -> DNS -> Edit**

    **Zone -> Zone -> Read**
    !!!

    点击 _Continue to summary_，然后点击 _Create Token_。

4. 复制生成的 Token Key 并妥善保存。
5. `cd` 进入 `secrets/cloudflare`，使用 `nano` 或其他编辑器创建名为 **CF_DNS_API_KEY** 的文件，并将 Key 粘贴进去。
6. 返回域名页面，进入 **DNS**。点击 **Add record** 添加如下两条 _A_ 类型记录。将 `PUBLIC_IP` 替换为你的公网 IP，将示例域名替换为你的域名，然后点击 _Save_。

    | Type | Name (required) | Target (required) | Proxy Status | TTL  |
    |------|-----------------|-------------------|--------------|------|
    | A    | DOMAIN.com      | PUBLIC_IP         | Proxied      | Auto |
    | A    | www             | PUBLIC_IP         | Proxied      | Auto |

7. 再添加一条 **`CNAME`** 类型的记录，然后点击 _Save_。以下是在 Cloudflare 控制台中的示例效果：

    | Type  | Name (required) | Target (required) | Proxy Status | TTL |
    |-------|-----------------|-------------------|--------------|-----|
    | CNAME | silly           | DOMAIN.com        | Proxied      | N/A |

8. 将 SillyTavern 克隆到 `docker` 文件夹中。
    ```sh
    cd /docker && git clone https://github.com/SillyTavern/SillyTavern
    ```
9. 使用 `nano` 或其他编辑器，创建名为 _docker-compose.yaml_ 的文件并粘贴以下内容。将 `silly.DOMAIN.com` 替换为你在上方添加的子域名，然后保存。

    ```yaml
    secrets:
        CF_DNS_API_KEY:
            file: ./secrets/cloudflare/CF_DNS_API_KEY

    services:
        traefik:
            image: traefik:latest
            container_name: traefik
            restart: unless-stopped
            secrets:
                - CF_DNS_API_KEY
            ports:
                - "80:80"
                - 443:443
                - 8080:8080
            environment:
                CLOUDFLARE_DNS_API_TOKEN_FILE: /run/secrets/CF_DNS_API_KEY
                CLOUDFLARE_ZONE_API_TOKEN_FILE: /run/secrets/CF_DNS_API_KEY
            volumes:
                - /var/run/docker.sock:/var/run/docker.sock:ro
                - ./appdata/traefik/traefik.yml:/traefik.yml:ro
                - ./appdata/traefik/config.yml:/config.yml:ro
                - ./appdata/traefik/acme.json:/acme.json
                - /etc/localtime:/etc/localtime:ro
        sillytavern:
            build: ./SillyTavern
            container_name: sillytavern
            hostname: sillytavern
            image: ghcr.io/sillytavern/sillytavern:latest
            volumes:
                - "./appdata/sillytavern/config:/home/node/app/config"
                - "./appdata/sillytavern/data:/home/node/app/data"
            restart: unless-stopped
            labels:
                - "traefik.enable=true"
                - "traefik.http.routers.sillytavern.entrypoints=http"
                - "traefik.http.routers.sillytavern.rule=Host(`silly.DOMAIN.com`)"
                - "traefik.http.middlewares.sillytavern-https-redirect.redirectscheme.scheme=https"
                - "traefik.http.routers.sillytavern.middlewares=sillytavern-https-redirect"
                - "traefik.http.routers.sillytavern-secure.entrypoints=https"
                - "traefik.http.routers.sillytavern-secure.rule=Host(`silly.DOMAIN.com`)"
                - "traefik.http.routers.sillytavern-secure.tls=true"
                - "traefik.http.routers.sillytavern-secure.service=sillytavern"
                - "traefik.http.services.sillytavern.loadbalancer.server.port=8000"

    networks:
        internal:
            driver: bridge
    ```

10. 使用以下命令运行 Docker Compose：
    ```sh
    docker compose up -d
    ```
11. 停止 SillyTavern Docker 容器。
    ```sh
    docker compose stop sillytavern
    ```
12. 进入 SillyTavern 文件夹（`appdata/sillytavern/config`），编辑 `config.yaml`，启用监听模式和基本身份验证，同时禁用 `whitelistMode`。

    ```yaml
    listen: yes
    whitelistMode: false
    basicAuthMode: true
    ```

    !!!warning 提示
    请务必将默认用户名和密码修改为强密码并牢记。
    !!!

13. 重新启动 SillyTavern Docker 容器。
    ```sh
    docker compose up -d sillytavern
    ```
14. 等待几分钟后，打开你为 ST 创建的域名页面。最终，你将能够通过一个 URL 和一个账户从任何地方访问 SillyTavern。
    !!!info 提示
    若等待数分钟后仍无响应，请查看 Traefik 容器日志以排查错误。
    !!!
15. 尽情享用吧！:D

## 更新 Cloudflare DNS

[**DDClient**](https://ddclient.net/) 可以在 ISP 更改你的公网 IP 时自动将新 IP 同步到 Cloudflare，让你无感知地继续访问 ST 实例。
