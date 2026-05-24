# Git 工作流规范

## 分支策略

### 分支命名

| 类型 | 命名格式 | 示例 |
|------|----------|------|
| 功能 | `feature/<ticket>-<描述>` | `feature/PROJ-123-user-auth` |
| 修复 | `fix/<ticket>-<描述>` | `fix/PROJ-456-login-bug` |
| 发布 | `release/<版本>` | `release/1.0.0` |
| 热修复 | `hotfix/<ticket>-<描述>` | `hotfix/PROJ-789-critical-fix` |

### 主要分支

```
main          # 主分支，保持稳定
├── develop   # 开发分支
├── feature/* # 功能分支
├── fix/*     # 修复分支
└── release/* # 发布分支
```

## 提交规范

### 提交格式

```
<类型>(<范围>): <描述>

[可选的正文]

[可选的脚注]
```

### 类型

| 类型 | 说明 |
|------|------|
| feat | 新功能 |
| fix | 修复 bug |
| docs | 文档更新 |
| style | 代码格式（不影响功能）|
| refactor | 重构 |
| test | 测试相关 |
| chore | 构建/工具相关 |

### 示例

```
feat(auth): 添加用户登录功能

- 实现用户名密码登录
- 添加记住我功能
- 集成验证码

Closes #123
```

## PR 规范

### PR 创建

1. 标题：`[TYPE] <描述> (#issue)`
2. 描述包含：改动内容、测试结果、截图（如有 UI 变更）
3. 指定 Reviewer

### PR 合并

- 至少 1 人 Review 通过
- CI/CD 全部通过
- 无冲突
- Squash merge 到 main

## Tag 规范

```
<major>.<minor>.<patch>

示例：
v1.0.0 - 首次发布
v1.0.1 - patch 修复
v1.1.0 - minor 新功能（向后兼容）
v2.0.0 - major 重大变更（不兼容）
```