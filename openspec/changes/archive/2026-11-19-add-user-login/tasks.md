# Tasks

## 1. Login flow

- [x] 1.1 Create login view in Catalan with CSRF token and verify it renders at /login
- [x] 1.2 Implement AuthController login with password_verify and verify with a PHPUnit test for valid and invalid credentials
- [x] 1.3 Regenerate session id on login and verify the id changes in the test

## 2. Guard and logout

- [x] 2.1 Add route guard for protected pages and verify anonymous /dashboard redirects to /login (HTTP test)
- [x] 2.2 Implement logout and verify the session is destroyed (HTTP test)

## 3. Delivery

- [x] 3.1 Open the PR with Closes #3, get one approval and a green CI run
