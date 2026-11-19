# sprint1.2-methodology

**Team 1 · DAW2 2026-27 · M0616 Projecte transversal · Sprint 1.2**
Ian · Gabi · Amin

Our AI-assisted development method, documented and reproducible. It combines
**OpenSpec** (spec-driven development), **Claude Code** (AI coding agent) and
**GitHub** (issues, kanban, pull requests and CI).

> **Specs first. AI second. Humans own it.**
> The agent never starts from a chat message. It starts from a reviewed spec,
> works on an issue branch, and a teammate approves the result.

---

## Try it in 10 minutes

Requirements: Git, Node.js 22.20 or newer, and (for steps 4–5) [Claude Code](https://docs.claude.com/en/docs/claude-code/overview).

```bash
# 1. Clone and install the OpenSpec CLI
git clone <REPO_URL> sprint1.2-methodology
cd sprint1.2-methodology
npm install -g @fission-ai/openspec

# 2. Explore what is already here
openspec list                 # changes in progress
openspec list --specs         # current specs (the "truth")
openspec show add-grade-mean  # an example change ready to implement
openspec validate --all --strict

# 3. Start the agent in this folder
claude
```

Then follow the full walkthrough in **[docs/demo.md](docs/demo.md)**: you will
propose your own change with Claude Code, review it, validate it and archive it,
exactly as we do in the team.

---

## What is in this repository

| Path | What it is |
| --- | --- |
| [METHODOLOGY.md](METHODOLOGY.md) | The method: principles, tools, phases, validation criteria and GitHub integration |
| [docs/demo.md](docs/demo.md) | Step-by-step reproducible demo |
| [docs/comparison.md](docs/comparison.md) | How we compared different ways of working with AI |
| [docs/onboarding.md](docs/onboarding.md) | Day one for a new teammate |
| [AGENTS.md](AGENTS.md) | Rules every AI agent (and every human) follows in our repos |
| [CLAUDE.md](CLAUDE.md) | Entry point for Claude Code; imports AGENTS.md |
| `openspec/config.yaml` | Project context and rules OpenSpec injects into every artifact |
| `openspec/specs/` | Current specifications (what the system does today) |
| `openspec/changes/` | Changes in progress; `archive/` holds finished ones |
| `.claude/skills/`, `.agents/skills/` | OpenSpec workflows for the agent (`/openspec-propose`, `/openspec-apply-change`, …) |
| `.github/` | Issue templates, PR template and the CI that validates specs |

## The workflow at a glance

```
Issue ──► Spec ──► Plan ──► Build ──► Validate ──► Merge
 #14     proposal  tasks.md  Claude    tests, CI,   PR merged,
         + specs   reviewed  Code on   human        change
                             branch    approval     archived
```

| Kanban (`sprint1.2-kanban`) | Todo → Blocked → In Progress → In Review → Done |
| --- | --- |
| Branch | `issue/14-grade-mean` |
| Commit | `feat: add grade mean #14` |
| Pull request | `Closes #14` |

## Examples included

- **`openspec/changes/archive/…-add-user-login`** — a finished change: proposal,
  design, spec delta and all tasks checked. Archiving it produced
  `openspec/specs/authentication/spec.md`.
- **`openspec/changes/add-grade-mean`** — a change ready to implement in our
  application repo (`sprint1.2-app`), with 8 tasks still open.

Both validate with `openspec validate --all --strict`, and GitHub Actions
checks it on every push.

## Language

Documentation, specs and code are in English. The user interface of the
application is in Catalan, as required by the project brief.
