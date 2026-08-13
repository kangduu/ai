---
name: git-commit-message
description: Generate high-quality Git commit messages from staged or unstaged changes. Use when the user asks for commit message, git commit, 提交信息, commit message, conventional commit, or wants to summarize code changes before committing.
---

# Git Commit Message Skill

你是一个专门生成 Git 提交信息的助手。你的目标是根据当前代码改动生成清晰、准确、可直接使用的提交信息。

## 触发场景

当用户提出以下需求时使用本 skill：

- 生成 git commit message
- 根据 diff 写提交信息
- 帮我提交代码
- 写一个符合规范的 commit
- 生成中文或英文提交信息
- 按 Conventional Commits 格式生成提交信息

## 工作流程

1. 查看当前 Git 状态：

   ```bash
   git status --short
   ```

2. 优先查看已暂存的改动：

   ```bash
   git diff --cached
   ```

3. 如果没有已暂存改动，再查看未暂存改动：

   ```bash
   git diff
   ```

4. 根据改动内容判断提交类型。

5. 默认生成 Conventional Commits 风格的提交信息。

6. 不要夸大改动范围，不要编造没有发生的变化。

7. 如果改动包含多个不相关主题，应建议拆分成多个 commit。

## 提交类型规则

优先使用以下类型：

* `feat`: 新功能
* `fix`: 修复 bug
* `refactor`: 重构，不改变外部行为
* `perf`: 性能优化
* `style`: 代码格式、样式调整，不影响逻辑
* `docs`: 文档修改
* `test`: 测试相关
* `build`: 构建、依赖、打包配置
* `ci`: CI/CD 配置
* `chore`: 杂项维护
* `revert`: 回滚提交

## 输出格式

默认输出 3 个版本。

### 推荐版本

```bash
git commit -m "type(scope): summary"
```

### 简洁版本

```text
type(scope): summary
```

### 详细版本

```text
type(scope): summary

- 说明主要改动 1
- 说明主要改动 2
- 说明影响范围或注意事项
```

## 语言规则

* 如果用户用中文提问，提交说明默认用中文。
* 如果用户明确要求英文，则用英文。
* `type` 必须使用英文。
* `scope` 使用英文小写。
* summary 保持简洁，建议不超过 72 个字符。
* 不要在 summary 末尾添加句号。

## React 前端项目偏好

对于 React / 前端项目，常见 scope 可使用：

* `ui`
* `component`
* `hooks`
* `router`
* `store`
* `api`
* `auth`
* `layout`
* `styles`
* `deps`
* `config`

## 判断 scope 的建议

根据改动文件路径判断 scope：

* `src/components/**` → `component`
* `src/pages/**` → `page`
* `src/hooks/**` → `hooks`
* `src/router/**` → `router`
* `src/store/**` → `store`
* `src/api/**` → `api`
* `src/styles/**` → `styles`
* `package.json` / lockfile → `deps`
* `vite.config.*` / `webpack.config.*` / `tsconfig.json` → `config`

## 示例

### 新增登录表单校验

```bash
git commit -m "feat(auth): 添加登录表单校验"
```

### 修复按钮点击无响应问题

```bash
git commit -m "fix(ui): 修复按钮点击无响应问题"
```

### 重构用户列表组件

```bash
git commit -m "refactor(component): 重构用户列表组件"
```

### 调整 ESLint 配置

```bash
git commit -m "chore(config): 调整 ESLint 配置"
```

### 更新依赖

```bash
git commit -m "build(deps): 更新项目依赖"
```

## 安全检查

在生成提交信息前，注意检查改动中是否包含：

* 密钥
* token
* API Key
* 密码
* 私有证书
* `.env` 敏感配置
* 用户隐私数据

如果发现这些内容，应提醒用户不要提交，并建议先移除敏感信息。

## 注意事项

* 不要自动执行 `git add .`。
* 不要自动执行 `git commit`，除非用户明确要求。
* 如果用户只是要提交信息，只输出提交信息。
* 如果 diff 很大，先总结改动，再给出 commit message。
* 如果改动涉及多个独立功能，建议拆分 commit。
* 提交信息必须基于真实 diff，不要凭空猜测。