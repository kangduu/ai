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

项目级（当前仓库 `.agents/skills/`）：

```bash
npx skills add kangduu/ai --skill git-commit -a cursor
```

全局（所有项目 `~/.cursor/skills/`）：

```bash
npx skills add kangduu/ai --skill git-commit -a cursor -g
```

### Codex IDE / TUI

项目级（当前仓库 `.agents/skills/`）：

```bash
npx skills add kangduu/ai --skill git-commit -a codex
```

全局（所有项目 `~/.codex/skills/`）：

```bash
npx skills add kangduu/ai --skill git-commit -a codex -g
```

或在 Codex 里用自带 `$skill-installer`（默认安装到全局 `~/.codex/skills/git-commit`）：

```bash
--repo kangduu/ai --path skills/git-commit
```

新开会话后即可调用。

## ...
