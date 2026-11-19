# Proposal

Issue: #14 — [feat] Show teachers' mean grade per item

## Why

Several teachers can grade the same student on the same evaluable item. Students and teachers need one final number per item, and the requirements define it as the arithmetic mean of the teachers' grades.

## What Changes

- Assigned teachers grade assigned students on assigned items, 0–10 with one decimal.
- The grade shown for a student and item is the mean of all teachers' grades, rounded to one decimal.
- An item with no grades shows an empty value, never 0.
- Only teachers assigned to the project can grade; the admin does not grade.

Out of scope: weighted means, grade history, exporting grades.

## Capabilities

### New Capabilities

- `grading`: entering grades and computing the mean per student and item.

### Modified Capabilities

_None._

## Impact

- `GradeController`, `Grades` domain helper, `Access::canGrade`, `grades.php` view.
- New `grades` table (see design.md for how existing databases receive it).
- Depends on `authentication` (archived change `add-user-login`).
