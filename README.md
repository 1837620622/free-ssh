# Free SSH Server

> 在云平台上免费部署 SSH 服务器的完整教程

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

## 支持平台

| 平台 | 难度 | 免费额度 | TCP支持 | 文档 |
|------|------|---------|---------|------|
| [Zeabur](https://zeabur.com) | ⭐ | ✅ | 原生 | [查看](docs/zeabur.md) |
| [Railway](https://railway.app) | ⭐⭐ | ✅ | TCP代理 | [查看](docs/railway.md) |
| [Fly.io](https://fly.io) | ⭐⭐ | ✅ | 原生 | [查看](docs/flyio.md) |
| [Cloudflare](https://cloudflare.com) | ⭐⭐⭐ | ✅ | Tunnel | [查看](docs/cloudflare.md) |

## 快速开始

### 镜像

所有教程统一使用镜像：

```
ghcr.io/vevc/ubuntu:25.7.14
```

### 启动命令

```bash
/bin/bash -c "echo root:$ROOT_PASSWORD | chpasswd; sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config; sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config; mkdir -p /var/run/sshd; /usr/sbin/sshd -D"
```

### 环境变量

| 变量名 | 说明 |
|--------|------|
| ROOT_PASSWORD | SSH 登录密码 |

### 连接

```bash
ssh root@<域名> -p <端口>
```

## 平台对比

### Zeabur（推荐新手）

- ✅ 操作最简单
- ✅ 亚太节点丰富
- ✅ TCP 端口原生支持

### Railway（推荐开发者）

- ✅ GitHub 集成
- ✅ 内置数据库服务
- ✅ CLI 工具强大

### Fly.io（推荐全球部署）

- ✅ 全球分布式
- ✅ 内置 SSH 功能
- ✅ 按量计费

### Cloudflare Tunnel（推荐安全需求）

- ✅ 无需开放端口
- ✅ Zero Trust 支持
- ✅ 全球 CDN

## 目录结构

```
free-ssh/
├── README.md
├── docs/
│   ├── zeabur.md
│   ├── railway.md
│   ├── flyio.md
│   └── cloudflare.md
└── LICENSE
```

## 注意事项

1. **端口类型**：SSH 必须使用 TCP 类型，不是 HTTP
2. **启动命令**：`sshd -D` 的 `-D` 参数不能省略
3. **密码安全**：使用环境变量设置密码，不要硬编码
4. **免费额度**：各平台免费额度有限，注意用量

## 贡献

欢迎提交 Issue 和 Pull Request！

## 许可证

MIT License
