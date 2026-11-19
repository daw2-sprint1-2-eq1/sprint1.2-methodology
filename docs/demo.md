# Reproducible demo

This walkthrough lets anyone repeat our method from scratch. Steps 1–3 need only
Git and Node.js. Steps 4–6 use Claude Code. Allow about 15 minutes.

## 0. Prerequisites

| Tool | Check | Install |
| --- | --- | --- |
| Git | `git --version` | https://git-scm.com |
| Node.js ≥ 22.20 | `node --version` | https://nodejs.org |
| OpenSpec CLI | `openspec --version` | `npm install -g @fission-ai/openspec` |
| Claude Code (steps 4–6) | `claude --version` | https://docs.claude.com/en/docs/claude-code/overview |

## 1. Clone

```bash
git clone <REPO_URL> sprint1.2-methodology
cd sprint1.2-methodology
```

## 2. Explore the specs and changes

```bash
openspec list --specs          # current truth: authentication
openspec show authentication --type spec
openspec list                  # changes in progress: add-grade-mean (0/8 tasks)
openspec show add-grade-mean   # proposal, deltas and tasks
openspec validate --all --strict
```

What to look at:

- `openspec/changes/add-grade-mean/proposal.md` — *why* and *what*, linked to issue #14.
- `openspec/changes/add-grade-mean/specs/grading/spec.md` — requirements with
  WHEN/THEN scenarios a teacher could test by hand.
- `openspec/changes/add-grade-mean/tasks.md` — small tasks, each with its verification.
- `openspec/changes/archive/` — a finished change (`add-user-login`). Compare it
  with `openspec/specs/authentication/spec.md`: archiving merged its delta into the spec.

## 3. See the lifecycle without an agent (optional)

```bash
git switch -c issue/99-demo-archive
openspec archive add-grade-mean -y   # merges the delta into the specs
git status                           # specs/grading/spec.md created, change moved to archive/
openspec list --specs                # now: authentication + grading

# throw the demo away
git restore . && git clean -fd openspec
git switch main && git branch -D issue/99-demo-archive
```

> In the real workflow we archive only after the PR is merged (see METHODOLOGY.md §3).

## 4. Propose a change with Claude Code

Create a branch as if an issue existed (use any number):

```bash
git switch -c issue/15-export-grades-csv
claude
```

Inside Claude Code:

```
/openspec-propose Teachers can download the grades of one project as a CSV file. Issue #15.
```

The agent loads `CLAUDE.md` → `AGENTS.md` and `openspec/config.yaml`, asks if
anything material is unclear, and creates
`openspec/changes/<name>/` with `proposal.md`, `specs/…/spec.md`, `design.md`
and `tasks.md`. It stops after planning; it does not write code.

## 5. Review and validate (the human part)

1. Read `proposal.md`. Is the issue linked? Is out-of-scope stated?
2. Read the spec. Are permissions per role explicit? Does every requirement have a scenario?
3. Read `tasks.md`. Does every task fit one session and say how it is verified?
4. Edit anything that is wrong, by hand or by asking the agent.
5. Validate:

```bash
openspec validate --all --strict
git add openspec && git commit -m "docs: propose grades CSV export #15"
```

This is the spec that would be reviewed by a teammate before any code exists.

## 6. Build, validate, merge (in the application repo)

In `sprint1.2-app` the same change continues:

```
/openspec-apply-change <change-name>
```

Then:

```bash
composer test                       # must pass
git push -u origin issue/15-export-grades-csv
gh pr create --fill                 # body uses the PR template: Closes #15
# after one approval and green CI → merge
openspec archive <change-name> -y   # specs now describe the new behaviour
```

## What this demo shows

| M0616 requirement | Where you saw it |
| --- | --- |
| Explore OpenSpec and an AI agent | Steps 2 and 4 |
| Apply SDD to an app feature | `add-grade-mean`, step 4 |
| Compare ways of working with AI | [comparison.md](comparison.md) |
| Own method: tools, phases, validation, GitHub | [../METHODOLOGY.md](../METHODOLOGY.md), steps 5–6 |
