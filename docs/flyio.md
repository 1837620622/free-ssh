# Fly.io 部署 SSH 服务

> Fly.io 是全球分布式应用部署平台，支持将应用部署到离用户最近的区域。

## 平台信息

| 项目 | 说明 |
|------|------|
| 官网 | https://fly.io |
| 镜像 | `ghcr.io/vevc/ubuntu:25.7.14` |
| 端口 | 22 |
| 难度 | ⭐⭐ 中等 |

## 部署步骤

### 1. 安装 flyctl

**macOS:**
```bash
brew install flyctl
```

**Linux:**
```bash
curl -L https://fly.io/install.sh | sh
```

**Windows:**
```powershell
powershell -Command "iwr https://fly.io/install.ps1 -useb | iex"
```

### 2. 登录

```bash
fly auth login
```

### 3. 创建项目目录

```bash
mkdir fly-ssh && cd fly-ssh
```

### 4. 创建 Dockerfile

```dockerfile
FROM ghcr.io/vevc/ubuntu:25.7.14

# 配置 SSH
RUN mkdir -p /var/run/sshd && \
    sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config && \
    sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config

# 创建启动脚本
RUN echo '#!/bin/bash\n\
echo "root:$ROOT_PASSWORD" | chpasswd\n\
exec /usr/sbin/sshd -D\n' > /entrypoint.sh && \
    chmod +x /entrypoint.sh

EXPOSE 22

ENTRYPOINT ["/entrypoint.sh"]
```

### 5. 创建 fly.toml

```toml
app = "your-app-name"
primary_region = "hkg"

[build]
  dockerfile = "Dockerfile"

[env]
  # ROOT_PASSWORD 通过 secrets 设置

[[services]]
  internal_port = 22
  protocol = "tcp"

  [[services.ports]]
    port = 22

[[services.tcp_checks]]
  interval = "10s"
  timeout = "2s"
```

### 6. 创建应用

```bash
fly launch --no-deploy
```

选择：
- 应用名称
- 区域（推荐 `hkg` 香港或 `nrt` 东京）

### 7. 设置密码（Secrets）

```bash
fly secrets set ROOT_PASSWORD=你的密码
```

### 8. 部署

```bash
fly deploy
```

### 9. 分配 IP

```bash
# 分配 IPv4（可能需要付费）
fly ips allocate-v4

# 或使用免费的 IPv6
fly ips allocate-v6
```

### 10. 连接 SSH

```bash
# 查看分配的 IP
fly ips list

# 连接
ssh root@<分配的IP> -p 22
```

## 使用 fly ssh 直连

Fly.io 提供内置 SSH 功能：

```bash
# 直接连接到应用
fly ssh console

# 以 root 身份连接
fly ssh console -u root
```

## 特点

- ✅ 全球分布式部署
- ✅ 内置 SSH 功能
- ✅ 按使用量计费
- ✅ 支持 IPv6
- ⚠️ IPv4 可能需要付费

## 常用命令

```bash
fly auth login       # 登录
fly launch           # 创建应用
fly deploy           # 部署
fly logs             # 查看日志
fly ssh console      # SSH 连接
fly ips list         # 查看 IP
fly secrets set      # 设置密钥
fly status           # 查看状态
```

## 区域选择

| 区域代码 | 位置 |
|---------|------|
| hkg | 香港 |
| nrt | 东京 |
| sin | 新加坡 |
| syd | 悉尼 |
| lax | 洛杉矶 |
| ord | 芝加哥 |
| iad | 华盛顿 |
| ams | 阿姆斯特丹 |
| lhr | 伦敦 |
