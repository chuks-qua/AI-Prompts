# Agent Brief

Turn a task description, file, issue, or this conversation into a precise, verifier-aligned brief an agent can execute and self-check against. Describe observable behaviour, define terms, separate hard requirements from free choices, and give checkable acceptance criteria.

Generate the brief and stop. Do not create issues, write PRDs, or start coding.

## Resolve the input

Read whatever follows the command and route it:

- Existing file path: read the file and use its contents as the task source.
- Issue or PR reference (`owner/repo#123`, `#123`, or a GitHub URL): fetch it with `gh issue view` or `gh pr view` if `gh` is available.
- Other free text: treat it as the task description.
- Nothing: build the brief from the current conversation.

Do not interview the user. Ask one question only when a hard requirement is missing. When the workspace holds relevant code, read it first so terms map to real names and behaviour.

## Compose the brief

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
- State what is free to change and what must be preserved.
- Make every acceptance criterion observable. If a program could not check it, rewrite it.
- Write in active voice and positive form. Cut needless words. No puffery, no em dashes.

## Output

- Default: print the brief in the chat.
- `--file`: write to `./<slug>-brief.md`, where `<slug>` is a short kebab-case name from the goal.
- `--save <path>`: write to the given path.

After printing or saving, you may offer once to hand the brief to a sub-agent. Do not dispatch automatically.
