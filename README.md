# development-awareness

An agent skill that keeps coding work honest: confirm what you cannot know, investigate what you can, reuse what already exists, and do not build for requirements that do not exist yet.

The skill is a single [SKILL.md](SKILL.md) file with YAML frontmatter (`name`, `description`) plus the instructions. It works with any agent that loads `SKILL.md` skills — Claude Code, Codex, DeepSeek Harness, and similar tools.

## What it covers

| Section | Guidance |
|---|---|
| 0. Request confirmation | Severity-labeled multiple-choice questions (Critical / Important / Moderate / Optional). Ask only when the answer changes the implementation and cannot be looked up. |
| 1. Planning | Look for existing libraries before writing custom code; when proposing OSS, state star count and maintenance status. |
| 2. Research | Search the web for "latest" or external-service facts; do not search settled knowledge. |
| 3. Implementation | YAGNI, resolve uncertainty from docs and changelogs, follow the existing codebase over documentation. |
| 4. Execution | 10-second guideline: foreground for quick commands, background for long or unpredictable ones; detect completion by events instead of polling. |

## Install

Copy the skill into your agent's skills directory.

### Claude Code

```bash
git clone https://github.com/zaochuan5854/development-awareness.git
mkdir -p ~/.claude/skills/development-awareness
cp development-awareness/SKILL.md ~/.claude/skills/development-awareness/SKILL.md
```

Or symlink it so updates flow through:

```bash
ln -s "$PWD/development-awareness" ~/.claude/skills/development-awareness
```

### DeepSeek Harness / other agents

Place the directory containing `SKILL.md` in the agent's skill search path.

## License

MIT
