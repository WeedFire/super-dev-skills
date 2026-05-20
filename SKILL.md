# super-dev-skills · 全栈之神自进化零缺陷交付套件

> 需求深访 + 共享语言 + TDD + 感官级UI/UX验证 + 供应链安全门禁 + 分层记忆系统 + 技能自我进化  
> 每次任务自动沉淀经验、提炼 Skill、持续越用越强，交付绝对安全可靠、体验极致的工业艺术品。

---

## 🏗️ 套件架构

本套件采用**总控调度 + 子 Skill 按需激活**的 MoE（混合专家）架构，解决单体 Skill 上下文浪费的核心痛点。

```
                    ┌─────────────────────────┐
                    │  super-dev-orchestrator  │  ← 总控
                    └─────────────────────────┘
          ┌─────────┬──────┬──────┬──────┬──────┬─────────┐
          ▼         ▼      ▼      ▼      ▼      ▼         ▼
   requirements architect  tdd    ux  security memory evolution shared-lang
```

## 📦 子 Skill 清单

| # | Skill | 文件 | 职责 |
|---|-------|------|------|
| 🎯 | Orchestrator | [super-dev-orchestrator.md](rules/skills/super-dev-orchestrator.md) | 总控调度、模式识别、任务委派、记忆存取 |
| 📋 | Requirements | [super-dev-requirements.md](rules/skills/super-dev-requirements.md) | 需求深访、用户故事提炼、验收标准、共享语言建立 |
| 🏗️ | Architect | [super-dev-architect.md](rules/skills/super-dev-architect.md) | 系统架构、技术选型、ADR 决策记录 |
| 🧪 | TDD | [super-dev-tdd.md](rules/skills/super-dev-tdd.md) | 测试驱动开发、Red→Green→Refactor 循环 |
| 🎨 | UX | [super-dev-ux.md](rules/skills/super-dev-ux.md) | 感官级 UI/UX 验证、无障碍、响应式 |
| 🔒 | Security | [super-dev-security.md](rules/skills/super-dev-security.md) | 供应链安全门禁、依赖扫描、代码审计 |
| 🧠 | Memory | [super-dev-memory.md](rules/skills/super-dev-memory.md) | 分层记忆系统、经验沉淀、知识检索 |
| 🔄 | Evolution | [super-dev-evolution.md](rules/skills/super-dev-evolution.md) | 技能自我进化、套件迭代优化 |
| 🗣️ | Shared Lang | [super-dev-shared-language.md](rules/skills/super-dev-shared-language.md) | 统一领域语言、消除翻译损耗 |

## 🚀 快速开始

```
# 激活全套件
激活 super-dev 套件，我要开发 [功能描述]

# 按需激活子 Skill
激活 super-dev-requirements，分析登录模块需求
激活 super-dev-security，审计新引入的 npm 依赖
激活 super-dev-evolution，检查套件是否需要优化
```

## 📐 核心设计原则

| 原则 | 说明 |
|------|------|
| **单一职责** | 每个子 Skill 只做一件事，做到极致 |
| **按需加载** | 不预先加载无关指令，节省上下文 |
| **总控调度** | Orchestrator 负责模式识别和任务委派 |
| **记忆闭环** | 每次任务自动沉淀经验，越用越强 |
| **安全默认** | Security 作为默认门禁，不跳过 |

## 📂 目录结构

```
super-dev-skills/
├── README.md
└── .codebuddy/
    └── skills/
        ├── super-dev-suite.md            # 套件总览
        ├── super-dev-orchestrator.md     # 总控调度
        ├── super-dev-requirements.md     # 需求深访
        ├── super-dev-architect.md        # 架构设计
        ├── super-dev-tdd.md              # TDD 驱动
        ├── super-dev-ux.md               # UI/UX 验证
        ├── super-dev-security.md         # 安全门禁
        ├── super-dev-memory.md           # 分层记忆
        ├── super-dev-evolution.md        # 自进化
        └── super-dev-shared-language.md  # 共享语言
```

## 🔄 任务生命周期

```
用户意图
  → Orchestrator 模式识别
    → 匹配 + 激活子 Skill
      → 子 Skill 执行（可链路激活多个）
        → 产出交付
          → Orchestrator 收集摘要
            → Memory 沉淀经验
              → Evolution 周期性优化套件
```

