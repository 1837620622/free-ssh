# Cloudflare Tunnel 部署 SSH 服务

> Cloudflare Tunnel 通过 cloudflared 客户端将内网服务安全暴露到公网。

## 平台信息

| 项目 | 说明 |
|------|------|
| 官网 | https://www.cloudflare.com |
| 镜像 | `ghcr.io/vevc/ubuntu:25.7.14` |
| 端口 | 22 |
| 难度 | ⭐⭐⭐ 较复杂 |

## 前提条件

- Cloudflare 账号
- 域名已托管在 Cloudflare

## 方法一：本地服务器 + Tunnel

### 1. 安装 cloudflared

**macOS:**
```bash
brew install cloudflared
```

**Ubuntu/Debian:**
```bash
curl -L https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64.deb -o cloudflared.deb
sudo dpkg -i cloudflared.deb
```

### 2. 登录 Cloudflare

```bash
cloudflared tunnel login
```

### 3. 创建 Tunnel

```bash
cloudflared tunnel create ssh-tunnel
```

### 4. 配置 DNS

```bash
cloudflared tunnel route dns ssh-tunnel ssh.yourdomain.com
```

### 5. 创建配置文件

创建 `~/.cloudflared/config.yml`:

```yaml
tunnel: <TUNNEL_ID>
credentials-file: /home/user/.cloudflared/<TUNNEL_ID>.json

ingress:
  - hostname: ssh.yourdomain.com
    service: ssh://localhost:22
  - service: http_status:404
```

### 6. 启动 Tunnel

```bash
cloudflared tunnel run ssh-tunnel
```

### 7. 客户端连接

**方式一：使用 cloudflared 代理**
```bash
cloudflared access ssh --hostname ssh.yourdomain.com
```

**方式二：配置 SSH config**
```bash
cloudflared access ssh-config --hostname ssh.yourdomain.com >> ~/.ssh/config
ssh user@ssh.yourdomain.com
```

## 方法二：Docker + Tunnel

### 1. 创建 docker-compose.yml

```yaml
version: '3'
services:
  ssh-server:
    image: ghcr.io/vevc/ubuntu:25.7.14
    container_name: ssh-server
    environment:
      - ROOT_PASSWORD=${ROOT_PASSWORD}
    command: >
      /bin/bash -c "
        echo root:$$ROOT_PASSWORD | chpasswd;
        sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config;
        mkdir -p /var/run/sshd;
        /usr/sbin/sshd -D
      "
    restart: unless-stopped
    networks:
      - tunnel-net

  cloudflared:
    image: cloudflare/cloudflared:latest
    container_name: cloudflared
    command: tunnel --config /etc/cloudflared/config.yml run
    volumes:
      - ./cloudflared:/etc/cloudflared
    depends_on:
      - ssh-server
    restart: unless-stopped
    networks:
      - tunnel-net

networks:
  tunnel-net:
```

### 2. 创建 .env 文件

```env
ROOT_PASSWORD=你的密码
```

### 3. 配置 cloudflared

创建 `cloudflared/config.yml`:

```yaml
tunnel: <TUNNEL_ID>
credentials-file: /etc/cloudflared/credentials.json

ingress:
  - hostname: ssh.yourdomain.com
    service: ssh://ssh-server:22
  - service: http_status:404
```

### 4. 启动服务

```bash
docker-compose up -d
```

## 特点

- ✅ 无需开放防火墙端口
- ✅ 自动 SSL/TLS 加密
- ✅ 支持 Zero Trust 身份验证
- ✅ 全球 CDN 加速
- ⚠️ 需要拥有域名

## 常用命令

```bash
cloudflared tunnel login           # 登录
cloudflared tunnel create <name>   # 创建 Tunnel
cloudflared tunnel list            # 列出 Tunnel
cloudflared tunnel run <name>      # 运行 Tunnel
cloudflared tunnel delete <name>   # 删除 Tunnel
```
