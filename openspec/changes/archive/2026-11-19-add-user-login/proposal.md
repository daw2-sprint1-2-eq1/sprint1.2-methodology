# Proposal

Issue: #3 — [feat] Implement user authentication

## Why

Every screen of the grade management app depends on knowing who the user is and which role they have. Without login there is no way to apply the permissions defined in the requirements.

## What Changes

- Existing users can log in with email and password; nobody can self-register.
- The session keeps the logged-in user until logout.
- Unauthenticated requests to protected pages are redirected to the login page.
- Users can log out.

Out of scope: password recovery, account creation (handled by the admin user CRUD issue).

## Capabilities

### New Capabilities

- `authentication`: login, logout and session handling for admin, teacher and student users.

### Modified Capabilities

_None._

## Impact

- New `AuthController`, `Auth` core helper and `login.php` view.
- `routes.php` gains a guard for protected routes.
- No new dependencies. Uses `password_hash` / `password_verify`.
