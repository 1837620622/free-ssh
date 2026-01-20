# Railway 部署 SSH 服务

> Railway 是现代化云基础设施平台，支持快速部署应用程序。

## 平台信息

| 项目 | 说明 |
|------|------|
| 官网 | https://railway.app |
| 镜像 | `ghcr.io/vevc/ubuntu:25.7.14` |
| 端口 | 22 |
| 难度 | ⭐⭐ 中等 |

## 方法一：网页控制台部署

### 1. 登录控制台

访问 [Railway](https://railway.app)，使用 GitHub 账号登录。

### 2. 创建项目

1. 点击 **New Project**
2. 选择 **Empty Project**

### 3. 添加 Docker 服务

1. 点击 **New Service** → **Docker Image**
2. 输入镜像地址：
   ```
   ghcr.io/vevc/ubuntu:25.7.14
   ```
3. 点击 **Deploy**

### 4. 配置启动命令

1. 进入服务 **Settings** → **Deploy**
2. 找到 **Start Command**
3. 填入：

```bash
/bin/sh -c "echo root:$ROOT_PASSWORD | chpasswd; sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config; mkdir -p /var/run/sshd; exec /usr/sbin/sshd -D"
```

> ⚠️ Railway 要求使用 `/bin/sh -c` 包装命令

### 5. 配置环境变量

在 **Variables** 页面添加：

| 变量名 | 值 |
|--------|-----|
| ROOT_PASSWORD | 你的密码 |

### 6. 配置 TCP 代理

1. 进入 **Settings** → **Networking**
2. 找到 **TCP Proxy**
3. 点击 **Enable TCP Proxy**
4. 设置内部端口为 `22`
5. 记录分配的外部地址和端口

### 7. 连接 SSH

```bash
ssh root@<分配的域名> -p <分配的端口>
```

## 方法二：CLI 部署

### 1. 安装 CLI

```bash
npm i -g @railway/cli
# 或
brew install railway
```

### 2. 登录

```bash
railway login
```

### 3. 创建项目文件

**Dockerfile:**
```dockerfile
FROM ghcr.io/vevc/ubuntu:25.7.14

RUN mkdir -p /var/run/sshd && \
    sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config

EXPOSE 22

CMD ["/bin/sh", "-c", "echo root:$ROOT_PASSWORD | chpasswd && /usr/sbin/sshd -D"]
```

**railway.toml:**
```toml
[build]
builder = "dockerfile"

[deploy]
startCommand = "/bin/sh -c 'echo root:$ROOT_PASSWORD | chpasswd && /usr/sbin/sshd -D'"
```

### 4. 部署

```bash
railway init
railway up
```

### 5. 配置环境变量和 TCP 代理

```bash
railway variables set ROOT_PASSWORD=你的密码
railway open  # 打开控制台配置 TCP Proxy
```

## 特点

- ✅ GitHub 集成，自动部署
- ✅ 内置数据库服务
- ✅ CLI 工具强大
- ⚠️ 免费额度有限

## 常用命令

```bash
railway login      # 登录
railway init       # 初始化
railway up         # 部署
railway logs       # 查看日志
railway open       # 打开控制台
```
