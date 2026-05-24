# Claude Code Template

Claude Code 项目脚手架，为新项目提供统一的编码规范和项目结构。

## 核心要求：遵循 SDD 协议

**在编写任何代码之前，必须先制定 SDD（Software Design Document）。**

所有代码实现都必须基于已批准的 SDD，不得偏离 SDD 的设计决策。

```
编写代码流程：
1. 理解需求 → 2. 编写 SDD → 3. 审核 SDD → 4. 实现代码 → 5. 验证实现
```

详见 `.claude/rules/SDD协议.md`

## SDD 协议要点

SDD 必须包含：

1. **概述** - 目的、范围、定义
2. **系统架构** - 组件图、模块关系
3. **接口定义** - API、消息格式、示例
4. **数据结构** - 数据模型、数据库 schema
5. **行为设计** - 状态机、流程图
6. **非功能性需求** - 性能、安全、可靠性

**审阅流程**：草稿 → 同行评审 → 架构师审核 → 批准发布

## 项目结构

```
├── src/              # 源代码
├── tests/            # 测试代码
├── docs/             # 文档（含 SDD 文档）
├── .claude/           # Claude Code 配置
│   ├── rules/        # 编码规范
│   ├── skills/       # 技能定义
│   └── MEMORY.md     # 项目记忆
└── pyproject.toml    # 项目配置
```

## 编码规范

项目使用以下编码规范，详见 `.claude/rules/` 目录：

- `SDD协议.md` - **必须遵循的软件设计文档协议**
- `python-coding.md` - Python 编码规范
- `api-design.md` - API 设计规范
- `git-workflow.md` - Git 工作流
- `testing.md` - 测试规范

## 使用方法

1. 复制模板到新项目：`cp -r claude_code_template <new_project>`
2. 更新 `pyproject.toml` 中的项目信息
3. 更新 `README.md`
4. 初始化 Git：`cd <new_project> && git init`

## Claude Code 使用

1. **收到需求时**：先编写 SDD，等待审核
2. **SDD 批准后**：按 SDD 实现代码
3. **代码完成后**：验证符合 SDD 设计
4. **遵循规则**：`.claude/rules/` 中的所有规范