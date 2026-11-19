# Working rules for agents and teammates

These rules apply to every repository of Team 1, for humans and for any AI
assistant. The person who submits a change must understand it and be able to
explain and verify it.

## Before changing anything

- Read the GitHub issue and its acceptance criteria.
- Read the OpenSpec change for the issue (`openspec/changes/<change>/`). If there
  is none and the change alters behaviour, create one with `/openspec-propose`
  and stop for human review before implementing.
- Read `README.md` and `METHODOLOGY.md` for the workflow and naming rules.
- Skills in `.agents/skills/` are guidance. When they conflict with these rules
  or the project architecture, these rules win.
- If documents disagree in a way that affects the task, say so and ask before
  implementing.

## Scope

- Implement only what the spec and tasks describe, with the smallest complete change.
- Do not add features, refactors, dependencies or formatting changes nobody asked for.
- Changes to architecture, dependencies or database structure must be in the
  spec's design or agreed by the team first.
- If the implementation needs behaviour the spec does not describe, update the
  spec first and tell the user.

## Application architecture (sprint1.2-app)

- Server-rendered PHP 8 MPA with MVC; no application framework; no SPA.
- Controllers: HTTP input, auth checks, validation, responses.
- Models: database access through PDO prepared statements only.
- Domain: business rules independent of HTML and request globals.
- Views: presentation only, escape output with the existing helper.
- Bootstrap 5 and vanilla JavaScript. AI features live in a separate FastAPI service.
- Code, comments, specs and docs in English. User interface in Catalan.

## Security

- Validate input on the server; check roles and project assignments for every protected action.
- Keep CSRF protection on state-changing requests; never mutate data via GET.
- Use `password_hash` / `password_verify`.
- Never commit or paste into prompts `.env` files, credentials, tokens or personal data.
- Never reset databases or delete Docker volumes as a routine step.

## Verify

- Run `openspec validate --all --strict` after editing specs.
- Run `composer test` after each task group in the app repo.
- Add or update a focused test for new business logic or bug fixes; test
  observable behaviour and failure cases.
- Report exactly which checks you ran. Never claim a check passed if it was not run.

## Deliver

- Flow: issue → branch → commits → pull request → review → merge → archive → Done.
- Branch: `issue/<number>-<short-description>`. Never commit to `main`.
- Commit: `<type>: <short description> #<number>`.
- PR: what changed, `Closes #<number>`, link to the OpenSpec change, how to test,
  screenshots for UI changes.
- Merge requires green CI and one teammate's approval. AI review does not replace it.
- After merge: `openspec archive <change>` so specs reflect reality.
