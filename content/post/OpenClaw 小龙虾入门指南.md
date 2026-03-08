---
date: '2026-03-08T17:20:23+08:00'
draft: false
title: 'OpenClaw 小龙虾入门指南'
author: 'Keti'
---
# OpenClaw 小龙虾入门指南

> **EXFOLIATE! EXFOLIATE!** 🦞

## 什么是 OpenClaw？

**OpenClaw** 是一个你可以在自己的设备上运行的**个人 AI 助手**。它不是一个网页应用或云端服务，而是一个完全本地化的 AI 助手系统，可以通过你已经在使用的各种通讯渠道与你互动。

### 核心特点

- 🏠 **本地优先** - Gateway 控制面板在本地运行，数据和会话私密安全
- 🌐 **多渠道支持** - 支持 20+ 种主流通讯平台
- 🔧 **高度可定制** - 支持自定义技能（Skills）和配置
- 🎙️ **语音交互** - 支持 Voice Wake 和 Talk Mode
- 📱 **多设备协同** - 可在 macOS、iOS、Android 设备间协同工作

## 官方资源

- 🌐 官网：[https://openclaw.ai/](https://openclaw.ai/)
- 📦 GitHub：[https://github.com/openclaw/openclaw](https://github.com/openclaw/openclaw)
- 💬 Discord 社区：[Discord](https://discord.gg/openclaw)

---

## 系统架构

```
WhatsApp / Telegram / Slack / Discord / Signal / iMessage
               │
               ▼
┌───────────────────────────────┐
│            Gateway            │
│       (控制面板 - 本地运行)    │
│     ws://127.0.0.1:18789      │
└──────────────┬────────────────┘
               │
               ├─ Pi agent (AI 代理)
               ├─ CLI (命令行工具)
               ├─ WebChat UI
               ├─ macOS App
               └─ iOS / Android 节点
```

### 核心组件

1. **Gateway（网关）** - WebSocket 控制平面，管理会话、渠道、工具和事件
2. **Agent（代理）** - Pi agent 在 RPC 模式下运行，提供 AI 能力
3. **Channels（渠道）** - 连接各种通讯平台的适配器
4. **Skills（技能）** - 可扩展的功能模块

---

## 安装指南

### 系统要求

- **Node.js** ≥ 22
- **操作系统**：macOS、Linux、Windows (WSL2)
- 安装 git
- **包管理器**：npm、pnpm 或 bun

### 快速安装

```bash
# 全局安装 OpenClaw
npm install -g openclaw@latest

# 运行配置向导（推荐）
openclaw onboard --install-daemon
```

配置向导会引导你完成：
- Gateway 设置
- Workspace 配置
- Channels 连接
- Skills 安装

### 从源码安装

```bash
git clone https://github.com/openclaw/openclaw.git
cd openclaw

pnpm install
pnpm ui:build
pnpm build

pnpm openclaw onboard --install-daemon

# 开发模式（自动重载）
pnpm gateway:watch
```

---

## 支持的通讯渠道

| 类别 | 支持的平台 |
|------|-----------|
| 即时通讯 | WhatsApp、Telegram、Signal、iMessage、LINE、Zalo |
| 工作协作 | Slack、Discord、Microsoft Teams、Google Chat、Feishu、Mattermost |
| 开源社区 | IRC、Matrix、Nostr |
| 企业服务 | Nextcloud Talk、Synology Chat、Tlon |
| 社交媒体 | Twitch |
| Web | WebChat |

---

## 核心功能

### 1. 多渠道收件箱

OpenClaw 将所有连接的渠道整合到一个统一的 AI 助手体验中。无论消息来自哪个平台，AI 都能以一致的方式回应。

### 2. 语音唤醒和对话模式

- **macOS/iOS**：支持语音唤醒词
- **Android**：支持持续语音模式
- **TTS 集成**：ElevenLabs + 系统默认 TTS

### 3. Live Canvas（实时画布）

Agent 可以控制一个可视化工作空间，支持 A2UI（Agent-to-User Interface）交互。

### 4. 浏览器控制

OpenClaw 可以管理专用的 Chrome/Chromium 浏览器实例，支持：
- 截图
- 页面操作
- 文件上传
- 配置管理

### 5. 伴随应用

- **macOS App**：菜单栏控制、语音唤醒、WebChat
- **iOS 节点**：Canvas、语音、相机、屏幕录制
- **Android 节点**：聊天、语音、Canvas、设备命令

---

## 基础使用

### 启动 Gateway

```bash
openclaw gateway --port 18789 --verbose
```

### 发送消息

```bash
# 发送到指定号码
openclaw message send --to +1234567890 --message "Hello from OpenClaw"

# 与 AI 助手对话
openclaw agent --message "帮我整理任务清单" --thinking high
```

### 聊天命令

在支持的渠道中可以使用以下命令：

- `/status` - 查看会话状态（模型、token、成本）
- `/new` 或 `/reset` - 重置会话
- `/compact` - 压缩会话上下文
- `/think <level>` - 设置思考级别（off|minimal|low|medium|high|xhigh）
- `/verbose on|off` - 开启/关闭详细模式
- `/usage off|tokens|full` - 显示使用统计
- `/restart` - 重启 Gateway

---

## 配置文件

配置文件位置：`~/.openclaw/openclaw.json`

### 最小配置

```json
{
  "agent": {
    "model": "anthropic/claude-opus-4-6"
  }
}
```

### 完整配置示例

```json
{
  "agent": {
    "model": "anthropic/claude-opus-4-6",
    "defaults": {
      "thinkingLevel": "high"
    }
  },
  "channels": {
    "whatsapp": {
      "allowFrom": ["*"]
    },
    "telegram": {
      "botToken": "YOUR_BOT_TOKEN"
    },
    "discord": {
      "token": "YOUR_DISCORD_TOKEN"
    }
  },
  "browser": {
    "enabled": true,
    "color": "#FF4500"
  }
}
```

---

## Workspace（工作空间）

OpenClaw 的核心记忆保存在：`~/.openclaw/workspace/`

### 目录结构

```
~/.openclaw/workspace/
├── memory/           # 记忆存储
├── MEMORY.md         # 记忆文件
├── AGENTS.md         # Agent 配置
├── SOUL.md           # 灵魂/个性定义
├── TOOLS.md          # 工具配置
└── skills/           # 技能目录
    └── <skill>/
        └── SKILL.md
```

---

## 安全性

### DM（私信）访问控制

默认情况下，OpenClaw 对未知发送者采用**配对模式**：

1. 未知发送者收到配对码
2. Bot 不处理其消息
3. 使用 `openclaw pairing approve <channel> <code>` 批准

要公开接收 DM，需要显式配置：

```json
{
  "channels": {
    "discord": {
      "dmPolicy": "open",
      "allowFrom": ["*"]
    }
  }
}
```

### 沙盒模式

为非主会话（群组/频道）启用 Docker 沙盒：

```json
{
  "agents": {
    "defaults": {
      "sandbox": {
        "mode": "non-main"
      }
    }
  }
}
```

---

## 技能（Skills）系统

OpenClaw 支持三种类型的技能：

1. **Bundled Skills** - 内置技能
2. **Managed Skills** - 托管技能
3. **Workspace Skills** - 用户自定义技能

### ClawHub

ClawHub 是一个技能注册表，允许 AI 自动搜索并安装新技能。

---

## 进阶功能

### Tailscale 远程访问

OpenClaw 可以通过 Tailscale Serve/Funnel 暴露 Gateway：

```json
{
  "gateway": {
    "tailscale": {
      "mode": "serve"  // 或 "funnel"
    }
  }
}
```

### 远程 Gateway

可以在 Linux 服务器上运行 Gateway，然后通过：
- Tailscale Serve/Funnel
- SSH 隧道

从客户端连接。

### Agent 间通信

使用 `sessions_*` 工具在不同会话间协调工作：
- `sessions_list` - 发现活跃会话
- `sessions_history` - 获取会话历史
- `sessions_send` - 向其他会话发送消息

---

## 故障排查

### 健康检查

```bash
openclaw doctor
```

### 常见问题

1. **Gateway 无法启动** - 检查端口占用和配置文件
2. **渠道连接失败** - 验证 token 和凭据
3. **权限问题** - 检查文件权限和 macOS 权限

---

## 与其他 AI 工具的对比

| 特性 | OpenClaw | Claude Code | Cursor |
|------|----------|-------------|--------|
| 部署方式 | 本地/自托管 | 本地 CLI | 本地 IDE |
| 通讯渠道 | 20+ 平台 | 终端 | IDE 内置 |
| 语音支持 | ✅ | ❌ | ❌ |
| 多设备协同 | ✅ | ❌ | ❌ |
| 主要用途 | 个人 AI 助手 | 代码助手 | 代码编辑 |

---

## 更新与维护

### 切换开发渠道

```bash
# 稳定版（默认）
openclaw update --channel stable

# 测试版
openclaw update --channel beta

# 开发版
openclaw update --channel dev
```

### 升级指南

1. 运行 `openclaw doctor` 检查配置
2. 更新到最新版本
3. 检查 Breaking Changes

---

## 社区与贡献

OpenClaw 是一个开源项目，欢迎社区贡献！

- **贡献指南**：[CONTRIBUTING.md](https://github.com/openclaw/openclaw/blob/main/CONTRIBUTING.md)
- **AI 友好**：欢迎 AI 生成的 PR 🤖

---

## 相关链接

- [[00 常用工具]] - 本人的工具集合笔记
- [OpenClaw 官方文档](https://docs.openclaw.ai/)
- [DeepWiki](https://github.com/openclaw/openclaw/wiki)

---

**最后更新**：2026-03-08
**标签**：#AI #OpenClaw #个人助手 #开源
