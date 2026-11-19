# Design

## Context

See proposal.md — Why. Authentication and role checks already exist (`authentication` spec). Projects, items and assignments are managed by the admin.

## Goals / Non-Goals

**Goals:** one source of truth for the mean, server-side permission checks, empty ≠ zero.

**Non-Goals:** caching means, weighted grades.

## Decisions

- **Mean computed in SQL with `AVG`** rather than in PHP: the number on screen is exactly the number the database computed, and `AVG` over zero rows returns `NULL`, which maps naturally to "empty".
- **`Access::canGrade` in the Domain layer**: one pure function checks role + three assignments, reusable by controller and tests.
- **`Grades::normalize`** accepts `7,5` and `7.5`, rejects more than one decimal, so validation is unit-testable without HTTP.
- **One row per (teacher, student, item)** with a unique key: a teacher re-grading updates their own row instead of adding a new one.

## Risks / Trade-offs

- [Existing database volumes do not run init SQL again] → ship a migration script `database/migrations/014_grades.sql` and document the command in the PR.
- [Rounding differences between SQL and PHP] → round only once, in SQL (`ROUND(AVG(grade), 1)`).
