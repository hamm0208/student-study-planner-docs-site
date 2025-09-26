---
sidebar_position: 2
---
# StudentStudyPlanner
Represents a student's personalized study planner, including their study plan, unit history, and amendments.

---
## Class Directory
```directory
src/app/class/
    ├── StudyPlanner/
        ├── StudentStudyPlanner.js
```
---
## Constructor

```js
new StudentStudyPlanner()
```
### Properties
```js
this.study_planner = new StudyPlanner();
this.required_cp = 0;
this.student_info = null;
this.unit_history = [];
this.completed_units = [];
this.uncompleted_units = [];
this.processed_completed_units = [];
this.amendments = [];
this.amendments_history = [];
```
---
## Getters & Setters
| Name                          | Returns        | Description                                                  |
| ----------------------------- | -------------- | ------------------------------------------------------------ |
| StudyPlanner (get)            | `StudyPlanner` | Returns the current `StudyPlanner` instance for the student. |
| StudyPlanner (set)            | `void`         | Sets a new `StudyPlanner` instance for the student.          |
| StudentInfo (get)             | `Object`       | Returns the student's information object.                    |
| CompletedUnits (get)          | `Array`        | Returns an array of the student's completed units.           |
| UncompletedUnits (get)        | `Array`        | Returns an array of the student's uncompleted units.         |
| ProcessedCompletedUnits (get) | `Array`        | Returns completed units grouped by year and term.            |

---
## Main Methods

### Init

Initializes the student study planner with student and planner data.

```js
async Init(student_id, planner_id)
```

#### Description

- Loads the master study planner using the provided `planner_id`.
- Checks if the master study planner is complete.
- Retrieves all units from the master planner.
- Fetches student information and unit history using the `student_id`.
- Sets required credit points and student info.
- Processes units into completed and uncompleted categories based on the student's history.
- Groups completed units by year and term.
- Adds completed and uncompleted units into the planner's year/semester structure.
- Fetches and applies any amendments made to the student's study planner.
- Initializes a new `StudyPlanner` instance with all processed data.
- Cleans the planner to remove empty semesters.
- Sets the final `study_planner` property for the student.
- Returns the initialized `StudentStudyPlanner` instance.

#### Parameters
| Name       | Type   | Description     |
| ---------- | ------ | --------------- |
| student_id | `string` | The student ID. |
| planner_id | `number` | The planner ID. |


#### Returns
| Type                | Description                                     |
| ------------------- | ----------------------------------------------- |
| `StudentStudyPlanner` | The initialized student study planner instance. |


---