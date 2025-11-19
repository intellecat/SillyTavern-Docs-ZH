---
label: Reverse proxying
order: -50
icon: server
route: /usage/st-reverse-proxy-guide/
---

!!!danger 注意
本节**不**涉及 OpenAI/Claude 反向代理。这专门指的是 **HTTP/HTTPS 反向代理**。
!!!

Termux 设置起来令人困惑吗？您是否厌倦了在您拥有的每台设备上更新和安装 ST？想要组织您的聊天和角色？那么您很幸运。本指南将_希望_涵盖如何在您的 PC 上托管 SillyTavern，您可以从任何地方连接并在用于运行 AI 模型的同一台 PC 上与您的机器人聊天！

!!!warning 警告
本指南**不适合**初学者。这将非常技术性。
!!!

## 公平警告

!!!info 对于 Windows 用户
本指南不适用于 Windows 用户。我们建议使用 Linux VM 或 WSL2 来遵循本指南。
!!!

!!!info 对于 Linux 用户
您必须具有以下方面的先验知识

- Linux 控制台命令
- DNS 记录
- 公共 IP 地址
- [Docker](https://www.docker.com)

!!!

**您必须为自己购买一个域并为您的 SillyTavern 页面配置 `CNAME`。我们建议在 [Cloudflare](https://www.cloudflare.com) 上添加或购买域，因为本指南将介绍如何使用 Cloudflare 本身执行此操作。**

## 安装

### Linux（裸机 SillyTavern）

对于 Linux，我们将通过 [Traefik](https://traefik.io/traefik/) 反向代理 SillyTavern。还有其他选项，如 _NGINX_ 或 _Caddy_，但对于本指南，我们将使用 Traefik，因为这是我们自己使用的。

1. 使用 `ifconfig` 或从路由器获取计算机的私有 IP。
   !!!info 提示
   建议将您的私有 IP 设置为静态 IP。请参阅路由器的手册或 Google 以配置静态 IP。
   !!!
2. 通过 Google `what's my ip` 获取调制解调器的公共 IP。
   !!!info 关于公共 IP
   大多数住宅/家庭网络使用**动态 IP**，这些 IP 在使用数月后会更新。如果您有动态 IP，请使用 DDClient 或记住定期在 Cloudflare 仪表板上检查和更改您的公共 IP。
   !!!
3. 按照 Docker 安装指南[此处](https://docs.docker.com/engine/install/)安装 Docker。
   !!!danger 注意
   **不要**安装 Docker Desktop。
   !!!
4. 按照 Docker 安装后指南[此处](https://docs.docker.com/engine/install/linux-postinstall/)中的**将 Docker 作为非 root 用户管理**中的步骤操作。
5. 转到 Linux 中的根文件夹并创建一个名为 `docker` 的新文件夹。
    ```sh
    cd /
    sudo mkdir docker && cd docker
    ```
6. 执行 `chown`，将 _<USER>_ 替换为您的 Linux 用户名以设置 docker 文件夹中的权限。
    ```sh
    sudo chown -R <USER>:<USER> .
    ```
7. 在 _docker_ 文件夹内创建一个文件夹，即 `secrets`，在 _secrets_ 内创建 `cloudflare`。
    ```sh
    mkdir secrets && mkdir secrets/cloudflare
    ```
8. 在 _docker_ 文件夹内创建一个文件夹，即 `appdata`，在 _appdata_ 内创建 `traefik`。之后进入 `appdata/traefik` 文件夹。
    ```sh
    mkdir appdata && mkdir appdata/traefik
    cd appdata/traefik
    ```
9. 使用 `touch` 创建一个 _acme.json_ 文件并将其权限设置为 600。
    ```sh
    touch acme.json
    chmod 600 acme.json
    ```
10. 使用 `nano` 或类似的编辑器，创建一个名为 _traefik.yml_ 的文件并粘贴以下内容。用您自己的电子邮件替换模板电子邮件，然后保存文件。
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
                email: YOUR_CLOUDFLARE_EMAL@DOMAIN.com
                storage: acme.json
                dnsChallenge:
                    provider: cloudflare
                    #disablePropagationCheck: true  # uncomment this if you have issues pulling certificates through cloudflare, By setting this flag to true disables the need to wait for the propagation of the TXT record to all authoritative name servers.
                    resolvers:
                        - "1.1.1.1:53"
                        - "1.0.0.1:53"
    ```
11. 返回 `docker` 文件夹。
    ```sh
    cd /docker
    ```
12. 使用 `nano` 或类似的编辑器，创建一个名为 _docker-compose.yaml_ 的文件并粘贴以下内容。之后保存文件。

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

13. 登录 Cloudflare 并点击您的域，然后点击 **Get your API token**。
14. 点击 _Create Token_，然后点击 _Create Custom Token_，确保您为令牌授予以下权限。
    !!!info 令牌权限
    **Zone -> DNS -> Edit**

    **Zone -> Zone -> Read**
    !!!

    点击 _Continue to summary_，然后点击 _Create Token._

15. 复制提供给您的令牌密钥并将其存储在安全的地方。
16. `cd` 进入 `secrets/cloudflare`，使用 `nano` 或类似的编辑器，创建一个名为 **CF_DNS_API_KEY** 的文件并将您的密钥粘贴进去。
17. 返回您的域页面并转到 **DNS**。使用 **Add record** 创建一个新记录，并创建两个如下所示的 _A_ 类型键。用您自己的公共 IP 替换 `PUBLIC_IP`，然后点击 _Save_。

    | Type | Name (required) | Target (required) | Proxy Status | TTL  |
    |------|-----------------|-------------------|--------------|------|
    | A    | DOMAIN.com      | PUBLIC_IP         | Proxied      | Auto |
    | A    | www             | PUBLIC_IP         | Proxied      | Auto |

18. 创建另一个 **`CNAME`** 类型的记录，然后点击 _Save_。以下是它在 Cloudflare 仪表板上的显示示例。

    | Type  | Name (required) | Target (required) | Proxy Status | TTL |
    |-------|-----------------|-------------------|--------------|-----|
    | CNAME | silly           | DOMAIN.com        | Proxied      | N/A |

19. `cd` 进入 _appdata/traefik_，使用 `nano` 或类似的编辑器，创建一个名为 _config.yml_ 的文件并粘贴以下内容。用您获得的私有 IP 替换 `PRIVATE_IP`，用您的子域和域页面的名称替换 `silly.DOMAIN.com`，然后保存文件。

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
21. 转到您的 SillyTavern 文件夹并编辑 `config.yaml` 以启用侦听模式和基本身份验证，同时禁用 `whitelistMode`。

    ```yaml
    listen: yes
    whitelistMode: false
    basicAuthMode: true
    ```

    !!!warning 提示
    确保将默认用户名和密码更改为您可以记住的强密码。
    !!!

    或者使用 SillyTavern 账户作为用户名和密码：

    ```yaml
    basicAuthMode: true
    enableUserAccounts: true
    perUserBasicAuth: true
    ```

    !!!warning 提示
    在启用 perUserBasicAuth 之前，请确保您有一个有效的多用户设置，并且密码正常工作。
    !!!

22. 等待几分钟，然后打开您为 ST 制作的域页面。最后，您应该能够从任何地方仅使用一个 URL 和一个账户打开 SillyTavern。
    !!!info 提示
    如果几分钟后什么都没有发生，请检查 Traefik 的容器日志以查找任何可能的错误。
    !!!
23. 享受！:D

### Linux（Docker SillyTavern）

!!!warning 注意
请注意，我们在裸机上运行 SillyTavern 而不是 Docker。这是我们在 Docker 上与我们倾向于与 ST 一起使用的其他 Docker 容器一起执行的大致想法。
!!!

1. 遵循 **Linux（裸机 SillyTavern）**的步骤 1-11。
2. 登录 Cloudflare 并点击您的域，然后点击 **Get your API token**。
3. 点击 _Create Token_，然后点击 _Create Custom Token_，确保您为令牌授予以下权限。
   !!!info 令牌权限
   **Zone -> DNS -> Edit**

    **Zone -> Zone -> Read**
    !!!

    点击 _Continue to summary_，然后点击 _Create Token._

4. 复制提供给您的令牌密钥并将其存储在安全的地方。
5. `cd` 进入 `secrets/cloudflare`，使用 `nano` 或类似的编辑器，创建一个名为 **CF_DNS_API_KEY** 的文件并将您的密钥粘贴进去。
6. 返回您的域页面并转到 **DNS**。使用 **Add record** 创建一个新记录，并创建两个如下所示的 _A_ 类型键。用您自己的公共 IP 和示例域替换 `PUBLIC_IP`，然后点击 _Save_。

    | Type | Name (required) | Target (required) | Proxy Status | TTL  |
    |------|-----------------|-------------------|--------------|------|
    | A    | DOMAIN.com      | PUBLIC_IP         | Proxied      | Auto |
    | A    | www             | PUBLIC_IP         | Proxied      | Auto |

7. 创建另一个 **`CNAME`** 类型的记录，然后点击 _Save_。以下是它在 Cloudflare 仪表板上的显示示例。

    | Type  | Name (required) | Target (required) | Proxy Status | TTL |
    |-------|-----------------|-------------------|--------------|-----|
    | CNAME | silly           | DOMAIN.com        | Proxied      | N/A |

8. 将 SillyTavern git clone 到 `docker` 文件夹中。
    ```sh
    cd /docker && git clone https://github.com/SillyTavern/SillyTavern
    ```
9. 使用 `nano` 或类似的编辑器，创建一个名为 _docker-compose.yaml_ 的文件并粘贴以下内容。用您上面添加的子域替换 `silly.DOMAIN.com`，然后保存文件。

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
12. 转到您的 SillyTavern 文件夹（`appdata/sillytavern/config`）并编辑 `config.yaml` 以启用侦听模式和基本身份验证，同时禁用 `whitelistMode`。

    ```yaml
    listen: yes
    whitelistMode: false
    basicAuthMode: true
    ```

    !!!warning 提示
    确保将默认用户名和密码更改为您可以记住的强密码。
    !!!

13. 再次启动 SillyTavern Docker 容器。
    ```sh
    docker compose up -d sillytavern
    ```
14. 等待几分钟，然后打开您为 ST 制作的域页面。最后，您应该能够从任何地方仅使用一个 URL 和一个账户打开 SillyTavern。
    !!!info 提示
    如果几分钟后什么都没有发生，请检查 Traefik 的容器日志以查找任何可能的错误。
    !!!
15. 享受！:D

## 更新您的 Cloudflare DNS

[**DDClient**](https://ddclient.net/) 允许您在 ISP 更改您的公共 IP 时将其同步到 Cloudflare，使您能够继续访问您的 ST 实例，就像什么都没有发生一样。
