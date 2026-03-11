---
name: skill-publisher
description: OpenClaw Skill 发布助手。当用户想要创建新 Skill、发布 Skill 到 GitHub、推送到 ClawHub 时激活。支持 Monorepo 方案、自动创建项目结构、GitHub 推送、Release 创建。
version: 1.1.0
license: MIT
author: tt (fclwtt)
homepage: https://github.com/fclwtt/openclaw-tt/tree/main/skill-publisher
metadata:
  {
    "openclaw":
      {
        "emoji": "🚀",
        "always": false,
        "requires":
          {
            "bins": ["git"],
          },
      },
  }
---

# OpenClaw Skill 发布助手

帮助用户自动化创建和发布 OpenClaw Skill 到 GitHub。

## 触发场景

- "创建一个新 skill"
- "发布 skill 到 github"
- "把项目推送到 github"
- "开源我的 skill"
- "创建 skill 项目"
- "帮我发布 wecom-channel-fix"

## 核心功能

### 1. Skill 项目创建
- 自动生成标准 OpenClaw Skill 结构
- 包含完整的文档模板
- 配置安装/卸载脚本
- 设置 GitHub Actions 工作流

### 2. GitHub 推送（Monorepo）
- 推送到统一仓库（如 openclaw-tt）
- 自动配置 SSH 连接
- 支持 Personal Access Token
- 创建 Release 和标签

### 3. 用户引导
- SSH 密钥配置指引
- GitHub Token 创建教程
- 错误诊断和解决方案

## 工作流程

### 阶段 1：项目初始化

1. **收集项目信息**
   - Skill 名称（英文，小写，连字符）
   - Skill 描述（一句话）
   - 功能列表（3-5 个）
   - 作者信息

2. **创建项目结构**
```
skill-name/
├── skills/
│   └── SKILL.md          # OpenClaw 技能定义
├── docs/
│   ├── INSTALL.md
│   ├── USAGE.md
│   └── TROUBLESHOOTING.md
├── scripts/
│   └── helper.sh
├── install.sh
├── uninstall.sh
├── README.md
├── LICENSE
├── .gitignore
└── .github/workflows/
    └── release.yml
```

### 阶段 2：GitHub 配置检查

3. **检查连接状态**
```bash
# 检查 SSH 密钥
ls -la ~/.ssh/id_ed25519* 2>/dev/null

# 测试 GitHub 连接
ssh -T git@github.com
```

4. **引导配置（如需要）**

**如无 SSH 密钥：**
```
🔑 需要配置 SSH 密钥

请执行：
1. ssh-keygen -t ed25519 -C "your@email.com"
2. cat ~/.ssh/id_ed25519.pub
3. 复制公钥到 https://github.com/settings/keys
4. 点击 "New SSH key" 并粘贴

完成后回复 "done" 继续。
```

**如使用 Token：**
```
🔐 需要 GitHub Personal Access Token

请执行：
1. 访问 https://github.com/settings/tokens
2. 点击 "Generate new token (classic)"
3. 勾选 repo, workflow 权限
4. 复制 token（ghp_xxxxxxxxxxxx）

回复我你的 token 继续推送。
```

### 阶段 3：推送发布

5. **推送到 Monorepo**
```bash
# 默认：fclwtt/openclaw-tt
git remote add origin git@github.com:fclwtt/openclaw-tt.git
git subtree add --prefix=<skill-name> . main --squash
git push origin main
```

6. **创建 Release**
```bash
git tag -a v1.0.0 -m "Release v1.0.0 - <skill-name>"
git push origin v1.0.0
```

## 配置项

### Monorepo 配置

```json
{
  "github": {
    "username": "fclwtt",
    "monorepo": "openclaw-tt",
    "remote": "git@github.com:fclwtt/openclaw-tt.git"
  }
}
```

### 自定义仓库

用户可指定自己的 Monorepo：
```
用户：推送到我的仓库 my-skills
我：好的，使用 fclwtt/my-skills 作为目标仓库
```

## 命令参考

### 创建新 Skill

```bash
# 交互式创建
~/.openclaw/skills/skill-publisher/scripts/create-skill.sh

# 命令行参数
~/.openclaw/skills/skill-publisher/scripts/create-skill.sh \
  --name my-skill \
  --desc "描述" \
  --author "作者"
```

### 推送到 GitHub

```bash
# 推送到 Monorepo
~/.openclaw/skills/skill-publisher/scripts/push-to-github.sh <skill-name>

# 推送到独立仓库
~/.openclaw/skills/skill-publisher/scripts/push-to-github.sh \
  <skill-name> --standalone
```

### 创建 Release

```bash
~/.openclaw/skills/skill-publisher/scripts/create-release.sh \
  <skill-name> v1.0.0
```

## 示例对话

### 示例 1：创建新 Skill

**用户：** 我想创建一个新 skill

**助手：**
```
🚀 好的！我来帮你创建 OpenClaw Skill。

请提供：
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
✅ 项目创建中...

📁 已创建：
/home/tt/.openclaw/workspace/my-skill/
├── skills/SKILL.md
├── docs/
├── README.md
├── install.sh
└── ...

🔍 检查 GitHub 连接...
✅ SSH 已配置
✅ GitHub 连接正常

🚀 推送到 fclwtt/openclaw-tt...
✅ 推送成功！

📦 创建 Release v1.0.0...
✅ Release 已创建！

🎉 完成！

访问：
https://github.com/fclwtt/openclaw-tt/tree/main/my-skill

安装：
git clone https://github.com/fclwtt/openclaw-tt.git
cd openclaw-tt/my-skill
./install.sh
```

### 示例 2：发布已有 Skill

**用户：** 把 wecom-channel-fix 发布到 github

**助手：**
```
🚀 发布 wecom-channel-fix 到 GitHub...

📁 项目位置：
/home/tt/.openclaw/workspace/wecom-channel-fix

🔍 检查 GitHub 连接...
✅ SSH 密钥已配置
✅ GitHub 连接正常

📦 推送到 Monorepo (fclwtt/openclaw-tt)...
   路径：wecom-channel-fix/

✅ 推送成功！

📦 创建 Release v1.0.0...
✅ Release 已创建！

访问：
https://github.com/fclwtt/openclaw-tt/tree/main/wecom-channel-fix
```

## 错误处理

| 错误 | 原因 | 解决方案 |
|------|------|---------|
| Repository not found | 仓库不存在 | 引导用户在 GitHub 创建 |
| Permission denied | SSH/Token 无效 | 重新配置认证 |
| Already exists | 技能已存在 | 提示是否更新版本 |
| Git not installed | 未安装 Git | 指导安装 Git |
| Subtree failed | 目录冲突 | 使用 pull 更新 |

## 最佳实践

### 1. Skill 命名规范
- ✅ 小写字母、数字、连字符
- ✅ 简洁明了（2-4 个词）
- ❌ 避免特殊字符和下划线

**好：** `wecom-channel-fix`, `redbook-manager`  
**坏：** `WeChat_Fix`, `mySkill123`

### 2. 版本管理
- 使用语义化版本（SemVer）
- `v1.0.0` - 首次发布
- `v1.1.0` - 新功能
- `v1.0.1` - Bug 修复

### 3. 文档完整性
每个 Skill 应包含：
- ✅ README.md - 项目说明
- ✅ INSTALL.md - 安装指南
- ✅ USAGE.md - 使用手册
- ✅ TROUBLESHOOTING.md - 故障排除

### 4. Monorepo 优势
- 所有技能在一个仓库
- 用户 clone 一次获得全部
- 共用 CI/CD 和文档
- 易于维护和更新

## 注意事项

1. **推送前确认**
   - 检查技能文件完整性
   - 确认名称无冲突
   - 验证文档质量

2. **用户隐私**
   - 不要硬编码个人 Token
   - 使用环境变量存储敏感信息
   - 提醒用户保护 SSH 密钥

3. **错误恢复**
   - 推送失败时提供详细指南
   - 保留本地备份
   - 支持回滚操作

## 更新日志

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

## 参考资料

- [OpenClaw Skill 规范](https://docs.openclaw.ai/skills)
- [ClawHub 发布指南](https://clawhub.ai/publish)
- [GitHub 最佳实践](https://docs.github.com/en/repositories)
- [语义化版本](https://semver.org/)
