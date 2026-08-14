# 案例：根据 diff 生成 Git 提交信息

- 层：行动与编排
- 主题：Skill
- 状态：完整

## 场景

前端改完登录页校验，暂存后要提交。人可以看懂 diff，但不想每次手写 Conventional Commits，也不想把「加了校验」写成含糊的 `update`。成功标准：得到三条可直接使用的提交信息（推荐命令、简洁版、详细版），且类型、scope、说明都对得上真实改动。

## 输入

### 用户原话

```text
帮我写提交信息
```

### 上下文

```text
$ git status --short
M  src/pages/Login.tsx
M  src/utils/validate.ts

$ git diff --cached
diff --git a/src/pages/Login.tsx b/src/pages/Login.tsx
index 1111111..2222222 100644
--- a/src/pages/Login.tsx
+++ b/src/pages/Login.tsx
@@ -1,6 +1,7 @@
 import { useState } from 'react'
+import { validateLogin } from '../utils/validate'

 export function Login() {
   const [email, setEmail] = useState('')
   const [password, setPassword] = useState('')
+  const [error, setError] = useState('')

   function onSubmit(e: React.FormEvent) {
     e.preventDefault()
+    const result = validateLogin({ email, password })
+    if (!result.ok) {
+      setError(result.message)
+      return
+    }
     // ... existing login request
   }
 }

diff --git a/src/utils/validate.ts b/src/utils/validate.ts
index 3333333..4444444 100644
--- a/src/utils/validate.ts
+++ b/src/utils/validate.ts
@@ -0,0 +1,17 @@
+export function validateLogin(input: { email: string; password: string }) {
+  if (!input.email.includes('@')) {
+    return { ok: false, message: '请输入有效邮箱' }
+  }
+  if (input.password.length < 8) {
+    return { ok: false, message: '密码至少 8 位' }
+  }
+  return { ok: true, message: '' }
+}
```

以上 diff 对应 [SKILL.md](SKILL.md) 中的示例「新增登录表单校验」，用来固定输入/输出契约，便于复现，而不是一次未保存的真实仓库快照。

### 运行条件

- 启用产物：[SKILL.md](SKILL.md)（`git-commit-message`）
- 用户使用中文提问 → 提交说明用中文
- 已有暂存改动 → 以 `git diff --cached` 为准
- 不执行 `git add` / `git commit`，只产出文案

## 输出

Skill 要求默认给出三个版本，且 `type` 英文、`scope` 小写、summary 无句号。

### 推荐版本

```bash
git commit -m "feat(auth): 添加登录表单校验"
```

### 简洁版本

```text
feat(auth): 添加登录表单校验
```

### 详细版本

```text
feat(auth): 添加登录表单校验

- 新增 validateLogin，校验邮箱格式与密码长度
- 登录提交前拦截非法输入并展示错误
```

`feat` 来自「新增校验」而不是修 bug；`auth` 来自登录页与校验逻辑，而不是泛化的 `ui`。文件虽在 `src/pages/**`，Skill 对 React 项目允许 `auth` 作为业务 scope，这里以业务含义为准。

## 产物

- [SKILL.md](SKILL.md)：可再次触发的 Git 提交信息 Skill（工作流、类型规则、三版本输出、安全检查）

单次三条文案是**输出样例**，不是产物。下次换一套 diff，应得到另一组文案，但仍走同一份 Skill。

## 抽出的知识点

- Skill 是可复用能力包：触发语和工作流写在产物里，这一次的 diff 只属于输入。
- 约束写在输入侧（中文提问、已暂存、不要代为 commit），格式写在输出侧（三个版本、`type(scope): summary`）。
- 输出必须绑定真实 diff：scope 和 type 从路径与改动性质推断，不能凭空写成 `update`。
