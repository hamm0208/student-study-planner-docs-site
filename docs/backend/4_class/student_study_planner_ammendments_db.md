# StudentStudyPlannerAmmendmentsDB

`StudentStudyPlannerAmmendmentsDB` is a static utility class for interacting with the Student Study Planner Amendments API. It provides methods to fetch, add, update, and delete amendment records related to a student's study planner.

---
## Class Directory
```directory
src/app/class/
	├── StudentStudyPlannerAmmendments/
		├── StudentStudyPlannerAmmendmentsDB.js
```
---
## Methods

### 1. FetchAmendments

Fetches amendments from the server with optional query parameters.

```js
StudentStudyPlannerAmmendmentsDB.FetchAmendments(params)
```

#### Parameters

| Name             | Type            | Description                                                           |
| ---------------- | --------------- | --------------------------------------------------------------------- |
| id               | `number \| null`  | Unique identifier for a specific amendment. Optional.                 |
| student_id       | `string`          | Student's unique identifier. Optional.                                |
| unit_code        | `string`          | Code of the unit being amended. Optional.                             |
| new_unit_code    | `string`          | New unit code if the unit is being changed. Optional.                 |
| new_unit_type_id | `number`          | ID of the new unit type if changed. Optional.                         |
| action           | `string`          | Type of action (e.g., "add", "remove", "change"). Optional.           |
| time_of_action   | `string`          | Timestamp of when the action occurred. Optional.                      |
| year             | `number`          | Academic year of the amendment. Optional.                             |
| sem_index        | `number`          | Index of the semester in the year. Optional.                          |
| sem_type         | `string`          | Type of semester (e.g., "Long Semester", "Short Semester"). Optional. |
| order_by         | `Array<Object>` | Array of objects specifying columns and sort order. Optional.         |
| return           | `Array<string>` | Array of strings specifying which fields to return. Optional.         |
| exclude          | `Array<string>` | Array of strings specifying which fields to exclude. Optional.        |
:::note
The following query modifiers apply to all columns in the **StudentStudyPlannerAmmendments** table.  
Ensure that the names used are consistent with the database schema:

- **`.return`** → Must match the column names in the database.  
- **`.order_by`** → Must match the column names in the database.  
- **`.exclude`** → Must match the column names in the database.  
:::
#### Returns
| Type   | Description                                          |
| ------ | ---------------------------------------------------- |
| Array  | Array of `StudentStudyPlannerAmmendments` instances. |
| Object | Error object if the request fails.                   |

---

### 2. AddAmendment

Adds one or more amendments to the server.

```js
StudentStudyPlannerAmmendmentsDB.AddAmendment(amendments)
```

#### Parameters
| Name       | Type                                    | Description                          |
| ---------- | --------------------------------------- | ------------------------------------ |
| amendments | StudentStudyPlannerAmmendments \| Array | Amendment object or array of objects |
#### Returns
| Property | Type    | Description                                |
| -------- | ------- | ------------------------------------------ |
| success  | `boolean` | Indicates if the operation was successful. |
| message  | `string`  | Message describing the result.             |
| data     | `any`     | The returned data from the operation.      |
| ids      | `array`   | Array of IDs of the affected amendments.   |
| status   | `number`  | HTTP status code of the response.          |

---
### 4. DeleteAmendment

Deletes an amendment from the server by ID.

```js
StudentStudyPlannerAmmendmentsDB.DeleteAmendment(id)
```

#### Parameters
| Name | Type   | Description                        |
| ---- | ------ | ---------------------------------- |
| id   | `number` | The ID of the amendment to delete. |
#### Returns



---

## Example Usage

```js
import StudentStudyPlannerAmmendmentsDB from './StudentStudyPlannerAmmendmentsDB';

// Fetch amendments
const amendments = await StudentStudyPlannerAmmendmentsDB.FetchAmendments({ student_id: 'S12345' });

// Add an amendment
const addResult = await StudentStudyPlannerAmmendmentsDB.AddAmendment({
    student_id: 'S12345',
    unit_code: 'COMP101',
    action: 'add',
    year: 2025,
    sem_index: 1,
    sem_type: 'Long Semester'
});

// Delete an amendment
const deleteResult = await StudentStudyPlannerAmmendmentsDB.DeleteAmendment(1);
```

---

## Export

The class is exported as the default export from the module.

```js
export default StudentStudyPlannerAmmendmentsDB;
```