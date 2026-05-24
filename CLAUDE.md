# Claude Code Template

Claude Code 项目脚手架，为新项目提供统一的编码规范和项目结构。

## 项目结构

```
├── src/              # 源代码
├── tests/            # 测试代码
├── docs/             # 文档
├── .claude/           # Claude Code 配置
│   ├── rules/        # 编码规范
│   ├── skills/       # 技能定义
│   └── MEMORY.md     # 项目记忆
└── pyproject.toml    # 项目配置
```

## 编码规范

项目使用以下编码规范，详见 `.claude/rules/` 目录：

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

- 遵循 `.claude/rules/` 中的编码规范
- 在编写代码前先阅读相关规范文件
- 使用 Skill 时参考 `.claude/skills/` 目录