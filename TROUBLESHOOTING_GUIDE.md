# TrollVNC 完整部署指南

本文档记录了从源码到成功连接 TrollVNC 的完整操作流程。

## 目录

1. [Fork 仓库](#1-fork-仓库)
2. [SSH Clone 到本地](#2-ssh-clone-到本地)
3. [GitHub Actions 编译](#3-github-actions-编译)
4. [下载 tipa 文件](#4-下载-tipa-文件)
5. [TrollStore 安装](#5-trollstore-安装)
6. [VNC 连接](#6-vnc-连接)

---

## 1. Fork 仓库

### 操作步骤

1. 打开浏览器，访问原仓库：
   ```
   https://github.com/OwnGoalStudio/TrollVNC
   ```

2. 点击页面右上角的 **Fork** 按钮

3. 选择自己的 GitHub 账户，创建 fork

4. 验证 fork 成功：
   ```
   https://github.com/你的用户名/TrollVNC
   ```

---

## 2. SSH Clone 到本地

### 前置条件

确保已配置 SSH 密钥：
```bash
# 检查是否已有 SSH 密钥
ls ~/.ssh/

# 如果没有，生成新的 SSH 密钥
ssh-keygen -t ed25519 -C "你的邮箱@example.com"
```

### Clone 命令

```bash
# 切换到工作目录
cd /Users/zhangliye/Documents

# SSH 方式 clone（替换为你的用户名）
git clone git@github.com:leafshow/TrollVNC.git

# 进入项目目录
cd TrollVNC

# 验证远程仓库
git remote -v
# 输出应类似：
# origin  git@github.com:leafshow/TrollVNC.git (fetch)
# origin  git@github.com:leafshow/TrollVNC.git (push)
```

### 同步到本地（已完成）

当前本地路径：`/Users/zhangliye/Documents/TrollVNC`

已配置远程仓库为你的 fork：
```
origin: https://github.com/leafshow/TrollVNC.git
```

---

## 3. GitHub Actions 编译

### 操作步骤

1. 访问你的仓库 Actions：
   ```
   https://github.com/leafshow/TrollVNC/actions
   ```

2. 找到 **"Build TrollVNC"** workflow

3. 点击 **"Run workflow"**

4. 直接点击 **"Run workflow"** 按钮（无需修改参数）

5. 等待编译完成（约 5-10 分钟）

### 编译内容

GitHub Actions 会自动编译 4 种方案：
| 方案 | 说明 |
|------|------|
| `default` | 默认方案 |
| `rootless` | Rootless 越狱 |
| `roothide` | RootlessHide 越狱 |
| `bootstrap` | **TrollStore 安装** |

---

## 4. 下载 tipa 文件

### 操作步骤

1. 进入完成编译的 workflow run 页面

2. 向下滚动到 **"Artifacts"** 区域

3. 下载 **`packages-bootstrap`** 文件包

4. 解压下载的 zip 文件

5. 得到 `.tipa` 文件

### 文件位置

```
Downloads/
└── packages-bootstrap/
    └── TrollVNC_3.1_iphoneos-arm.tipa
```

---

## 5. TrollStore 安装

### 方法一：iOS 设备本地安装

1. 将 `.tipa` 文件传输到 iOS 设备
   - 通过 AirDrop
   - 通过数据线
   - 通过网盘

2. 在 iOS 设备上：
   - 用 Safari 打开 `.tipa` 文件
   - 点击 **"安装"**

3. 等待安装完成

### 方法二：通过 TrollStore 助手

如果设备已安装 TrollStore：
- 使用爱思助手或其他 TrollStore 助手工具安装 `.tipa`

### 安装后

1. 在 TrollStore 中找到 **TrollVNC** 图标

2. 启动应用

3. 点击界面上的 **"Start Server"** 启动 VNC 服务

---

## 6. VNC 连接

### 获取设备 IP

在 iOS 设备上：
```
设置 → WiFi → 当前连接的网络 → IP 地址
```

例如：`192.168.1.100`

### 使用 TigerVNC 连接

#### macOS 安装 TigerVNC

```bash
# 使用 Homebrew 安装
brew install tigervnc

# 启动 VNC Viewer
vncviewer
```

#### 连接步骤

1. 打开 TigerVNC Viewer

2. 在地址栏输入：
   ```
   192.168.1.100:5901
   ```

3. 点击 **Connect**

4. 如果设置密码，输入 VNC 密码

### 分辨率调整

如果屏幕显示过大，可以调整缩放比例：

```bash
# 在 iOS 设备上重新启动（需要 SSH 或通过应用设置）
trollvncserver -p 5901 -s 0.5 -n "My iPhone"
```

常用缩放参数：
| 参数 | 说明 |
|------|------|
| `-s 1.0` | 原生分辨率 |
| `-s 0.75` | 75%（推荐） |
| `-s 0.5` | 50%（更小） |

### 其他 VNC 客户端

| 平台 | 推荐客户端 |
|------|-----------|
| macOS | 屏幕共享、RealVNC、TigerVNC |
| Windows | RealVNC Viewer、UltraVNC |
| iOS | VNC Viewer |

---

## 常见问题

### Q: Microsoft Remote Desktop 能连接吗？

**不能**。RDP 和 VNC 是不同协议，Microsoft Remote Desktop 只能连接 Windows 远程桌面。

### Q: 连接超时怎么办？

1. 检查设备 IP 是否正确
2. 确认 TrollVNC 已启动
3. 检查防火墙设置

### Q: 画面模糊怎么办？

启动时使用更高缩放比例：
```bash
trollvncserver -p 5901 -s 1.0
```

### Q: 设备重启后 TrollVNC 用不了？

TrollStore 应用签名会过期，在 TrollStore 中"重新安装" TrollVNC 刷新签名即可。

---

## 相关链接

- 原仓库：https://github.com/OwnGoalStudio/TrollVNC
- 你的 Fork：https://github.com/leafshow/TrollVNC
- TigerVNC：https://tigervnc.org/
- TrollStore：https://github.com/opa334/TrollStore

---

*文档创建时间：2024年*
