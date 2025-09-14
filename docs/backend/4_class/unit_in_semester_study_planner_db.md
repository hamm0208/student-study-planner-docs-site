# UnitInSemesterStudyPlannerDB
`UnitInSemesterStudyPlannerDB` is a static utility class for interacting with the Unit In Semester Study Planner API. It provides methods to fetch, add, update, and delete unit records associated with a semester in a study planner year.

---
Class Directory
```directory
src/app/class/
	├── UnitInSemesterStudyPlanner/
		├── UnitInSemesterStudyPlannerDB.js
```
---
## Methods

### 1. FetchUnitsInSemesterStudyPlanner

Fetches units from the server with optional query parameters.

```js
UnitInSemesterStudyPlannerDB.FetchUnitsInSemesterStudyPlanner(params)
```

#### Parameters

| Name                              | Type           | Description                                                        |
| --------------------------------- | -------------- | ------------------------------------------------------------------ |
| id                                | `number` \| `null` | Unique identifier for a specific unit. Optional.                   |
| unit_type_id                      | `number`         | Identifier for the type of unit. Optional.                         |
| unit_code                         | `string`         | Code representing the unit. Optional.                              |
| semester_in_study_planner_year_id | `number`         | ID of the associated semester in the study planner year. Optional. |
| order_by                          | `Array<Object>`  | Array of objects specifying columns and sort order. Optional.      |
| return                            | `Array<string>`  | Array of strings specifying which fields to return. Optional.      |
| exclude                           | `Array<string>`  | Array of strings specifying which fields to exclude. Optional.     |
:::note
The following query modifiers apply to all columns in the **UnitInSemesterStudyPlanner** table.  
Ensure that the names used are consistent with the database schema:

- **`.return`** → Must match the column names in the database.  
- **`.order_by`** → Must match the column names in the database.  
- **`.exclude`** → Must match the column names in the database.  
:::
#### Returns
| Type   | Description                                      |
| ------ | ------------------------------------------------ |
| `Array`  | Array of `UnitInSemesterStudyPlanner` instances. |
| `Object` | Error object if the request fails.               |

---

### 2. SaveUnitInSemesterStudyPlanner

Adds a new unit to the server.

```js
UnitInSemesterStudyPlannerDB.SaveUnitInSemesterStudyPlanner(unitData)
```

#### Parameters
| Name     | Type   | Description                  |
| -------- | ------ | ---------------------------- |
| unitData | `Object` | The unit object to be added. |
```js
unitData = {
	UnitTypeID:1, //FK to UnitType table
	UnitCode: COS20045, //FK to Unit table
	SemesterInStudyPlannerYearID: 1, //FK to SemesterInStudyPlannerYear table
}
```

#### Returns
| Property | Type    | Description                                |
| -------- | ------- | ------------------------------------------ |
| success  | `boolean` | Indicates if the operation was successful. |
| message  | `string`  | Message describing the result.             |
| data     | `any`     | The returned data from the operation.      |

---

### 3. UpdateUnitInSemesterStudyPlanner

Updates one or more units on the server.

```js
UnitInSemesterStudyPlannerDB.UpdateUnitInSemesterStudyPlanner(units)
```

#### Parameters
| Name  | Type  | Description                          |
| ----- | ----- | ------------------------------------ |
| units | `Array` | Array of unit objects to be updated. |
```js
units = [
	{
	    "unit_id": 8, //UnitInSemesterStudyPlanner ID to refer to
	    "unit_type_id": 2, //New unit type id (FK: UnitType table)
	    "unit_code": "COS10034" //New unit code (FK: UnitCode table)
	},
	
]
```

#### Returns

| Property | Type    | Description                                   |
|----------|---------|-----------------------------------------------|
| success  | `boolean` | Indicates if the operation was successful.    |
| message  | `string`  | Message describing the result.                |
| data     | `any`     | The returned data from the operation.         |

---

### 4. DeleteUnitInSemesterStudyPlanner

Deletes one or more units from the server by their IDs.

```js
UnitInSemesterStudyPlannerDB.DeleteUnitInSemesterStudyPlanner(id)
```

#### Parameters

| Name | Type           | Description                                 |
|------|----------------|---------------------------------------------|
| id   | `number` \| Array | The ID or array of IDs of units to delete. |

#### Returns

| Property | Type    | Description                                   |
|----------|---------|-----------------------------------------------|
| success  | `boolean` | Indicates if the operation was successful.    |
| message  | `string`  | Message describing the result.                |
| data     | `any`     | The returned data from the operation.         |
| results  | `Array`   | Array of individual delete results (if array input). |

---

## Example Usage

```js
import UnitInSemesterStudyPlannerDB from './UnitInSemesterStudyPlannerDB';

// Fetch units
const units = await UnitInSemesterStudyPlannerDB.FetchUnitsInSemesterStudyPlanner({ semester_in_study_planner_year_id: 5 });

// Add a unit
const addResult = await UnitInSemesterStudyPlannerDB.SaveUnitInSemesterStudyPlanner({
    unit_type_id: 2,
    unit_code: 'COS20045',
    semester_in_study_planner_year_id: 5
});

// Update units
const updateResult = await UnitInSemesterStudyPlannerDB.UpdateUnitInSemesterStudyPlanner([
    { unit_id: 1, unit_type_id: 2, unit_code: 'COS20045' }
]);

// Delete units
const deleteResult = await UnitInSemesterStudyPlannerDB.DeleteUnitInSemesterStudyPlanner([1, 2, 3]);
```

---

## Export

The class is exported as the default export from the module.

```js
export default UnitInSemesterStudyPlannerDB;
```