---
label: 反向代理
order: -50
icon: server
route: /usage/st-reverse-proxy-guide/
---


!!! danger 注意
本节**不**涉及 OpenAI/Claude 反向代理。这仅涉及 **HTTP/HTTPS 反向代理**。
!!!

Termux 的设置让您感到困惑吗？您是否厌倦了在每个设备上更新和安装 ST？想要组织您的聊天和角色吗？好消息是，您很幸运。本指南将_希望_涵盖如何在您的 PC 上托管 SillyTavern，让您可以从任何地方连接，并在同一台用于运行 AI 模型的 PC 上与您的机器人聊天！

!!! warning 警告
本指南**不适合**初学者。这将非常技术性。
!!!

## 公平警告

!!! info 对于 Windows 用户
本指南不适用于 Windows 用户。我们建议使用 Linux 虚拟机或 WSL2 来按照本指南操作。
!!!

!!! info 对于 Linux 用户
您必须具备以下先验知识：

-   Linux 控制台命令
-   DNS 记录
-   公共 IP 地址
-   [Docker](https://www.docker.com)

!!!

**您需要为自己购买一个域名，并为您的 SillyTavern 页面配置一个 `CNAME`。我们建议在 [Cloudflare](https://www.cloudflare.com) 上添加或购买域名，因为本指南将介绍如何使用 Cloudflare 本身来完成这项工作。**

## 安装

### Linux（裸机 SillyTavern）

对于 Linux，我们将通过 [Traefik](https://traefik.io/traefik/) 反向代理 SillyTavern。还有其他选项，如 _NGINX_ 或 _Caddy_，但在本指南中，我们将使用 Traefik，因为这是我们自己使用的。

1. 使用 `ifconfig` 或从路由器获取计算机的私有 IP。
   !!! info 提示
   建议将您的私有 IP 设置为静态 IP。请参考您的路由器手册或 Google 来配置静态 IP。
   !!!
2. 通过 Google 搜索 `what's my ip` 获取调制解调器的公共 IP。
   !!! info 关于公共 IP
   大多数住宅/家庭网络使用**动态 IP**，这些 IP 在使用数月后会更新。如果您有动态 IP，请使用 DDClient 或记得经常在 Cloudflare 仪表板上检查和更改您的公共 IP。
   !!!
3. 按照[此处](https://docs.docker.com/engine/install/)的 Docker 安装指南安装 Docker。
   !!! danger 注意
   **不要**安装 Docker Desktop。
   !!!
4. 按照[此处](https://docs.docker.com/engine/install/linux-postinstall/)的 Docker 安装后指南中的**以非 root 用户身份管理 Docker**步骤进行操作。
5. 进入 Linux 的根文件夹并创建一个名为 `docker` 的新文件夹。
    ```sh
    cd /
    sudo mkdir docker && cd docker
    ```
6. 执行 `chown`，将 _<USER>_ 替换为您的 Linux 用户名，以设置 docker 文件夹中的权限。
    ```sh
    sudo chown -R <USER>:<USER> .
    ```
7. 在 _docker_ 文件夹内创建一个名为 `secrets` 的文件夹，在 _secrets_ 内创建 `cloudflare`。
    ```sh
    mkdir secrets && mkdir secrets/cloudflare
    ```
8. 在 _docker_ 文件夹内创建一个名为 `appdata` 的文件夹，在 _appdata_ 内创建 `traefik`。之后进入 `appdata/traefik` 文件夹。
    ```sh
    mkdir appdata && mkdir appdata/traefik
    cd appdata/traefik
    ```
9. 使用 `touch` 创建一个 _acme.json_ 文件，并将其权限设置为 600。
    ```sh
    touch acme.json
    chmod 600 acme.json
    ```
10. 使用 `nano` 或类似的编辑器，创建一个名为 _traefik.yml_ 的文件并粘贴以下内容。将模板电子邮件替换为您自己的电子邮件，然后保存文件。
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
                    #disablePropagationCheck: true  # 如果您通过 cloudflare 拉取证书时遇到问题，请取消注释此行。将此标志设置为 true 会禁用等待 TXT 记录传播到所有权威名称服务器的需求。
                    resolvers:
                        - "1.1.1.1:53"
                        - "1.0.0.1:53"
    ```
11. 返回到 `docker` 文件夹。
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

13. 登录 Cloudflare 并点击您的域名，然后点击 **Get your API token**。
14. 点击 _Create Token_，然后点击 _Create Custom Token_，确保您给予令牌以下权限。
    !!! info 令牌权限
    **Zone -> DNS -> Edit**

    **Zone -> Zone -> Read**
    !!!

    点击 _Continue to summary_，然后点击 _Create Token_。

15. 复制给您的令牌密钥并将其存储在安全的地方。
16. `cd` 进入 `secrets/cloudflare`，使用 `nano` 或类似的编辑器，创建一个名为 **CF_DNS_API_KEY** 的文件并在其中粘贴您的密钥。
17. 返回到您的域名页面并转到 **DNS**。使用 **Add record** 创建新记录，并创建两个如下所示的 _A_ 类型密钥。将 `PUBLIC_IP` 替换为您自己的公共 IP，然后点击 _Save_。

    | 类型 | 名称（必填） | 目标（必填） | 代理状态 | TTL  |
    |------|------------|-------------|----------|------|
    | A    | DOMAIN.com | PUBLIC_IP   | 已代理   | 自动 |
    | A    | www        | PUBLIC_IP   | 已代理   | 自动 |

18. 创建另一个 **`CNAME`** 类型的记录，然后点击 _Save_。以下是它在 Cloudflare 仪表板上应该显示的示例。

    | 类型  | 名称（必填） | 目标（必填） | 代理状态 | TTL |
    |-------|------------|-------------|----------|-----|
    | CNAME | silly      | DOMAIN.com  | 已代理   | N/A |

19. `cd` 进入 _appdata/traefik_，使用 `nano` 或类似的编辑器，创建一个名为 _config.yml_ 的文件并粘贴以下内容。将 `PRIVATE_IP` 替换为您获得的私有 IP，将 `silly.DOMAIN.com` 替换为您的子域名和域名页面的名称，然后保存文件。

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
21. 进入您的 SillyTavern 文件夹并编辑 `config.yaml` 以启用监听模式和基本认证，同时禁用 `whitelistMode`。

    ```yaml
    listen: yes
    whitelistMode: false
    basicAuthMode: true
    ```

    !!! warning 提示
    确保将默认用户名和密码更改为您能记住的强密码。
    !!!

    或者使用 SillyTavern 账户作为用户名和密码：

    ```yaml
    basicAuthMode: true
    enableUserAccounts: true
    perUserBasicAuth: true
    ```

    !!! warning 提示
    在启用 perUserBasicAuth 之前，确保您有一个带有有效密码的多用户设置。
    !!!

22. 等待几分钟，然后打开您为 ST 创建的域名页面。最后，您应该能够从任何地方只使用一个 URL 和一个账户就能打开 SillyTavern。
    !!! info 提示
    如果几分钟后什么都没有发生，请检查 Traefik 的容器日志是否有任何可能的错误。
    !!!
23. 尽情享受！:D

### Linux（Docker SillyTavern）

!!! warning 注意
请注意，我们在裸机上运行 SillyTavern 而不是 Docker。这是我们在 Docker 上使用其他 Docker 容器与 ST 一起使用时的粗略想法。
!!!

1. 按照 **Linux（裸机 SillyTavern）**的步骤 1-11 操作。
2. 登录 Cloudflare 并点击您的域名，然后点击 **Get your API token**。
3. 点击 _Create Token_，然后点击 _Create Custom Token_，确保您给予令牌以下权限。
   !!! info 令牌权限
   **Zone -> DNS -> Edit**

    **Zone -> Zone -> Read**
    !!!

    Click on _Continue to summary_ followed by _Create Token._

4. Copy the Token Key given to you and store it somewhere secure.
5. `cd` into `secrets/cloudflare` and using `nano` or a similar editor, create a file named **CF_DNS_API_KEY** and paste your key inside.
6. Return to your domain page and go to **DNS**. Create a new record using **Add record** and create two _A_ type keys like the ones below. Replace `PUBLIC_IP` with your own public IP and the example domain with your domain, then click _Save_.

    | Type | Name (required) | Target (required) | Proxy Status | TTL  |
    |------|-----------------|-------------------|--------------|------|
    | A    | DOMAIN.com      | PUBLIC_IP         | Proxied      | Auto |
    | A    | www             | PUBLIC_IP         | Proxied      | Auto |

7. Create another record of the **`CNAME`** type, then click _Save_. Here is an example on how it should appear on the Cloudflare dashboard.

    | Type  | Name (required) | Target (required) | Proxy Status | TTL |
    |-------|-----------------|-------------------|--------------|-----|
    | CNAME | silly           | DOMAIN.com        | Proxied      | N/A |

8. Git clone SillyTavern into the `docker` folder.
    ```sh
    cd /docker && git clone https://github.com/SillyTavern/SillyTavern
    ```
9. Using `nano` or a similar editor, create a file name _docker-compose.yaml_ and paste the following. Replace `silly.DOMAIN.com` with the subdomain you added above, the save the file afterwards.

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

10. Run Docker Compose using the following commands:
    ```sh
    docker compose up -d
    ```
11. Stop the SillyTavern Docker container.
    ```sh
    docker compose stop sillytavern
    ```
12. Go to your SillyTavern folder (`appdata/sillytavern/config`) and edit `config.yaml` to enable listen mode and basic authentication, whilst disabling `whitelistMode`.

    ```yaml
    listen: yes
    whitelistMode: false
    basicAuthMode: true
    ```

    !!! warning Tip
    Make sure to change the default username and password to something strong that you can remember.
    !!!

13. Start the SillyTavern Docker container again.
    ```sh
    docker compose up -d sillytavern
    ```
14. Wait a few minutes, then open your domain page you made for ST. At the end of it, you should be able to open SillyTavern from anywhere you go just with one URL and one account.
    !!! info Tip
    If nothing happens after several minutes, check the container logs for Traefik for any possible errors.
    !!!
15. Enjoy! :D

## Updating your Cloudflare DNS

[**DDClient**](https://ddclient.net/) allows you to sync your public IP to Cloudflare in the situation that your ISP changes it, allowing you to continue accessing your ST instance as if nothing ever happened.
