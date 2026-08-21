---
name: git-commit-message
description: Writes detailed Conventional Commits messages in Simplified Chinese from git diffs. Use when generating, reviewing, or wording a commit message, 提交信息, or conventional commit text. Do not use this skill to run git add, git commit, or git push; executing a commit is handled by the /commit command.
---

# Git Commit Message

只根据真实 diff 生成提交文案。不执行 `git add`、`git commit`、`git push`。

真正提交由用户使用 `/commit` 完成。若当前请求来自 `/commit`，仍按本文案规则写 message，执行步骤交给那条命令。

## 何时使用

- 生成或润色 commit message
- 按 Conventional Commits 写中文提交说明
- `/commit` 需要提交文案时

不要在用户只是说「提交代码」时擅自 commit。

## 读取改动

只读检查，不改暂存区：

1. `git status --short`
2. 优先 `git diff --cached`
3. 若没有已暂存改动，再看 `git diff`
4. 需要对齐风格时看 `git log -8 --oneline`

不要夸大范围，不要编造未出现的变化。多个不相关主题时建议拆成多条 message，不要写成一次提交。

## 默认格式（详细版）

只输出这一种，除非用户明确只要单行：

```text
type(scope): 中文摘要

- 主要改动 1
- 主要改动 2
- 影响范围或注意事项
```

- `type` 英文；`scope` 英文小写；摘要不加句号，尽量不超过 72 字符。
- 用户用中文提问时说明用简体中文；明确要求英文时用英文。
- 组件名、模块名、路径、变量、函数、API、配置键保持英文，不翻译。
- diff 很大时先概括，再写 message。

## 类型

- `feat`: 新功能
- `fix`: 修复 bug
- `refactor`: 重构，不改变外部行为
- `perf`: 性能优化
- `style`: 格式调整，不影响逻辑
- `docs`: 文档
- `test`: 测试
- `build`: 构建、依赖、打包配置
- `ci`: CI/CD
- `chore`: 杂项维护
- `revert`: 回滚

## Scope

前端常见：`ui`、`component`、`page`、`hooks`、`router`、`store`、`api`、`auth`、`layout`、`styles`、`deps`、`config`。

按路径判断：

- `app/components/**`、`src/components/**` → `component`
- `app/pages/**`、`src/pages/**` → `page`
- `app/hooks/**`、`src/hooks/**` → `hooks`
- `app/routes.ts`、`src/router/**` → `router`
- `app/stores/**`、`src/store/**` → `store`
- `app/requests/**`、`src/api/**` → `api`
- `app/styles/**`、`src/styles/**` → `styles`
- `package.json` / lockfile → `deps`
- `vite.config.*` / `tsconfig.json` → `config`

## 示例

```text
feat(auth): 添加登录表单校验

- 为登录表单补充必填与格式校验
- 校验失败时在表单项下方展示错误提示
```

```text
fix(ui): 修复按钮点击无响应问题

- 修正点击处理未绑定到实际按钮节点的问题
- 保持原有按钮样式与禁用态不变
```

```text
refactor(component): 重构用户列表组件

- 抽出列表项渲染逻辑，不改变筛选与分页行为
```

## 安全

生成文案前检查是否包含密钥、token、API Key、密码、证书、`.env`、用户隐私数据。若有，明确提醒不要提交，并说明应先移除。
