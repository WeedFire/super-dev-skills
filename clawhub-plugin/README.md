# 🦞 Super-Dev Plugin for OpenClaw

> 全栈之神·自进化零缺陷交付 — 集成需求深访、TDD 驱动开发、供应链安全门禁、架构设计审查、感官级 UI/UX 验证的全栈开发工具套件。

## 📦 安装

```bash
# 从 ClawHub 安装
openclaw plugins install @openclaw/super-dev

# 或本地开发安装
openclaw plugins install -l ./clawhub-plugin
```

## 🛠️ 提供的工具

| 工具名 | 功能 | 适用阶段 |
|--------|------|----------|
| `super_dev_analyze_requirements` | 5W2H-D 需求深度分析，输出结构化用户故事和验收标准 | 需求分析 |
| `super_dev_review_architecture` | FIRST 原则架构审查，识别坏味道，输出架构建议 | 架构设计 |
| `super_dev_audit_security` | 四级安全门禁：依赖→代码→配置→供应链完整性 | 全阶段 |
| `super_dev_check_ux` | 四维 UI/UX 验证：视觉/交互/无障碍/响应式 | 前端开发 |
| `super_dev_generate_test_scaffold` | TDD 测试脚手架自动生成 (Red→Green→Refactor) | 编码实现 |
| `super_dev_analyze_code_quality` | 代码质量分析：复杂度/重复/命名/SOLID | 代码审查 |
| `super_dev_generate_adr` | 架构决策记录 (ADR) 文档生成 | 架构决策 |
| `super_dev_build_shared_language` | 领域术语提取与统一语言建立 | 领域建模 |

## ⚙️ 配置

在 `openclaw.json` 中配置：

```jsonc
{
  "plugins": {
    "entries": {
      "super-dev": {
        "enabled": true,
        "config": {
          "autoAudit": true,              // 自动安全审计
          "strictTDD": false,             // 严格 TDD 模式
          "auditLevel": "high",           // 审计级别: low|medium|high|critical
          "uxCheckEnabled": true,         // UI/UX 检查
          "architectureReviewEnabled": true, // 架构审查
          "dependencyCheckEnabled": true  // 依赖扫描
        }
      }
    }
  }
}
```

## 🚀 使用示例

### 需求分析
```
请使用 super_dev_analyze_requirements 分析以下需求：
"用户希望能够通过手机号注册账号，注册后可以浏览商品并下单购买"
```

### 安全审计
```
请使用 super_dev_audit_security 扫描当前项目的安全问题
```

### TDD 脚手架
```
请使用 super_dev_generate_test_scaffold 为以下函数生成测试：
language: typescript
signature: async function calculateDiscount(quantity: number, unitPrice: number, userLevel: string): Promise<number>
```

### 架构审查
```
请使用 super_dev_review_architecture 审查以下架构设计：
（描述你的架构方案）
```

## 📂 插件结构

```
clawhub-plugin/
├── openclaw.plugin.json   # 插件清单（声明元数据、配置Schema、工具列表）
├── package.json           # Node.js 包清单（含 openclaw.extensions）
├── index.ts               # TypeScript 主入口，注册 8 个 Agent 工具
├── README.md              # 本文档
└── skills/                # 技能目录（每个子目录含 SKILL.md）
    ├── super-dev-orchestrator/SKILL.md      # 🎯 总控调度
    ├── super-dev-requirements/SKILL.md      # 📋 需求深访
    ├── super-dev-architect/SKILL.md         # 🏗️ 架构设计
    ├── super-dev-tdd/SKILL.md               # 🧪 TDD 驱动
    ├── super-dev-ux/SKILL.md                # 🎨 UI/UX 验证
    ├── super-dev-security/SKILL.md          # 🔒 安全门禁
    ├── super-dev-memory/SKILL.md            # 🧠 分层记忆
    ├── super-dev-evolution/SKILL.md         # 🔄 自进化
    ├── super-dev-shared-language/SKILL.md   # 🗣️ 共享语言
    └── super-dev-suite/SKILL.md             # 📦 套件总览
```

## 🧩 技能（Skills）一览

| Skill | 触发词 | 职责 |
|-------|--------|------|
| `super-dev-orchestrator` | 任何开发任务 | 模式识别、任务委派、记忆存取 |
| `super-dev-requirements` | 新功能、需求模糊 | 5W2H-D 深访、用户故事、验收标准 |
| `super-dev-shared-language` | 术语不一致 | DDD 统一语言、禁止映射表 |
| `super-dev-architect` | 架构设计、技术选型 | 架构图、技术选型、DDL、ADR |
| `super-dev-tdd` | 开始编码、修Bug | Red→Green→Refactor、五轴审查 |
| `super-dev-ux` | 测UI、检查体验 | AI 美学反模式、四维感官审计 |
| `super-dev-security` | 新增依赖、交付前 | Always/Ask/Never、恶意包比对 |
| `super-dev-memory` | 任务启动/结束 | 三层记忆加载/写回/压缩 |
| `super-dev-evolution` | 任务收尾 | Skill 提炼、Nudge Engine、经验归档 |
| `super-dev-suite` | 套件总览 | 架构说明、激活方式 |

## 🔗 相关资源

- [Super-Dev Skills 套件](../) — 配套的 Skill 定义文件
- [OpenClaw 插件文档](https://docs.openclaw.ai/plugins/building-plugins)
- [ClawHub 注册中心](https://github.com/openclaw/clawhub)

## 📄 License

MIT
