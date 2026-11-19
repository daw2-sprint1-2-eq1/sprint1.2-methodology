# Comparing ways of working with AI

We compare three ways of using an AI agent on the same kind of task (a feature of
the grade management app) on the four aspects the M0616 brief
asks for: requirements, planning, code generation and validation.

| | A. Prompt & pray | B. Agent with plan mode | C. Spec-Driven (OpenSpec + Claude Code) |
| --- | --- | --- | --- |
| **How it works** | Describe the feature in the chat and accept the code | Ask the agent to plan first, approve the plan in the chat, then build | Write a reviewed OpenSpec change, then let the agent apply its tasks |
| **Requirements** | Live in the chat and get lost when the session ends | Better, but still only in the chat history | Versioned in Git as specs with testable scenarios |
| **Planning** | The agent improvises | A plan exists, but nobody else on the team can review it | `tasks.md` reviewed by a teammate before code |
| **Code generation** | Large, unfocused diffs; extra features nobody asked for | Smaller diffs, but scope can drift between sessions | Smallest change per task, one issue per branch |
| **Validation** | "It looks like it works" | Manual testing of what the plan said | Spec scenarios → tests, CI, human approval |
| **Team work** | Hard to hand over | Hard to hand over | Anyone can continue from the change folder |
| **Speed for a tiny fix** | Fastest | Fast | Slower: overhead not worth it for a typo |

## What we concluded

- **A** is useful only for throwaway experiments.
- **B** is good for one person, but the plan is not shared or versioned.
- **C** costs more time at the start but saves it in review and rework, and it is
  the only option that leaves documentation the team and teachers can audit.

## Our rule of thumb

| Change | Use |
| --- | --- |
| New behaviour, permissions, data model | Full OpenSpec change (C) |
| Bug fix with clear expected behaviour | Issue + agent + test; add a spec scenario if behaviour was unspecified |
| Typo, formatting, dependency bump | Issue + PR, no spec (`skip_specs` if a change folder is created) |

## Tools

- **OpenSpec** — adopted. Lightweight, Markdown only, works with any agent, and
  `openspec validate` can run in CI.
- **Claude Code** — adopted as our coding agent. Reads `CLAUDE.md`/`AGENTS.md`,
  supports project skills (the OpenSpec workflows are installed as skills).
- **GitHub** — already required by the course; we connected each OpenSpec change
  to an issue, a branch and a PR.

> The brief also mentions OpenCode and Antigravity. Because OpenSpec and
> `AGENTS.md` are agent-agnostic, the method does not depend on Claude Code: the
> same change folder can be applied by another agent (`openspec init --tools opencode`).
