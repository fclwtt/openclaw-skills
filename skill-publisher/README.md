# 🚀 OpenClaw Skill: Skill Publisher

**Skill 发布助手** - 自动化创建和发布 OpenClaw Skill 到 GitHub

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![OpenClaw Skill](https://img.shields.io/badge/OpenClaw-Skill-blue)](https://openclaw.ai)
[![Version](https://img.shields.io/badge/version-1.1.0-green.svg)](https://github.com/fclwtt/openclaw-tt/releases)

---

## 📋 功能

- ✅ **自动创建 Skill** - 生成标准 OpenClaw Skill 项目结构
- ✅ **Monorepo 支持** - 推送到统一仓库（如 openclaw-tt）
- ✅ **GitHub 自动推送** - SSH/Token 双模式认证
- ✅ **Release 创建** - 自动打标签发布
- ✅ **用户引导** - 清晰的 SSH/Token 配置指引
- ✅ **错误诊断** - 智能检测和修复常见问题

---

## 🚀 安装

### 从 Monorepo 安装

```bash
git clone https://github.com/fclwtt/openclaw-tt.git
cd openclaw-tt/skill-publisher
./install.sh
```

### ClawHub 安装

```bash
clawhub install skill-publisher
```

---

## 💡 使用方式

### 方式 1：对话中使用（推荐）

直接告诉我：
- "创建一个新 skill"
- "发布 skill 到 github"
- "帮我推送 wecom-channel-fix"

我会自动引导你完成整个流程！

### 方式 2：使用脚本

**创建新 Skill：**
```bash
~/.openclaw/skills/skill-publisher/scripts/create-skill.sh
```

**推送到 GitHub：**
```bash
~/.openclaw/skills/skill-publisher/scripts/push-to-github.sh <skill-name>
```

---

## 🎯 工作流程

### 创建新 Skill

```
1. 用户触发："创建一个新 skill"
   ↓
2. 收集信息：名称、描述、功能、作者
   ↓
3. 创建项目结构（skills/, docs/, scripts/）
   ↓
4. 生成标准文件（SKILL.md, README.md, install.sh）
   ↓
5. 询问是否推送到 GitHub
   ↓
6. 检查 SSH/Token 配置
   ↓
7. 推送到 Monorepo（fclwtt/openclaw-tt）
   ↓
8. 创建 Release v1.0.0
   ↓
✅ 完成！
```

### 发布已有 Skill

```
1. 用户触发："发布 xxx 到 github"
   ↓
2. 定位 Skill 目录
   ↓
3. 检查 Git 配置
   ↓
4. 推送到 Monorepo
   ↓
5. 创建 Release
   ↓
✅ 完成！
```

---

## 🔧 配置

### Monorepo 配置

编辑 `scripts/push-to-github.sh`：

```bash
GITHUB_USER="fclwtt"           # 你的 GitHub 用户名
MONOREPO="openclaw-tt"         # Monorepo 仓库名
REMOTE="git@github.com:$GITHUB_USER/$MONOREPO.git"
```

### SSH 密钥配置（推荐）

```bash
# 生成密钥
ssh-keygen -t ed25519 -C "your@email.com"

# 查看公钥
cat ~/.ssh/id_ed25519.pub

# 添加到 GitHub
# https://github.com/settings/keys
```

### Personal Access Token

```bash
# 创建 Token
# https://github.com/settings/tokens
# 勾选：repo, workflow

# 使用 Token
export GITHUB_TOKEN=ghp_xxxxxxxxxxxx
```

---

## 📁 项目结构

Skill Publisher 创建的标准结构：

```
skill-name/
├── skills/
│   └── SKILL.md          # OpenClaw 技能定义
├── docs/
│   ├── INSTALL.md        # 安装指南
│   ├── USAGE.md          # 使用手册
│   └── TROUBLESHOOTING.md # 故障排除
├── scripts/
│   └── helper.sh         # 辅助脚本
├── install.sh            # 安装脚本
├── uninstall.sh          # 卸载脚本
├── README.md             # 项目说明
├── LICENSE               # MIT 许可证
├── .gitignore            # Git 忽略规则
└── .github/workflows/
    └── release.yml       # 自动发布工作流
```

---

## 📊 示例对话

### 示例 1：创建新 Skill

**用户：** 我想创建一个新 skill

**助手：**
```
🚀 好的！请提供：

1. Skill 名称（英文，小写，连字符）：
   例：wecom-channel-fix

2. Skill 描述（一句话）：
   例：企业微信通道配置诊断与修复

3. 主要功能（3-5 个）：
   例：
   - 自动诊断企微通道问题
   - 插件 ID 不匹配修复
   - 多账号模式检测

准备好了吗？
```

**用户：** 名称：my-skill，描述：...，功能：...

**助手：**
```
✅ 项目创建完成！
📁 /home/tt/.openclaw/workspace/my-skill/

🔍 检查 GitHub 连接...
✅ SSH 已配置

🚀 推送到 fclwtt/openclaw-tt...
✅ 推送成功！

📦 创建 Release v1.0.0...
✅ 完成！

访问：
https://github.com/fclwtt/openclaw-tt/tree/main/my-skill
```

---

## 🐛 故障排除

### SSH 连接失败

```bash
# 测试连接
ssh -T git@github.com

# 应看到：Hi username! You've successfully authenticated
```

### 仓库不存在

请先在 GitHub 创建 Monorepo：
1. 访问 https://github.com/new
2. Repository name: `openclaw-tt`
3. 不要勾选初始化选项
4. 点击 "Create repository"

### Git subtree 失败

```bash
# 如已存在，使用 pull 更新
git subtree pull --prefix=skill-name origin main --squash
```

---

## 📚 相关资源

- [OpenClaw 文档](https://docs.openclaw.ai)
- [ClawHub](https://clawhub.ai)
- [GitHub 文档](https://docs.github.com)
- [语义化版本](https://semver.org/)

---

## 📝 更新日志

### v1.1.0 (2026-03-12)
- ✨ 改进 Monorepo 推送逻辑
- 🐛 修复目录结构冲突问题
- 📝 完善用户引导文档
- 🔧 优化 SSH/Token 配置流程

### v1.0.0 (2026-03-12)
- 🎉 首次发布
- ✅ Skill 创建功能
- ✅ GitHub 推送功能
- ✅ Monorepo 支持

---

**版本：** 1.1.0  
**作者：** tt (fclwtt)  
**许可证：** MIT
