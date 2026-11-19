# Our AI-assisted development method

This document defines how Team 1 builds software with AI agents. It covers the
tools, the phases of work, the validation criteria and the integration with
GitHub, as required by M0616 in sprint 1.2.

## 1. Principles

1. **Specs first.** Requirements are written, reviewed and versioned before any
   code is generated. A chat message is never the source of truth.
2. **AI second.** The agent implements a reviewed plan. It does not decide scope.
3. **Humans own it.** The person who opens a pull request must understand,
   explain and defend every line, whoever (or whatever) wrote it.
4. **Small and traceable.** One issue → one change → one branch → one PR.
5. **Verified, not assumed.** "Done" means tests, CI and a teammate's approval,
   never "it looks like it works".

## 2. Tools

| Tool | Role | What it owns |
| --- | --- | --- |
| **OpenSpec** | Defines *what* | `proposal.md`, `design.md`, `tasks.md`, spec deltas, the current specs |
| **Claude Code** | Builds *how* | Reads `AGENTS.md`, the spec and the skills; implements on the issue branch; runs tests |
| **GitHub** | Tracks & gates | Issues, Projects kanban, branches, pull requests, Actions CI |

Supporting files:

- `AGENTS.md` — shared rules for every agent and every teammate (architecture,
  security, verification, delivery). Tool-agnostic.
- `CLAUDE.md` — imports `AGENTS.md` so Claude Code loads it automatically.
- `openspec/config.yaml` — project context and per-artifact rules that OpenSpec
  injects into every proposal, spec, design and task list.
- `.agents/skills/` — reusable instructions (OpenSpec workflows, and in the app
  repo `php-pro`, `frontend-design`, `ponytail`). **Skills guide; project rules
  decide.** When a skill conflicts with `AGENTS.md`, `AGENTS.md` wins.

## 3. Phases

| # | Phase | Who | Output | Gate to continue |
| --- | --- | --- | --- | --- |
| 1 | **Issue** | Any teammate | GitHub issue `[type] Title` with description, acceptance criteria, estimate, assignee | Issue is in *Todo* on `sprint1.2-kanban` |
| 2 | **Spec** | Author + Claude Code (`/openspec-propose`) | `openspec/changes/<change>/` with proposal, spec deltas, design | `openspec validate <change> --strict` passes |
| 3 | **Plan** | Author, reviewed by a teammate | `tasks.md` with small tasks, each with its verification | A teammate agrees with scope and tasks (comment on the issue or a spec PR) |
| 4 | **Build** | Claude Code (`/openspec-apply-change`), supervised by the author | Commits on `issue/<n>-<desc>` | Each task group's tests pass locally |
| 5 | **Validate** | Author, CI, reviewer | Pull request with `Closes #<n>`, test evidence and screenshots | Every item of the Definition of Done (§4) |
| 6 | **Merge** | Reviewer merges; author archives | `main` updated; `openspec archive <change>` updates `openspec/specs/` | Issue moves to *Done* |

### How the agent is used in each phase

- **Spec:** we describe the issue in plain words and ask
  `/openspec-propose <issue summary>`. The agent reads `openspec/config.yaml`,
  existing specs and our rules, and writes the artifacts. We **edit them by hand**
  until they say what we mean; the agent's first draft is never accepted as is.
- **Plan:** we check that every task fits one class session and names how it is
  verified. If not, we ask the agent to split it, or we split it ourselves.
- **Build:** `/openspec-apply-change <change>` implements tasks in order and
  ticks the checkboxes. We stop it after each task group, read the diff and run
  the tests. If it drifts from the spec, we fix the spec first, not only the code.
- **Validate:** the agent may review the diff, but **AI review never replaces a
  human approval**.

## 4. Validation criteria (Definition of Done)

A task is *Done* only when **all** of these are true:

1. The issue's acceptance criteria are met and match the spec scenarios.
2. `openspec validate <change> --strict` passes.
3. `composer test` passes locally (application repo).
4. CI is green: PHP lint, PHPUnit, HTTP permission flows, JS syntax (app) and
   OpenSpec validation (this repo).
5. At least one teammate has approved the PR. AI review does not count.
6. The author can explain every line of the diff.
7. No secrets, `.env` files, tokens or personal data in the diff or in prompts.
8. Documentation is updated when behaviour or setup changes.
9. After merge, the change is archived so `openspec/specs/` reflects reality.

## 5. GitHub integration

**Organization and repos.** `daw2-sprint1-2-eq1` holds the application repo
(`sprint1.2-app`) and this methodology repo. Teachers have owner access; the
team is responsible for merges.

**Issues.** Title `[type] Short description`, where type is `feat`, `fix`,
`docs`, `test`, `refactor` or `chore`. Templates in `.github/ISSUE_TEMPLATE/`
include acceptance criteria and a field for the OpenSpec change name.

**Kanban.** Project `sprint1.2-kanban` with columns
*Todo → Blocked → In Progress → In Review → Done*. Estimated time on creation,
real time when moved to Done.

**Branches.** `issue/<number>-<short-description>` from `main`. Nobody commits
to `main`. Branches are deleted after merge.

**Commits.** `<type>: <short description> #<number>`, small and frequent.

**Pull requests.** Use `.github/pull_request_template.md`: what changed,
`Closes #<n>`, link to the OpenSpec change, how to test, screenshots for UI
changes, checklist of the Definition of Done.

**CI.** `.github/workflows/openspec.yml` runs `openspec validate --all --strict`
on every push and PR, so an invalid spec cannot be merged. The app repo runs its
own test workflow.

| Phase | Kanban column | GitHub artefact |
| --- | --- | --- |
| Issue | Todo | Issue |
| Spec / Plan | In Progress | `openspec/changes/<change>/` on the issue branch |
| Build | In Progress | Commits on `issue/<n>-…` |
| Validate | In Review | Pull request + CI |
| Merge | Done | Merged PR, archived change |

If work stops because of a dependency, the card goes to *Blocked* with a
comment saying what it waits for.

## 6. Roles

| Role | Member | Responsibility in this method |
| --- | --- | --- |
| Coordinator | Gabi | Keeps the kanban moving, daily meeting, talks to teachers |
| GitHub Manager | Ian | Checks branch/commit/issue naming, PR reviews before merge, release tag |
| Documentation Manager | Amin | Keeps `METHODOLOGY.md`, `AGENTS.md`, specs and README accurate |

Everyone writes specs, codes with the agent and reviews PRs.

## 7. Known limits and how we handle them

- The agent writes plausible specs that miss our real rules (e.g. "empty is not
  zero") unless `config.yaml` and `AGENTS.md` state them. Context files are the
  main lever.
- Long tasks make the agent drift. Tasks of one class session keep diffs readable.
- Generated tests can mirror the implementation. We review that tests check
  observable behaviour and failure cases.
- Database changes need a human decision about existing volumes; the agent does
  not see the running environment.

## 8. How we improve the method

At the end of each sprint we review: estimated vs real time per issue, how many
spec edits were needed before apply, and review findings. Changes to this method
are made with a PR to this repository.
