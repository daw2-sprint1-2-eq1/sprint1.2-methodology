# New on the team? Day one

| # | Step | How |
| --- | --- | --- |
| 1 | **Read** | `README.md` and `docs/team-agreements.md` in `sprint1.2-app`, then this repo's `METHODOLOGY.md` |
| 2 | **Run** | In `sprint1.2-app`: `docker compose up -d --build`, open the app and log in with a seed account |
| 3 | **Learn** | `AGENTS.md`, `docs/requirements.md` and `openspec list --specs` |
| 4 | **Pick** | Take a *Todo* issue from `sprint1.2-kanban`, assign it to yourself, move it to *In Progress*, create `issue/<n>-<desc>` |
| 5 | **Ship** | `/openspec-propose` → review → `/openspec-apply-change` → `composer test` → PR with `Closes #<n>` → approval → merge → `openspec archive` |

## Checklist before your first PR

- [ ] I can run the app and the tests locally.
- [ ] My branch and commits follow the naming rules.
- [ ] My change folder validates with `openspec validate <change> --strict`.
- [ ] I can explain every line of my diff without the agent.
- [ ] No `.env`, passwords or tokens in the diff or in my prompts.

## Asking for help

Blocked for more than one session? Move the card to *Blocked*, write what you
are waiting for, and tell the team in the daily meeting.
