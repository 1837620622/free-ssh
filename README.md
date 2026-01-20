# Free SSH Server

> 🚀 在云平台上免费部署 SSH 服务器的完整教程

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![GitHub stars](https://img.shields.io/github/stars/1837620622/free-ssh)](https://github.com/1837620622/free-ssh/stargazers)

## ✨ 特性

- 🆓 **完全免费** - 利用各平台免费额度
- 🐳 **Docker 部署** - 统一镜像，一键部署
- 🔐 **Root 权限** - 默认 root 用户，最高权限
- 🌍 **多平台支持** - 5 个主流云平台教程
- 📖 **详细文档** - 图文并茂，新手友好

## 📋 支持平台

| 平台 | 难度 | 免费额度 | TCP支持 | 文档 |
|------|------|---------|---------|------|
| [Zeabur](https://zeabur.com) | ⭐ | ✅ | 原生 | [查看](docs/zeabur.md) |
| [爪云 Claw Cloud](https://console.run.claw.cloud) | ⭐ | ✅ $5/月 | 原生 | [查看](docs/clawcloud.md) |
| [Railway](https://railway.app) | ⭐⭐ | ✅ | TCP代理 | [查看](docs/railway.md) |
| [Fly.io](https://fly.io) | ⭐⭐ | ✅ | 原生 | [查看](docs/flyio.md) |
| [Cloudflare](https://cloudflare.com) | ⭐⭐⭐ | ✅ | Tunnel | [查看](docs/cloudflare.md) |

## 🚀 快速开始

### 1️⃣ 选择平台

根据需求选择合适的平台：

| 需求 | 推荐平台 |
|------|---------|
| 新手入门 | Zeabur |
| 免费额度最多 | 爪云 Claw Cloud |
| 开发者 | Railway |
| 全球部署 | Fly.io |
| 安全需求 | Cloudflare |

### 2️⃣ 配置镜像

所有平台统一使用镜像：

```
ghcr.io/vevc/ubuntu:25.7.14
```

### 3️⃣ 设置启动命令

```bash
/bin/bash -c "echo root:$ROOT_PASSWORD | chpasswd; sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config; sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config; mkdir -p /var/run/sshd; /usr/sbin/sshd -D"
```

### 4️⃣ 配置环境变量

| 变量名 | 说明 | 示例 |
|--------|------|------|
| ROOT_PASSWORD | SSH 登录密码 | YourSecurePassword123 |

### 5️⃣ 连接 SSH

```bash
ssh root@<分配的域名> -p <分配的端口>
```

## 📊 平台对比

| 平台 | 优势 | 适合场景 |
|------|------|---------|
| **Zeabur** | 操作简单、亚太节点丰富 | 新手入门 |
| **爪云** | GitHub 账号每月 $5 免费 | 免费用户 |
| **Railway** | GitHub 集成、内置数据库 | 开发者 |
| **Fly.io** | 全球分布式、内置 SSH | 全球部署 |
| **Cloudflare** | Zero Trust、无需开放端口 | 安全需求 |

## ⚠️ 注意事项

| 事项 | 说明 |
|------|------|
| 端口类型 | SSH 必须使用 **TCP** 类型，不是 HTTP |
| 启动命令 | `sshd -D` 的 `-D` 参数不能省略 |
| 密码安全 | 使用环境变量设置密码，不要硬编码 |
| 免费额度 | 各平台免费额度有限，注意用量 |

## 📁 目录结构

```
free-ssh/
├── README.md
├── LICENSE
└── docs/
    ├── zeabur.md      # Zeabur 教程
    ├── clawcloud.md   # 爪云教程
    ├── railway.md     # Railway 教程
    ├── flyio.md       # Fly.io 教程
    └── cloudflare.md  # Cloudflare 教程
```

## 🤝 贡献

欢迎提交 Issue 和 Pull Request！

## 📄 许可证

[MIT License](LICENSE)

## ⭐ Star History

如果这个项目对你有帮助，请给一个 Star ⭐
