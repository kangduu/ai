# 提交当前改动

先读取并遵循 `@git-commit-message` Skill 再生成详细版提交文案，然后立即执行 commit。不要只给文案就停。

执行步骤：

1. 并行跑 `git status`、`git diff`（含已暂存）、`git log -8 --oneline`。
2. 只暂存本次相关文件，不要 `git add .`。
3. 用 skill 的详细版格式作为唯一提交信息，不要再给推荐版/简洁版让我选。
4. 执行 `git commit`（多行 message），不要 push。
5. 发现密钥、token、`.env` 或用户隐私数据时停止并提醒。
6. 多个不相关主题时先建议拆分，不要揉成一次提交。
7. commit 成功后再跑一次 `git status` 确认。
