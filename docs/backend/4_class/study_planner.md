# StudyPlanner
The `StudyPlanner` class serves as the core model for the master study planner view page. It manages all planner logic, including adding and removing unit rows and semesters, calculating next semester intakes, and detecting unit conflicts. The class also handles backend-related calculations and state updates to ensure the planner remains accurate and up-to-date.

---
## Class Directory
```directory
src/app/class/
	├── StudyPlanner/
		├── StudyPlanner.js
```
---
## Constructor

```js
new StudyPlanner()
```

### Properties
```JS
this.details = {
	id: -1,
	status: '',
	course: {},
	intake: {},
	unit_types: [],
	recommended_electives: []
};
// Total credits counter
this.total_combined_credits = 0;
// Elective counter
this.elective_counter = 0;
this.years = [];
this.semester_state = {
	added: [], //Should the Year and the Semester type
	removed: [], //Should be the ID that it wants to be removed
}
this.units_state = {
	added: [], //The semester_id that it will be added to
	edited: [], //Only if both is existing in database, it can be edited
	removed: [], //Only if it existing in database, it can be removed
}
```
---

## Getters & Setters
| Name                 | Returns | Description                                                         |
| -------------------- | ------- | ------------------------------------------------------------------- |
| courseName           | `string`  | The name of the course.                                             |
| courseCode           | `string`  | The code of the course.                                             |
| status               | `string`  | The status of the planner.                                          |
| intakeYear           | `number`  | The intake year.                                                    |
| majorName            | `string`  | The name of the major.                                              |
| creditsRequired      | `number`  | The required credits for the course.                                |
| intakeName           | `string`  | The intake name.                                                    |
| intakeMonth          | `string`  | The intake month.                                                   |
| unit_types           | `Array`   | The array of unit types.                                            |
| recommendedElectives | `Array`   | The array of recommended electives. (No functionality included yet) |
| totalCredits         | `number`  | The total combined credits in the planner.                          |
| allYears             | `Array`   | The years array.                                                    |

---
## Main Methods

### InitDetails
Initializes the course and intake details for the study planner.

```js
InitDetails(course_name, course_code, credits_required, major_name, intake_name, intake_month, intake_year)
```
#### Parameters
| Name             | Type   | Description                                |
| ---------------- | ------ | ------------------------------------------ |
| course_name      | `string` | The name of the course.                    |
| course_code      | `string` | The code of the course.                    |
| credits_required | `number` | The total credits required for the course. |
| major_name       | `string` | The name of the major.                     |
| intake_name      | `string` | The name of the intake term.               |
| intake_month     | `string` | The month of the intake.                   |
| intake_year      | `number` | The year of the intake.                    |
#### Returns
This method does not return a value. It sets the `details.course` and `details.intake` properties of the StudyPlanner instance.

---
### async Init(master_study_planner_id)

Initializes the study planner instance with data from the backend, including course details, intake information, years, semesters, units, and unit types.
#### Description
- Fetches all relevant planner data from the backend using the provided `master_study_planner_id`.
- Sets up course and intake details.
- Builds the years, semesters, and units structure using the fetched data.
- Adds 4 empty unit rows to any semester that does not have units.
- Checks and marks unit conflicts based on requisites.
- Sets the total combined credits and unit types.
- Recalculates elective numbers for the planner.
#### Parameters
| Name                    | Type     | Description                                       |
| ----------------------- | -------- | ------------------------------------------------- |
| master_study_planner_id | `number` | The ID of the master study planner to initialize. |
#### Returns
| Type    | Description                                 |
| ------- | ------------------------------------------- |
| Promise | Resolves to `void` or `false` if not found. |

---
### GetYearByIndex

Returns the year object at the specified index.

```js
GetYearByIndex(index)
```
#### Parameters
| Name  | Type     | Description                      |
| ----- | -------- | -------------------------------- |
| index | `number` | The index of the year to return. |
#### Returns
| Type     | Description      |
| -------- | ---------------- |
| `Object` | The year object. |

---
### GetAllYearsAndSemesters

Returns a summary array of all years and their semesters, including semester type and unit count.

```js
GetAllYearsAndSemesters()
```
#### Parameters
_None_
#### Returns
| Type    | Description                                                                  |
| ------- | ---------------------------------------------------------------------------- |
| 'Array' | Array of year objects, each containing semester type and unit count summary. |

---
### GetAllUnits
Returns a flat array of all units across all years and semesters in the planner.
```js
GetAllUnits()
```
#### Parameters
_None_
#### Returns
| Type    | Description                |
| ------- | -------------------------- |
| 'Array' | Array of all unit objects. |

---
### isEmpty
Checks if the planner is empty (only one year and all semesters have no units).
```js
isEmpty()
```
#### Parameters
_None_
#### Returns
| Type      | Description                                    |
| --------- | ---------------------------------------------- |
| 'boolean' | `true` if planner is empty, `false` otherwise. |

---
### RecalculateElectiveNumbers
Recalculates and renames elective units in the planner based on their current order.
```js
RecalculateElectiveNumbers(planner)
```
#### Parameters
| Name    | Type         | Description                     |
| ------- | ------------ | ------------------------------- |
| planner | StudyPlanner | The planner instance to update. |
#### Description
- Clones the provided planner instance.
- Resets the names of all elective units that do not have a unit code.
- Assigns sequential names (`Elective 1`, `Elective 2`, ...) to all unnamed elective units.
- Updates the planner's `elective_counter` to reflect the total number of electives.
- Returns the updated planner instance.
#### Returns
| Type         | Description                                             |
| ------------ | ------------------------------------------------------- |
| StudyPlanner | The updated planner with recalculated elective numbers. |
#### Example
```js
const updatedPlanner = planner.RecalculateElectiveNumbers(planner);
```
---
### UpdateStatus
Evaluates the current state of the study planner and updates its status accordingly.
```js
UpdateStatus()
```
#### Description
- Checks if the planner is empty.
- Checks for any empty unit slots or missing unit types.
- Verifies if the total combined credits meet the required credits for the course.
- Sets the planner status to `"Empty"`, `"Draft"`, or `"Complete"` based on these checks.
- Returns an object containing the status, a message, and a boolean indicating if the planner is complete.
#### Returns
| Property    | Type    | Description                                           |
| ----------- | ------- | ----------------------------------------------------- |
| status      | string  | The updated status of the planner.                    |
| message     | string  | A message describing the current planner state.       |
| is_complete | boolean | `true` if the planner is complete, otherwise `false`. |

---
### IsFullyCompleted
Checks if the study planner is fully completed.

```js
IsFullyCompleted()
```
#### Description
- Returns `true` only if:
  - The total combined credits exactly match the required credits.
  - All unit slots are filled.
  - No units have conflicts.
  - All units are offered in their respective semesters.
#### Returns
| Type      | Description                                                  |
| --------- | ------------------------------------------------------------ |
| `boolean` | `true` if the planner is fully completed, `false` otherwise. |

---
### AddNewYear

Adds a new year to the study planner, automatically incrementing from the latest year.

```js
AddNewYear()
```

#### Description
- Finds the latest year number in the planner.
- Creates a new year object with the next year number and an empty semesters array.
- Clones the current planner state.
- Adds the new year to the planner's years array.

#### Returns
| Type     | Description                                           |
| -------- | ----------------------------------------------------- |
| `Object` | The updated planner instance with the new year added. |
#### Example
```js
const updatedPlanner = planner.AddNewYear();
```
---
### RemoveYear
Removes a year from the study planner and shifts all subsequent years up by one.
```js
RemoveYear(yearNumber)
```
#### Parameters
| Name       | Type     | Description                |
| ---------- | -------- | -------------------------- |
| yearNumber | `number` | The year number to remove. |
#### Description
- Removes the specified year and all its semesters and units.
- Shifts the year numbers of all subsequent years down by one.
- Updates semester and unit states to reflect the removal.
- Recalculates semester intake months/years and names for all affected semesters.
- Checks and updates unit offering and conflict status for all affected units.
#### Returns
| Type    | Description                                                                                         |
| ------- | --------------------------------------------------------------------------------------------------- |
| Promise | Resolves to the updated planner instance, or `false` if the year is not found or is the first year. |
#### Example
```js
const updatedPlanner = planner.RemoveYear(2);
```

---
### AddNewSemester
Adds a new semester to a specific year, following the semester scheduling rules.
```js
AddNewSemester(yearNumber, semType, semester = null, auto_add_units = true, master_mode = true)
```
#### Parameters
| Name           | Type      | Optional | Description                                                                     |
| -------------- | --------- | -------- | ------------------------------------------------------------------------------- |
| yearNumber     | `number`  | No       | The year number to add the semester to.                                         |
| semType        | `string`  | No       | Type of semester ("Long Semester" or "Short Semester").                         |
| semester       | `Object`  | Yes      | Semester object to use for initialization.                                      |
| auto_add_units | `boolean` | Yes      | If true, automatically adds 4 unit rows to the new semester. Default is `true`. |
| master_mode    | `boolean` | Yes      | If true, uses master mode logic for conflicts and offerings. Default is `true`. |
#### Description
- Validates semester limits (max 2 long and 2 short semesters per year).
- Determines the next semester's intake month and year based on scheduling rules.
- Sets the semester term and name according to intake and type.
- Creates and appends the new semester to the specified year.
- Recalculates intake, term, and name for all subsequent semesters.
- Checks for unit conflicts and offerings in all affected semesters.
- Updates planner state to reflect the addition.
- Optionally adds 4 empty unit rows to the new semester.
#### Returns
| Type     | Description                                               |
| -------- | --------------------------------------------------------- |
| `Object` | The updated planner instance with the new semester added. |
#### Throws
| Type    | Description                                                                        |
| ------- | ---------------------------------------------------------------------------------- |
| `Error` | If the year is not found or the semester cannot be added due to scheduling limits. |
#### Example
```js
const updatedPlanner = planner.AddNewSemester(2, "Long Semester");
```
---
### RemoveSemester
Removes a semester from a specific year in the study planner.
```js
RemoveSemester(yearNumber, semesterIndex, master_mode = true)
```
#### Parameters
| Name          | Type      | Description                                                             |
| ------------- | --------- | ----------------------------------------------------------------------- |
| yearNumber    | `number`  | The year number from which to remove the semester.                      |
| semesterIndex | `number`  | The index of the semester to remove (0-based).                          |
| master_mode   | `boolean` | (Optional) If true, uses master mode logic for conflicts and offerings. |
#### Description
- Throws an error if trying to remove the first semester of the first year.
- Removes the specified semester and deducts its credit points.
- Removes unused unit types.
- If the year becomes empty (except first year), removes the year and shifts subsequent years up by one.
- Recalculates intake, term, and name for all affected semesters after the removal.
- Checks and updates unit offering and conflict status for all affected units.
- Updates semester and unit states to reflect the removal.
- Shifts semester indices and unit indices for all subsequent semesters and units in the same year.

#### Returns
| Type      | Description                                             |
| --------- | ------------------------------------------------------- |
| '`Object` | The updated planner instance with the semester removed. |
#### Throws
| Type    | Description                                                                                        |
| ------- | -------------------------------------------------------------------------------------------------- |
| `Error` | If the year or semester is not found, or if trying to remove the first semester of the first year. |
#### Example
```js
const updatedPlanner = planner.RemoveSemester(2, 1);
```
---
### AddNewUnitRowIntoSemester
Adds a new empty unit row to a specific semester in a given year.
```js
AddNewUnitRowIntoSemester(year, sem_index, master_mode = true, append = true)
```
#### Parameters
| Name        | Type      | Description                                                                                              |
| ----------- | --------- | -------------------------------------------------------------------------------------------------------- |
| year        | `number`  | The year number to add the unit row to.                                                                  |
| sem_index   | `number`  | The index of the semester to add the unit row to (0-based).                                              |
| master_mode | `boolean` | (Optional) If true, uses master mode logic for unit status. Default is `true`.                           |
| append      | `boolean` | (Optional) If true, appends the unit row at the end; otherwise, inserts at the start. Default is `true`. |
#### Description
- Finds the target year and semester.
- Creates a new unit row with default values.
- Adds the new unit row to the semester (either at the end or start).
- Updates the planner's `units_state.added` to track the new unit row.
- Returns the updated planner instance.
#### Returns
| Type     | Description                                               |
| -------- | --------------------------------------------------------- |
| `Object` | The updated planner instance with the new unit row added. |
#### Throws
| Type    | Description                           |
| ------- | ------------------------------------- |
| `Error` | If the year or semester is not found. |
#### Example

```js
const updatedPlanner = planner.AddNewUnitRowIntoSemester(2, 1);
```
---
### EditUnitInUnitRow
Edits the data of a unit row in a specific semester and year.
```js
EditUnitInUnitRow(year, sem_index, unit_index, unit_data, status = null, master_mode = true)
```
#### Parameters
| Name        | Type      | Description                                                                  |
| ----------- | --------- | ---------------------------------------------------------------------------- |
| year        | `number`  | The year number containing the semester.                                     |
| sem_index   | `number`  | The index of the semester (0-based).                                         |
| unit_index  | `number`  | The index of the unit row to edit (0-based).                                 |
| unit_data   | `Object`  | The new unit data to set (type, code, name, credit points, requisites, etc). |
| status      | `string`  | (Optional) The status of the unit ("planned", "pass", "fail").               |
| master_mode | `boolean` | (Optional) If true, uses master mode logic for credit points and conflicts.  |
#### Description
- Finds the target year, semester, and unit.
- Updates the unit's type, code, name, credit points, requisites, and offered/conflict status.
- Handles elective units and sets default credit points and availability.
- Updates total combined credits.
- Updates `units_state.edited` and `units_state.added` as needed.
- Removes unused unit types.
- Checks for conflicts in all subsequent units.
- Recalculates elective numbers.
- Returns the updated planner instance.

#### Returns
| Type     | Description                                            |
| -------- | ------------------------------------------------------ |
| `Object` | The updated planner instance with the unit row edited. |
#### Throws
| Type    | Description                                  |
| ------- | -------------------------------------------- |
| `Error` | If the year, semester, or unit is not found. |
#### Example

```js
const updatedPlanner = planner.EditUnitInUnitRow(2, 1, 0, unitData, "planned");
```

---
### DeleteUnitRowFromSemester

Deletes a unit row from a specific semester in a given year.

```js
DeleteUnitRowFromSemester(year, sem_index, unit_index)
```
#### Parameters
| Name       | Type     | Description                                    |
| ---------- | -------- | ---------------------------------------------- |
| year       | `number` | The year number containing the semester.       |
| sem_index  | `number` | The index of the semester (0-based).           |
| unit_index | `number` | The index of the unit row to delete (0-based). |
#### Description
- Clones the current planner state.
- Finds the target year, semester, and unit.
- Removes the unit row and deducts its credit points from the total.
- Updates `units_state.removed` and `units_state.edited` if the unit exists in the database.
- Removes unused unit types if necessary.
- Updates `units_state.added` to remove the unit and adjust indices for subsequent units.
- Checks for conflicts in all subsequent units.
- Recalculates elective numbers.
- Returns the updated planner instance.
#### Returns
| Type     | Description                                             |
| -------- | ------------------------------------------------------- |
| `Object` | The updated planner instance with the unit row deleted. |
#### Throws
| Type    | Description                                  |
| ------- | -------------------------------------------- |
| `Error` | If the year, semester, or unit is not found. |
#### Example

```js
const updatedPlanner = planner.DeleteUnitRowFromSemester(2, 1, 0);
```
---
### GetConflictingUnitsIndex
Returns an array of all units with conflicts or not offered, including their year, semester, and unit index.
```js
GetConflictingUnitsIndex()
```
#### Description
- Iterates through all years and semesters in the planner.
- Skips semesters marked as completed (`sem_completed`).
- For each unit, checks if it has a conflict (`has_conflict`) or is not offered (`is_offered` is `false`).
- Collects the indices of these units, including the year number, semester index, and unit index.
#### Returns
| Type  | Description                                                                       |
| ----- | --------------------------------------------------------------------------------- |
| Array | Array of objects `{ year_num, sem_index, unit_index }` for each conflicting unit. |

---
### SwapUnits
Swaps two units between semesters and years, updating their data and resolving conflicts.
```js
SwapUnits(sourceYear, sourceSemIndex, sourceUnitIndex, targetYear, targetSemIndex, targetUnitIndex, master_mode = true)
```
#### Parameters
| Name            | Type      | Description                                     |
| --------------- | --------- | ----------------------------------------------- |
| sourceYear      | `number`  | Year of the source unit.                        |
| sourceSemIndex  | `number`  | Semester index of the source unit.              |
| sourceUnitIndex | `number`  | Unit index of the source unit.                  |
| targetYear      | `number`  | Year of the target unit.                        |
| targetSemIndex  | `number`  | Semester index of the target unit.              |
| targetUnitIndex | `number`  | Unit index of the target unit.                  |
| master_mode     | `boolean` | (Optional) Use master mode for conflict checks. |
#### Description
- Finds the source and target units by their year, semester, and unit indices.
- Prepares the unit data for both source and target units.
- If the target unit slot is empty, moves the source unit to the target slot and deletes the source unit.
- If both slots are filled, swaps the unit data between the two slots.
- Updates unit offering and conflict status as needed.
- Returns the updated planner instance after the swap.
#### Returns
| Type     | Description                              |
| -------- | ---------------------------------------- |
| `Object` | Updated planner instance after the swap. |
#### Throws
| Type    | Description                                  |
| ------- | -------------------------------------------- |
| `Error` | If any year, semester, or unit is not found. |

---
### GetUnitPosition
Finds the position of a unit by code, or finds the first empty slot if requested.
```js
GetUnitPosition(unitCode = null, master_mode = true, get_empty = false)
```
#### Parameters
| Name        | Type      | Description                                     |
| ----------- | --------- | ----------------------------------------------- |
| unitCode    | `string`  | The code of the unit to find. (null = elective) |
| master_mode | `boolean` | (Optional) Use master mode for planned status.  |
| get_empty   | `boolean` | (Optional) If true, returns first empty slot.   |
#### Description
- Iterates through all years, semesters, and units in the planner.
- If `get_empty` is `true`, returns the position of the first empty unit slot (where `unit_type` is missing or `unit.name` is `null`).
- If `unitCode` is provided, returns the position of the unit with the matching code.
  - In master mode, returns any matching unit.
  - In non-master mode, only returns units with status `"planned"`.
- If no matching unit or empty slot is found, returns `null`.
#### Returns
| Type     | Description                                                 |
| -------- | ----------------------------------------------------------- |
| `Object` | `{ year, semesterIndex, unitIndex }` if found, else `null`. |

---
### GetUnit
Returns the unit object for a given unit code.
```js
GetUnit(unitCode, master_mode = true)
```
#### Parameters
| Name        | Type      | Description                                    |
| ----------- | --------- | ---------------------------------------------- |
| unitCode    | `string`  | The code of the unit to find.                  |
| master_mode | `boolean` | (Optional) Use master mode for planned status. |
#### Description
- Iterates through all years, semesters, and units in the planner.
- If a unit with the matching code is found:
  - In master mode, returns the unit regardless of its status.
  - In non-master mode, only returns the unit if its status is `"planned"`.
- If no matching unit is found, returns `null`.
#### Returns
| Type     | Description                       |
| -------- | --------------------------------- |
| `Object` | `{ unit }` if found, else `null`. |

---
### CleanStudyPlanner
Removes empty semesters from the planner and updates unit conflicts and offerings.
```js
async CleanStudyPlanner(master_mode = true)
```
### Parameters
| Name        | Type      | Description                                     |
| ----------- | --------- | ----------------------------------------------- |
| master_mode | `boolean` | (Optional) Use master mode for conflict checks. |
#### Description
- Iterates through all years and semesters.
- Updates each unit's conflict (`has_conflict`) and offering (`is_offered`) status.
- Identifies semesters that do not contain any valid units (no unit code or valid elective).
- Skips the first semester of the first year (always required).
- Removes all empty semesters in reverse order to avoid index shifting issues.
- Uses `RemoveSemester` to handle semester removal and state updates.
- Returns the updated planner instance with all empty semesters removed.
#### Returns
| Type     | Description                                            |
| -------- | ------------------------------------------------------ |
| `Object` | Updated planner instance with empty semesters removed. |

---
### Clone
Creates a deep clone of the current StudyPlanner instance.
```js
Clone()
```
#### Returns
| Type           | Description                                     |
| -------------- | ----------------------------------------------- |
| `StudyPlanner` | A new StudyPlanner instance with the same data. |

---
### CheckUnitRequisites
Checks if a unit has satisfy its requisites.
```js
CheckUnitRequisites(unit, targetYear, targetSemIndex, master_mode = true)
```
#### Parameters
| Name           | Type      | Description                                     |
| -------------- | --------- | ----------------------------------------------- |
| unit           | `Object`  | The unit to check.                              |
| targetYear     | `number`  | Target year number.                             |
| targetSemIndex | `number`  | Target semester index.                          |
| master_mode    | `boolean` | (Optional) Use master mode for conflict checks. |
#### Returns
| Type   | Description                                  |
| ------ | -------------------------------------------- |
| Object | `{ isValid, messages }` result of the check. 

---
### GetAllPassedUnits
Returns all units with status "pass".
```js
GetAllPassedUnits()
```
#### Returns
| Type    | Description                   |
| ------- | ----------------------------- |
| `Array` | Array of passed unit objects. |

---

### GetAllConflicts
Returns all units with conflicts or not offered, including details.
```js
GetAllConflicts()
```
#### Returns
| Type  | Description                                           |
| ----- | ----------------------------------------------------- |
| Array | Array of objects with unit details and conflict info. |

---

### GetAllUnitTypeUsed
Returns a set of all unit type names currently used in the planner.
```js
GetAllUnitTypeUsed()
```
#### Returns
| Type | Description                    |
| ---- | ------------------------------ |
| Set  | Set of unit type names in use. |

---

### RemoveUnusedUnitTypes
Removes unit types from the planner's unit_types array that are not in use.
```js
RemoveUnusedUnitTypes(unit_typesToCheck)
```
#### Parameters
| Name              | Type       | Description                                    |
| ----------------- | ---------- | ---------------------------------------------- |
| unit_typesToCheck | `string[]` | Array of unit type names to check for removal. |

---
### RemoveAllUnusedUnitTypes
Removes all unused unit types from the planner.
```js
RemoveAllUnusedUnitTypes()
```

---

### async SaveToDB
Saves the current planner state to the database, including semesters and units.
```js
async SaveToDB()
```

#### Description
- Updates the planner's status and checks if it is complete or empty.
- Updates the main study planner status in the database.
- Updates the course intake status if the planner is not complete.
- Updates the first semester type in the database.
- Removes semesters and units marked for deletion from the database.
- Adds new semesters and units to the database.
- Updates existing units in the database.
- Handles all database operations for semesters and units in the planner.
- Returns a success or failure message based on the outcome of the database operations.
#### Returns
| Type   | Description                                          |
| ------ | ---------------------------------------------------- |
| Object | `{ success, message }` result of the save operation. |

---
## Helper Functions
These are defined outside the class and used for utility logic:
- `FetchMasterPlannerContext(master_study_planner_id)`
- `BuildYearsFromData({...})`
- `CheckMinCP(years, req, targetYear, targetSemIndex, master_mode)`
- `CheckPrerequisite(years, req, targetYear, targetSemIndex, master_mode)`
- `CheckCorequisite(years, req, targetYear, targetSemIndex, master_mode)`
- `CheckAntirequisite(years, req, targetYear, targetSemIndex, master_mode)`
- `DetermineNextIntake(last_intake_month, last_intake_year, semType)`
- `FindLastSemesterFromPreviousYears(startYear, years)`

---
## Example Usage

```js
import StudyPlanner from './StudyPlanner';

const planner = new StudyPlanner();
await planner.Init(master_study_planner_id);
planner.AddNewYear();
planner.AddNewSemester(2, "Long Semester");
planner.SaveToDB();
```

---
## Export
The class is exported as the default export from the module.
```js
export default StudyPlanner;
```