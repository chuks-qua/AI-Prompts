---
name: agent-brief
description: Turn a task description, file, issue, or conversation context into a precise, verifier-aligned agent brief in the DeepSWE instruction.md style - goal, expected outcomes with inline term definitions, implementation-freedom notes, and observable acceptance criteria. Use when briefing a coding agent, writing acceptance criteria, turning a task into an agent prompt or spec, or when the user runs /agent-brief. Output prints inline by default; pass --file or --save <path> to write a file.
---

# Agent Brief

Produce a behaviour-focused brief an agent can execute and self-check against. Mirror the DeepSWE `instruction.md` style: describe observable behaviour, define terms, separate hard requirements from free choices, and give checkable acceptance criteria. See [example-brief.md](example-brief.md) for a filled example.

This skill generates the brief and stops. It does not create issues, write PRDs, or dispatch agents. Compose those steps separately.

## Resolve input

Read the argument passed after the skill name and route it:

| Argument | Action |
|----------|--------|
| Existing file path | Read the file; treat its contents as the task source |
| Issue/PR ref (`owner/repo#123`, `#123`, or a GitHub URL) and `gh` available | Fetch it with `gh issue view` / `gh pr view` |
| Other free text | Treat as the task description |
| No argument | Synthesize from the current conversation context |

Do not interview the user. Ask a clarifying question only when a hard requirement is genuinely missing. If a repo is present and the task references code, explore briefly to ground terms in real names and behaviour.

## Compose the brief

Fill this structure:

```
<one-sentence imperative goal>

Expected outcomes
1. <concern group>
   - <observable behaviour>
   - <term> means <precise definition>.
2. <concern group>
   - ...

Implementation notes
- <what is an implementation detail, free to change>
- <public contracts / signatures that MUST be preserved>
- Keep the implementation focused on the behaviours above.

Acceptance criteria
- [ ] <observable, checkable behaviour a verifier could test>
- [ ] ...

Context (why)
<1-3 lines of motivation>
```

## Rules

- Describe behaviour, not implementation. Never dictate internal structure, helper names, or file layout unless it is a public contract.
- Define every non-obvious term inline ("X means ...").
- State explicitly what is free to change and what must be preserved.
- Make every acceptance criterion observable and checkable. If a program could not verify it, rewrite it.
- Apply the writing-clearly-and-concisely skill: active voice, positive form, concrete language, no puffery, no em dashes.

## Output

- Default: print the brief inline in the response.
- `--file`: write to `./<slug>-brief.md` in the current directory, where `<slug>` is a short kebab-case name derived from the goal.
- `--save <path>`: write to the given path.

After printing or saving, you may offer once: "Want me to hand this to a sub-agent?" Do not dispatch automatically.

## Verify before finishing

- [ ] Brief has all five sections.
- [ ] Outcomes and criteria are behaviour-only.
- [ ] Each acceptance criterion is observable.
- [ ] No em dashes, no puffery, active voice.
- [ ] Output went to the right target (inline / file / path).
