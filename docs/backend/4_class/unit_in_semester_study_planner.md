# UnitInSemesterStudyPlanner

The `UnitInSemesterStudyPlanner` class represents a unit (subject/module) scheduled within a specific semester of a study planner year. It encapsulates details such as the unit's type, code, and its association with a semester.

---
## Class Directory
```directory
src/app/class/
	├── UnitInSemesterStudyPlanner/
		├── UnitInSemesterStudyPlanner.js
```
---
## Constructor

```js
new UnitInSemesterStudyPlanner({
    id,
    unit_type_id,
    unit_code,
    semester_in_study_planner_year_id
})
```

### Parameters

| Name                           | Type    | Description                                                        |
|--------------------------------|---------|--------------------------------------------------------------------|
| id                             | `number`  | Unique identifier for the unit in the semester. Optional.          |
| unit_type_id                   | `number`  | Identifier for the type of unit (e.g., core, elective).            |
| unit_code                      | `string`  | Code representing the unit.                                        |
| semester_in_study_planner_year_id | `number` | ID of the associated semester in the study planner year.           |

---

## Getters and Setters

- **id**: Gets or sets the unique identifier for the unit in the semester.
- **unit_type_id**: Gets or sets the type identifier for the unit.
- **unit_code**: Gets or sets the code representing the unit.
- **semester_in_study_planner_year_id**: Gets or sets the ID of the associated semester in the study planner year.

---
## Example Usage

```js
import UnitInSemesterStudyPlanner from './UnitInSemesterStudyPlanner';

const unit = new UnitInSemesterStudyPlanner({
    id: 1,
    unit_type_id: 2,
    unit_code: 'COS20045',
    semester_in_study_planner_year_id: 5
});

console.log(unit.unit_code); // 'COMP101'
console.log(unit.semester_in_study_planner_year_id); // 5
```

---

## Export

The class is exported as the default export from the module.

```js
export default UnitInSemesterStudyPlanner;
```