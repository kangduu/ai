---
name: git-commit
description: Generates a detailed Simplified Chinese Conventional Commits message from git diffs, stages related files, and runs git commit without pushing. Use when the user invokes git-commit, asks to commit current changes, or wants a commit message written and committed.
license: MIT
---

# Git Commit

根据真实 diff 生成详细版中文 Conventional Commits 文案，立即暂存相关文件并执行 commit。不要只给文案就停。不执行 `git push`。

## 读取改动

1. 并行跑 `git status --short`、`git diff`（含已暂存）、`git log -8 --oneline`。
2. 优先看已暂存改动 `git diff --cached`；若没有已暂存改动，再看 `git diff`。
3. 不要夸大范围，不要编造未出现的变化。多个不相关主题时先建议拆分，不要揉成一次提交。

## 提交文案（详细版）

只使用这一种格式，不要给推荐版/简洁版让用户选，除非用户明确只要单行：

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

### 类型

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

### Scope

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

### 示例

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

## 执行 commit

1. 发现密钥、token、API Key、密码、证书、`.env` 或用户隐私数据时停止并提醒，不要提交。
2. 只暂存本次相关文件，不要 `git add .`。
3. 用上面的详细版格式作为唯一提交信息，执行 `git commit`（按当前 shell 传入多行 message），不要 push。
4. commit 成功后再跑一次 `git status` 确认。
