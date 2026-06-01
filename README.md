# AI-Prompts

Prompts, commands, and agents for the coding assistants I use, organised by provider.

## Layout

| Path | Provider | Format |
|------|----------|--------|
| `claude/skills/` | Claude Code | Skills (`SKILL.md` + support files) |
| `cursor/commands/` | Cursor | Slash commands (markdown) |
| `codex/prompts/` | OpenAI Codex CLI | Custom prompts (markdown, `$ARGUMENTS`) |
| `vscode/agents/` | GitHub Copilot | Custom agents (`*.agent.md`) |
| `vscode/prompts/` | GitHub Copilot | Prompt files (`*.prompt.md`) |

## Installing

Copy a file into the matching config location for the tool:

| Provider | Source | Install to |
|----------|--------|------------|
| Claude Code | `claude/skills/<name>/` | `~/.claude/skills/<name>/` |
| Cursor | `cursor/commands/<name>.md` | `~/.cursor/commands/` (global) or `.cursor/commands/` (per repo) |
| Codex | `codex/prompts/<name>.md` | `~/.codex/prompts/` |
| Copilot agent | `vscode/agents/<name>.agent.md` | VS Code user prompts dir or `.github/` (per repo) |
| Copilot prompt | `vscode/prompts/<name>.prompt.md` | VS Code user prompts dir or `.github/prompts/` (per repo) |

Each runs as `/<name>` in its tool.

## agent-brief

Turns a task description, file, issue, or conversation into a precise, verifier-aligned brief an agent can execute and self-check against. The brief states one goal, lists expected outcomes with inline term definitions, marks what is free to change versus what must hold, and ends with checkable acceptance criteria. The format follows the [DeepSWE](https://github.com/datacurve-ai/deep-swe) `instruction.md` style.

Available for all four providers above. The Claude skill carries a worked example in `claude/skills/agent-brief/example-brief.md`.
