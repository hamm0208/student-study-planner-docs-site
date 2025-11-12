---
sidebar_position: 3
---
# StudentStudyPlanner

Tracks a student's progress through their course, including completed and planned units.
Integrates student information and unit history from the database to form student's personal study planner.
## Class Directory
```directory
src/app/class/
	├── StudyPlanner/
		├── StudentStudyPlanner.js
```

## Constructor
```js
new StudyPlanner()
```

### Properties
```JS
this.study_planner = new StudyPlanner();
this.required_cp = 0;
this.student_info = {
	student_id: "",
	name: "",
	status: "",
	credits_completed: 0,
	course_id: 0,
	major_id: 0,
	intake_id: 0,
	};
this.unit_history = [];
this.completed_units = [];
this.uncompleted_units = [];
this.amendments = [];
this.amendments_history = [];
```

---
## Main Methods

### Init
Initializes a student’s personalized study planner from a master planner and their academic history.
```JS
Init(student_id, planner_id)
```
#### Parameters
| Name       | Type     | Description                                     |
| ---------- | -------- | ----------------------------------------------- |
| student_id | `string` | The student’s unique identifier.                |
| planner_id | `number` | The master study planner ID to initialize from. |
#### Description
- Initializes the **master study planner** by fetching all required unit and course data.
- Verifies that the master study planner is complete before proceeding.
- Fetches **student information** and **unit history**.
- Processes all units into completed and uncompleted sets.
- Groups completed units by year and term, then inserts them into the planner structure.
- Dynamically places uncompleted units into years based on the current date and previous failures.
- Applies amendments from the database (`StudentStudyPlannerAmmendmentsDB`).
- Populates the study planner with course details (course, major, intake).
- Cleans and finalizes the study planner for use.
#### Returns
| Type                | Description                                                       |
| ------------------- | ----------------------------------------------------------------- |
| StudentStudyPlanner | The fully initialized and updated student study planner instance. |

---
### ProcessUnits

Processes all units from the master planner into completed and uncompleted categories based on the student’s history.

```JS
ProcessUnits(all_units, unit_history)
```
#### Parameters
| Name         | Type    | Description                                            |
| ------------ | ------- | ------------------------------------------------------ |
| all_units    | `Array` | All units retrieved from the master study planner.     |
| unit_history | `Array` | The student’s unit history records with term and year. |
#### Description
- Initializes `completed_units` as empty and copies all units into `uncompleted_units`. 
- Sorts the student’s unit history chronologically (by **Year**, then **Term Year**, then **Term Month**).
- Performs a batch fetch of unit details for all history unit codes to optimize lookups.
- Iterates through each historical unit record and:
    1. **Case 1: Found in master planner**
        - Adds the unit to `completed_units` with term, year, and status.
        - Removes it from `uncompleted_units` if status is `"pass"`.
    2. **Case 2: MPU unit**
        - Creates a completed MPU unit with default credit points and marks it as completed.
    3. **Case 3: Not in master planner, but can replace an elective**
        - If the student passed, replaces an elective slot with this unit.
        - Populates details from batch fetch results if available, otherwise falls back to history data.
        - Removes the elective from `uncompleted_units` if passed.
    4. **Case 4: Not in master planner and no elective available**
        - Creates a completed “External” unit with elective-like type metadata.
        - Keeps `uncompleted_units` unchanged.
#### Returns
none

---
### ClearAmendments
Clears all amendments stored in the planner instance.

```JS
ClearAmendments()
```

#### Parameters
_None_
#### Description
- Resets the `amendments` property of the planner to an empty array.
#### Returns
none

---
### RemoveYearUnitAmendments
Adjusts amendment indices after a year is removed from the planner.
```JS
RemoveYearUnitAmendments(year)
```
#### Parameters
| Name | Type     | Description                             |
| ---- | -------- | --------------------------------------- |
| year | `number` | The index of the year that was removed. |
#### Description
- Iterates through all stored amendments.
- For each amendment where `yearIndex` is greater than the removed year, decrements `yearIndex` by 1.
- Ensures amendment references stay aligned with the planner’s updated year structure.
#### Returns
none

---
### RemoveSemesterUnitAmendments
Removes all units from a semester and updates amendment indices accordingly.

```JS
RemoveSemesterUnitAmendments(sem, year, semIndex)
```
#### Parameters
| Name     | Type     | Description                                                      |
| -------- | -------- | ---------------------------------------------------------------- |
| sem      | `Object` | The semester object containing its units, type, and identifiers. |
| year     | `number` | The index of the year containing the semester.                   |
| semIndex | `number` | The index of the semester within the specified year.             |
#### Description
- Iterates through all units in the given semester and calls `MakeAmendments` for each one:
    - Marks the unit as **deleted** with the associated metadata (`unit code`, `unit type`, year, semester index, semester type, semester ID).
- Iterates through all stored amendments:
    - For amendments in the same year where `semIndex` is greater than the removed semester, decrements `semIndex` by 1.
- Ensures amendment references remain consistent with the updated semester structure.
---
### MakeAmendments
Creates and stores an amendment record for a unit change in the study planner.
```JS
MakeAmendments(old_unit_code,new_unit_code,old_unit_type_id,new_unit_type_id,   year_index, sem_index,action = 'swapped',sem_type = "Long Semester",sem_id )
```
#### Parameters
| Name             | Type     | Default           | Description                                                                                 |
| ---------------- | -------- | ----------------- | ------------------------------------------------------------------------------------------- |
| old_unit_code    | `string` | `null`            | The unit code of the original unit.                                                         |
| new_unit_code    | `string` | `null`            | The unit code of the replacement unit (if applicable).                                      |
| old_unit_type_id | `number` | `null`            | The type ID of the original unit.                                                           |
| new_unit_type_id | `number` | `null`            | The type ID of the new unit.                                                                |
| year_index       | `number` | `null`            | The index of the year in the planner.                                                       |
| sem_index        | `number` | `null`            | The index of the semester in the given year.                                                |
| action           | `string` | `"swapped"`       | The action being recorded (e.g., `"swapped"`, `"deleted"`, `"replaced"`, `"changed_type"`). |
| sem_type         | `string` | `"Long Semester"` | The type of semester (e.g., `Long Semester`, `Short Semester`).                             |
| sem_id           | `number` | `null`            | The unique identifier of the semester, if available.                                        |

#### Description
- Validates the `action` parameter before proceeding.
       - Only the following values are allowed:
        - `"changed_type"`            
        - `"deleted"`
        - `"replaced"`
        - `"swapped"`
    - If an invalid action is provided, the function returns `false`.
- Constructs an amendment object containing details of the unit change.
- Checks whether an identical amendment already exists in `this.amendments`.
    - Special case: if the old unit is an **elective** with a `null` code, the amendment is always considered new.
- If no duplicate is found, the amendment is pushed into the `amendments` array.
#### Returns
| Type    | Description                                                             |
| ------- | ----------------------------------------------------------------------- |
| `void`  | Updates the `amendments` list in place if valid and not a duplicate.    |
| `false` | Returned if the provided `action` is not one of the valid action types. |

---
### SaveAmendmentsToDB
Saves the current list of amendments to the database in a formatted structure.
```JS
SaveAmendmentsToDB(amendments)
```
#### Parameters
| Name       | Type           | Description                                               |
| ---------- | -------------- | --------------------------------------------------------- |
| amendments | `Ammendment[]` | A list of amendment objects to be stored in the database. |
#### Description
- Maps amendment objects into a database-friendly format with required fields.
- Uses `sem_id` if available; otherwise falls back to `year_index` and `sem_index`.
- Calls `StudentStudyPlannerAmmendmentsDB.AddAmendment` to persist amendments.
- If the database operation is successful, clears the in-memory `amendments` list.

#### Returns
| Type    | Description                        |
| ------- | ---------------------------------- |
| `true`  | If saving to the DB is succesful   |
| `false` | If saving to the DB is unsuccesful |

---
### SolveConflicts

Attempts to automatically resolve unit conflicts in the student’s study planner.

```JS
SolveConflicts()
```

#### Parameters
none
#### Description
- Clones the current study planner to work on a temporary `unsolved_planner`. 
- Identifies all conflicting units using `GetConflictingUnitsIndex()`.
- Iteratively attempts to resolve each conflict by calling `SolveUnitConflict()`.
- Collects amendments during the solving process but only commits them if the planner is fully solvable.
- Limits conflict resolution attempts to prevent infinite loops (default `max_attempts = 20`).
- Returns the updated study planner with conflicts resolved (if successful).
#### Returns
An object with the following fields:

| Type            | Description                                                                                                                           |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| `success`       | `boolean` – `true` if all conflicts were resolved, `false` otherwise.                                                                 |
| `message`       | `string` – A summary message (e.g., "No conflicts found", "Successfully resolved X conflicts", or "Failed to resolve all conflicts"). |
| `study_planner` | `StudyPlanner` – If successful, the resolved planner. If unsuccessful, the original unsolved planner.                                 |

---

### SolveUnitConflict

Attempts to resolve a single unit conflict by detecting its type and applying the appropriate resolution strategy.

`SolveUnitConflict(planner, year_num, sem_index, unit_index, completed_units, units_offered)`

#### Parameters

| Name            | Type            | Description                                                       |
| --------------- | --------------- | ----------------------------------------------------------------- |
| planner         | `StudyPlanner`  | The planner instance containing the conflicting unit.             |
| year_num        | `number`        | The academic year in which the conflict occurs.                   |
| sem_index       | `number`        | The index of the semester within the year.                        |
| unit_index      | `number`        | The index of the unit within the semester.                        |
| completed_units | `Array<Object>` | A list of completed units (with codes) for prerequisite checking. |
| units_offered   | `Array<Object>` | A list of all units offered (used to check availability).         |

#### Description
- Locates the unit at the given year, semester, and index.
- Checks if the unit has a conflict (`has_conflict`) or is not offered.
- Detects the conflict type using `DetectConflictType()`.
- Resolves the conflict based on type:
    - **`prerequisite` / `corequisite` / `antirequisite` / `min_cp`** → handled by `SolveComplexRequisiteConflict()`.
    - **`availability`** → handled by `SolveAvailabilityConflict()`.
    - **Other / unknown types** → resolution not attempted.
- Returns `true` if the conflict was successfully resolved, otherwise `false`.
#### Returns
| Type      | Description                                                       |
| --------- | ----------------------------------------------------------------- |
| `boolean` | `true` if the conflict was resolved successfully, `false` if not. |

---

### SolveAvailabilityConflict
Attempts to resolve unit availability conflicts by moving or swapping the unit into semesters where it is offered.
```JS
SolveAvailabilityConflict(planner, year_num, sem_index, unit_index, units_offered, priortise_later_sems = false)
```
#### Parameters
| Name                 | Type            | Description                                                                                          |
| -------------------- | --------------- | ---------------------------------------------------------------------------------------------------- |
| planner              | `StudyPlanner`  | The planner instance containing the conflict.                                                        |
| year_num             | `number`        | The academic year where the conflict occurs.                                                         |
| sem_index            | `number`        | The index of the semester within the year.                                                           |
| unit_index           | `number`        | The index of the unit within the semester.                                                           |
| units_offered        | `Array<Object>` | A list of all units offered, with their availability by term.                                        |
| priortise_later_sems | `boolean`       | If `true`, prefers moving the unit to later semesters when resolving conflicts. Defaults to `false`. |
#### Description
- Identifies semesters where the conflicted unit is actually offered.
- Iterates through candidate semesters to attempt swaps with units already in those semesters.
- Evaluates swaps based on:
    - Whether both swapped units are conflict-free after the move.
    - The total number of conflicts across the planner (minimizing conflicts is preferred).
    - Preference rules:
        - Elective slots are preferred as swap targets.
        - If conflicts are tied, proximity or direction (earlier vs later semesters) is used as a tie-breaker.
- Applies the **best swap** found and records amendments for both units using `MakeAmendments`.
- Returns `true` if a valid swap was applied, otherwise `false`.
#### Returns
| Type      | Description                                                                         |
| --------- | ----------------------------------------------------------------------------------- |
| `boolean` | `true` if the conflict was resolved by moving/swapping the unit, otherwise `false`. |

---
### SolveComplexRequisiteConflict
Attempts to resolve complex requisite conflicts (prerequisites, corequisites, antirequisites, minimum credit points) by analyzing **AND/OR groups** of requisites and targeting unsatisfied conditions.

```JS
SolveComplexRequisiteConflict(planner, year_num, sem_index, unit_index, completed_codes, units_offered)
```

#### Parameters
| Name            | Type            | Description                                                   |
| --------------- | --------------- | ------------------------------------------------------------- |
| planner         | `StudyPlanner`  | The planner instance containing the unit with conflicts.      |
| year_num        | `number`        | The academic year where the unit is scheduled.                |
| sem_index       | `number`        | The index of the semester within the year.                    |
| unit_index      | `number`        | The index of the unit within the semester.                    |
| completed_codes | `string[]`      | List of unit codes that have been completed by the student.   |
| units_offered   | `Array<Object>` | A list of all units offered, with their availability by term. |
#### Description

- Retrieves the target unit and its requisites.
- Groups requisites into **logical sets** using `GroupRequisitesByOperator`, distinguishing between **AND** (all must be satisfied) and **OR** (at least one must be satisfied).
- Evaluates each requisite with `EvaluateSingleRequisite` to determine whether it is satisfied given the student’s completed units and planner state.
- If all groups are satisfied → returns `true` immediately (no conflict remains).
- For **unsatisfied groups**, iterates through each missing requisite and attempts targeted conflict resolution via specialized solvers:
    - `SolvePrerequisiteConflict` for prerequisites.
    - `SolveCorequisiteConflict` for corequisites.
    - `SolveAntirequisiteConflict` for antirequisites.
    - `SolveMinCPConflict` for minimum credit point requirements.
- Returns `true` as soon as one of the unsatisfied requisites is successfully resolved.
- If no unsatisfied groups can be resolved, returns `false`.
#### Returns
| Type      | Description                                                                            |
| --------- | -------------------------------------------------------------------------------------- |
| `boolean` | `true` if at least one unsatisfied requisite conflict was resolved, otherwise `false`. |

---
### SolveAntirequisiteConflict

Attempts to resolve **antirequisite conflicts** by moving the current unit into a different semester where it does not conflict with its antirequisite unit.

```JS
SolveAntirequisiteConflict(planner, year_num, sem_index, unit_index, completed_codes, units_offered)
```

#### Parameters

| Name            | Type            | Description                                                |
| --------------- | --------------- | ---------------------------------------------------------- |
| planner         | `StudyPlanner`  | The planner instance containing all years and semesters.   |
| year_num        | `number`        | The academic year where the conflicting unit is scheduled. |
| sem_index       | `number`        | The semester index within the year.                        |
| unit_index      | `number`        | The index of the conflicting unit in the semester.         |
| completed_codes | `string[]`      | List of unit codes already completed.                      |
| units_offered   | `Array<Object>` | List of units offered, with availability by term.          |
#### Description
- Identifies **antirequisite requirements** of the current unit.
- Locates the **conflicting unit** in the same semester.
- Finds alternative semesters where the current unit is offered.
- Attempts to **swap** the unit into a valid semester, optionally with another unit (including electives).
- If the swap removes the antirequisite conflict, updates the planner and records amendments.
#### Returns
| Type      | Description                                                          |
| --------- | -------------------------------------------------------------------- |
| `boolean` | `true` if the conflict was successfully resolved, otherwise `false`. |

---
### SolveCorequisiteConflict

Ensures that **corequisite units** are scheduled in the same semester by swapping units when necessary.

```JS
SolveCorequisiteConflict(planner, year_num, sem_index, unit_index, completed_codes, units_offered)
```

#### Parameters

| Name            | Type            | Description                                              |
| --------------- | --------------- | -------------------------------------------------------- |
| planner         | `StudyPlanner`  | The planner instance containing all years and semesters. |
| year_num        | `number`        | The academic year of the unit.                           |
| sem_index       | `number`        | The semester index within the year.                      |
| unit_index      | number          | The index of the unit within the semester.               |
| completed_codes | `string[]`      | List of unit codes already completed.                    |
| units_offered   | `Array<Object>` | List of units offered with availability by term.         |
#### Description
- Finds the **corequisite units** required by the target unit.
- If the corequisite is not in the same semester:
    - Attempts to **swap units** so both units are placed in the same semester.
    - If direct swap is not possible, attempts swaps into later semesters.
- Updates the planner and records amendments if successful.
#### Returns
| Type      | Description                                                         |
| --------- | ------------------------------------------------------------------- |
| `boolean` | `true` if the corequisite conflict was resolved, otherwise `false`. |

---
### SolveMinCPConflict

Attempts to resolve **minimum credit point (CP) conflicts** by swapping the current unit with one in later semesters, prioritizing electives.

```JS
SolveMinCPConflict(planner, year_num, sem_index, unit_index, req, completed_codes, units_offered)
```

#### Parameters

| Name            | Type            | Description                                           |
| --------------- | --------------- | ----------------------------------------------------- |
| planner         | `StudyPlanner`  | The planner instance containing years and semesters.  |
| year_num        | `number`        | The academic year where the unit is located.          |
| sem_index       | `number`        | The semester index of the unit.                       |
| unit_index      | `number`        | The index of the unit within the semester.            |
| req             | `Object`        | The minimum credit point requirement to be satisfied. |
| completed_codes | `string[]`      | List of completed unit codes.                         |
| units_offered   | `Array<Object>` | List of all units offered by term.                    |

#### Description

- Scans later semesters for potential swap candidates.
- Prioritizes swapping with **elective units** before other types.
- Uses `EvaluateMinCP` to check if the swap satisfies the minimum CP requirement.
- If a valid swap is found:
    - Updates the planner with the swapped units.
    - Records amendments for both units.
- Returns `false` if no valid swap is possible.

#### Returns
| Type      | Description                                                                   |
| --------- | ----------------------------------------------------------------------------- |
| `boolean` | `true` if a swap successfully resolves the minCP conflict, otherwise `false`. |


---

### GetAllPassedAndPlannedUnitsRatioByUnitType

Generates a summary of unit types in the planner, showing counts and ratios of **passed** and **planned** units.

```JS
GetAllPassedAndPlannedUnitsRatioByUnitType()
```
#### Parameters
_None_
#### Description
- Iterates through all units in the planner. 
- Groups units by their **unit type**.    
- Counts the number of:
    - `passed` units.
    - `planned` units (including units with no status).
    - `total` units.
- Calculates ratio `passedAndPlanned / total`.
- Includes the **unit type color** if available.
#### Returns

| Type     | Description                                                                                                                                                                                                                                                                                                           |
| -------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Object` | An object keyed by unit type name, where each value contains:  <br/>- `passed`: number of passed units  <br/>- `planned`: number of planned units  <br/>- `passedAndPlanned`: combined total  <br/>- `total`: total number of units  <br/>- `ratio`: formatted string `"X/Y"`  <br/>- `color`: hex color code for the type. |
