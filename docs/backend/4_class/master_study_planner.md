# MasterStudyPlanner
The `MasterStudyPlanner` class represents a master study planner entity, typically used to manage and track the status of a course intake for a academic program.

---
## Class Directory
```directory
src/app/class/
	├── MasterStudyPlanner/
		├── MasterStudyPlanner.js
```

---
## Constructor

```js
new MasterStudyPlanner({ id = null, course_intake_id, status })
```

### Parameters

| Name                 | Type     | Description                                                                          |
| -------------------- | -------- | ------------------------------------------------------------------------------------ |
| **id**               | `number` | The unique identifier for the master study planner.                                  |
| **course_intake_id** | `number` | The identifier for the associated course intake.                                     |
| **status**           | `string` | The current status of the master study planner (`"Empty"`, `"Complete"`, `"Draft"`). |
### Properties

| Name                 | Type     | Description                                   |
| -------------------- | -------- | --------------------------------------------- |
| **id**               | `number` | Gets or sets the planner’s unique identifier. |
| **course_intake_id** | `number` | Gets or sets the associated course intake ID. |
| **status**           | `string` | Gets or sets the planner’s status.            |

---
## Getters and Setters

- `id`: Get or set the planner's ID.
- `course_intake_id`: Get or set the course intake ID.
- `status`: Get or set the planner's status.

---

## Example Usage

```js
import MasterStudyPlanner from './MasterStudyPlanner';

const planner = new MasterStudyPlanner({
    id: 1,
    course_intake_id: 101,
    status: 'complete'
});

console.log(planner.id); // 1
console.log(planner.course_intake_id); // 101
console.log(planner.status); // 'complete'
```

---

## Export

The class is exported as the default export from the module.

```js

export default MasterStudyPlanner;

```