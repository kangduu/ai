# Head towards AI

## prompt

## skills

Git skills 使用 [Agent Skills](https://agentskills.io) 格式，可在 Cursor 与 Codex 的 IDE / TUI 中直接安装。

源码：

```text
skills/
  git-commit/
    SKILL.md
```

安装后用 `/git-commit` 生成详细版中文 Conventional Commits 文案、暂存相关文件并执行 commit（不 push）。

### Cursor IDE / TUI

```bash
npx skills add kangduu/ai --skill git-commit -a cursor
```

全局安装加 `-g`（落到 `~/.cursor/skills/`）。项目级落到 `.agents/skills/`。

### Codex IDE / TUI

```bash
npx skills add kangduu/ai --skill git-commit -a codex
```

或在 Codex 里用自带 `$skill-installer`：

```bash
# 安装到 ~/.codex/skills/git-commit
--repo kangduu/ai --path skills/git-commit
```

新开会话后即可调用。

## ...
