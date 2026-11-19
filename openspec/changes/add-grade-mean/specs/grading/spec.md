## Purpose
Defines how teachers enter grades and how the app computes the grade shown per student and evaluable item.

## ADDED Requirements

### Requirement: Assigned teachers grade students
The system SHALL allow a teacher to grade a student on an item only when the teacher, the student and the item are all assigned to the same project.

#### Scenario: Assigned teacher grades
- **WHEN** a teacher assigned to project P grades a student of P on an item of P with 7,5
- **THEN** the grade is stored as 7.5

#### Scenario: Unassigned teacher is refused
- **WHEN** a teacher not assigned to project P tries to grade a student of P
- **THEN** the server rejects the request with a permission error, even if the form was submitted manually

#### Scenario: Admin cannot grade
- **WHEN** the admin opens the grading page
- **THEN** no grading form is shown and grade submissions from the admin are rejected

### Requirement: Grades use a 0 to 10 scale with one decimal
The system SHALL accept grades from 0 to 10 with at most one decimal, accepting a comma or a dot as decimal separator.

#### Scenario: Two decimals rejected
- **WHEN** a teacher submits 7,25
- **THEN** the system rejects it and shows a validation message in Catalan

### Requirement: The shown grade is the teachers' mean
The system SHALL show, for each student and item, the arithmetic mean of all teachers' grades rounded to one decimal, and SHALL show an empty value when there are no grades.

#### Scenario: Two teachers
- **WHEN** teacher A gives 6 and teacher B gives 9 to the same student and item
- **THEN** the student sees 7.5

#### Scenario: No grades yet
- **WHEN** no teacher has graded a student on an item
- **THEN** the cell is empty and is not counted as 0
