# authentication Specification

## Purpose
Defines how admin, teacher and student users sign in, keep a session and sign out of the grade management app.

## Requirements

### Requirement: Users log in with their credentials
The system SHALL let an existing user log in with their email and password, and SHALL NOT offer self-registration.

#### Scenario: Valid credentials
- **WHEN** an existing user submits the login form with a correct email and password
- **THEN** the system starts a session for that user and shows the dashboard for their role

#### Scenario: Invalid credentials
- **WHEN** a user submits an unknown email or a wrong password
- **THEN** the system shows a generic error in Catalan and does not reveal which field was wrong

### Requirement: Protected pages require a session
The system SHALL redirect any request to a protected page to the login page when there is no logged-in user.

#### Scenario: Anonymous access
- **WHEN** an anonymous visitor opens the dashboard URL directly
- **THEN** the system redirects them to the login page

### Requirement: Users can log out
The system SHALL end the session when the user logs out.

#### Scenario: Logout
- **WHEN** a logged-in user chooses "Tancar sessió"
- **THEN** the session is destroyed and the login page is shown
