# Claude Code entry point

@AGENTS.md

## OpenSpec workflows available as skills

- `/openspec-propose <description>` — create a change (proposal, specs, design, tasks). Planning only.
- `/openspec-apply-change <change>` — implement the tasks of a reviewed change.
- `/openspec-archive-change <change>` — archive a merged change and update the specs.
- `/openspec-explore` — think through an idea before proposing.

Always stop after `/openspec-propose` so a human can review the change before it is applied.
