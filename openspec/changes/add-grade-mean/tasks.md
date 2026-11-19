# Tasks

## 1. Data

- [ ] 1.1 Add the grades table with a unique (teacher, student, item) key in a migration and verify it applies to an existing volume
- [ ] 1.2 Add Records methods to upsert a grade and read ROUND(AVG(grade), 1) per student and item, verified by a MariaDB integration test

## 2. Rules

- [ ] 2.1 Implement Access::canGrade (teacher + three assignments) and verify with PHPUnit cases for assigned, unassigned and admin
- [ ] 2.2 Implement Grades::normalize (comma or dot, max one decimal, 0–10) and verify with PHPUnit cases for 7,5, 7.5, 7,25 and 11

## 3. Interface

- [ ] 3.1 Build the grading form and the grades table in Catalan, empty cells for no grades, and verify manually on narrow and wide screens
- [ ] 3.2 Reject forged submissions on the server and verify with an HTTP test posting as an unassigned teacher

## 4. Delivery

- [ ] 4.1 Run composer test and confirm CI is green on the PR
- [ ] 4.2 Open the PR with Closes #14, screenshots of the grades page, and get one teammate approval

## Workflow follow-up

- After merge: `openspec archive add-grade-mean` and move the issue to Done.
