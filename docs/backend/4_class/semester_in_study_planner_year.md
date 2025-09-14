# SemesterInStudyPlannerYear

The `SemesterInStudyPlannerYear` class models a semester within a specific year of a master study planner. It encapsulates properties such as the semester's ID, associated master study planner, year, and semester type.

---
## Class Directory
```directory
src/app/class/
	├── SemesterInStudyPlannerYear/
		├── SemesterInStudyPlannerYear.js
```

---
## Constructor

```js
new SemesterInStudyPlannerYear({ id = null, master_study_planner_id, year, sem_type })
```

### Parameters

| Name                    | Type   | Description                                                | Optional | Example           |
| ----------------------- | ------ | ---------------------------------------------------------- | -------- | ----------------- |
| id                      | `number` | Unique identifier for the semester.                        | Yes      | `1`               |
| master_study_planner_id | `number` | ID of the associated master study planner.                 | No       | `101`             |
| year                    | `number` | The academic year for this semester.                       | No       | `2025`            |
| sem_type                | `string` | Type of semester (e.g., "Long Semester", "Short Semester") | No       | `"Long Semester"` |
## Getters and Setters

- **id** (`number|null`): Gets or sets the semester's unique identifier.
- **master_study_planner_id** (`number`): Gets or sets the associated master study planner ID.
- **year** (`number`): Gets or sets the academic year.
- **sem_type** (`string`): Gets or sets the semester type.

---

## Example Usage

```js
import SemesterInStudyPlannerYear from './SemesterInStudyPlannerYear';

const semester = new SemesterInStudyPlannerYear({
    id: 1,
    master_study_planner_id: 101,
    year: 2025,
    sem_type: 'Long Semester'
});

console.log(semester.year); // 2025
console.log(semester.sem_type); // 'Long Semester'
```

---

## Export

The class is exported as the default export from the module.

```js
export default SemesterInStudyPlannerYear;
```