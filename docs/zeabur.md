# Zeabur 部署 SSH 服务

> Zeabur 是一站式云部署平台，支持一键部署各种应用和服务。

## 平台信息

| 项目 | 说明 |
|------|------|
| 官网 | https://zeabur.com |
| 镜像 | `ghcr.io/vevc/ubuntu:25.7.14` |
| 端口 | 22 |
| 难度 | ⭐ 简单 |

## 部署步骤

### 1. 登录控制台

访问 [Zeabur 控制台](https://dash.zeabur.com)，使用 GitHub / Google / 邮箱登录。

### 2. 创建项目

1. 点击 **New Project** 按钮
2. 选择部署区域：
   - Hong Kong（推荐国内用户）
   - Tokyo
   - Singapore

### 3. 添加服务

1. 点击 **Add Service** → **Prebuilt Image**
2. 输入镜像地址：
   ```
   ghcr.io/vevc/ubuntu:25.7.14
   ```
3. 点击 **Deploy**

### 4. 配置启动命令

1. 进入服务 **Settings** 页面
2. 找到 **Command** 配置项
3. 填入启动命令：

```bash
/bin/bash -c "echo root:$ROOT_PASSWORD | chpasswd; sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config; sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config; mkdir -p /var/run/sshd; /usr/sbin/sshd -D"
```

4. 点击 **Save**

### 5. 配置环境变量

在 **Variables** 页面添加：

| 变量名 | 值 |
|--------|-----|
| ROOT_PASSWORD | 你的密码 |

### 6. 配置网络

1. 进入 **Networking** 页面
2. 点击 **暴露新端口**
3. 配置：
   - 端口：`22`
   - 类型：`TCP`（重要！）
4. 点击 **生成域名**
5. 开启 **端口转发**

### 7. 连接 SSH

```bash
ssh root@<分配的域名> -p <分配的端口>
```

## 特点

- ✅ 操作简单，纯网页配置
- ✅ TCP 端口原生支持
- ✅ 亚太区域节点丰富
- ✅ 按量计费

## 注意事项

- 端口类型必须选择 **TCP**
- 端口转发开关必须开启
- 启动命令中 `-D` 参数不能省略
