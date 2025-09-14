# StudentStudyPlannerAmmendments

The `StudentStudyPlannerAmmendments` class represents an amendment made to a student's study planner, such as adding, removing, or changing a unit. It encapsulates all relevant details about the amendment, including the affected unit, action type, and timing.

---
## Class Directory
```directory
src/app/class/
	├── StudentStudyPlannerAmmendments/
		├── StudentStudyPlannerAmmendments.js
```
---

## Constructor

```js
new StudentStudyPlannerAmmendments({
    id,
    student_id,
    unit_code,
    new_unit_code,
    action,
    time_of_action,
    new_unit_type_id,
    year,
    sem_index,
    sem_type
})
```

### Parameters

| Name              | Type      | Description                                                        |
|-------------------|-----------|--------------------------------------------------------------------|
| id                | `number`    | Unique identifier for the amendment. Optional.                     |
| student_id        | `string`    | Student's unique identifier. Optional.                             |
| unit_code         | `string`    | Code of the unit being amended. Optional.                          |
| new_unit_code     | `string`    | New unit code if the unit is being changed. Optional.              |
| action            | `string`    | Type of action (e.g., "add", "remove", "change"). Optional.        |
| time_of_action    | `string`    | Timestamp of when the action occurred. Optional.                   |
| new_unit_type_id  | `number`    | ID of the new unit type if changed. Optional.                      |
| year              | `number`    | Academic year of the amendment. Optional.                          |
| sem_index         | `number`    | Index of the semester in the year. Optional.                       |
| sem_type          | `string`    | Type of semester (e.g., "Long Semester", "Short Semester"). Optional. |

---

## Getters and Setters

- **id**: Gets or sets the amendment's unique identifier.
- **student_id**: Gets or sets the student's unique identifier.
- **unit_code**: Gets or sets the code of the unit being amended.
- **new_unit_code**: Gets or sets the new unit code if changed.
- **action**: Gets or sets the type of action performed.
- **time_of_action**: Gets or sets the timestamp of the action.
- **new_unit_type_id**: Gets or sets the new unit type ID if changed.
- **year**: Gets or sets the academic year of the amendment.
- **sem_index**: Gets or sets the semester index.
- **sem_type**: Gets or sets the type of semester.
- **term_id**: Gets or sets the term ID (if applicable).

---

## Example Usage

```js
import StudentStudyPlannerAmmendments from './StudentStudyPlannerAmmendments';

const amendment = new StudentStudyPlannerAmmendments({
    id: 1,
    student_id: 'S12345',
    unit_code: 'COMP101',
    new_unit_code: 'COMP102',
    action: 'change',
    time_of_action: '2025-09-11T10:00:00Z',
    new_unit_type_id: 2,
    year: 2025,
    sem_index: 1,
    sem_type: 'Long Semester'
});

console.log(amendment.action); // 'change'
console.log(amendment.unit_code); // 'COMP101'
```

---

## Export

The class is exported as the default export from the module.
```js
export default StudentStudyPlannerAmmendments;
```