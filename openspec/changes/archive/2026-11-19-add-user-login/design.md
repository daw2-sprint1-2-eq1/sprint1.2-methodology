# Design

## Context

See proposal.md — Why. The app is a server-rendered PHP MVC application without a framework.

## Goals / Non-Goals

**Goals:** a single login flow for all roles, server-side session, CSRF-protected form.

**Non-Goals:** OAuth, remember-me cookies, password reset.

## Decisions

- **Native PHP sessions** over JWT: the app is an MPA, every page is rendered by PHP, so a stateless token adds nothing.
- **`password_hash` / `password_verify`** over a custom hash: standard, salted and upgradeable.
- **Generic error message**: avoids leaking which emails exist.
- **Guard in the router** rather than in each controller, so a new route cannot forget it.

## Risks / Trade-offs

- [Session fixation] → regenerate the session id after a successful login.
- [Brute force] → out of scope for this sprint; noted for a later issue.
