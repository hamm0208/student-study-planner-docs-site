# MasterStudyPlannerDB
`MasterStudyPlannerDB` is a static utility class for interacting with the Master Study Planner API. It provides methods to fetch, add, update, and delete master study planner records from the backend.

---
## Class Directory
```directory
src/app/class/
	├── MasterStudyPlanner/
		├── MasterStudyPlannerDB.js
```

---
## Methods
### FetchMasterStudyPlanners

Fetches master study planners from the server with optional query parameters.

```js
MasterStudyPlannerDB.FetchMasterStudyPlanners(params)
```
#### Parameters
| Name   | Type   | Optional          | Description                                                            | Example |
| ------ | ------ | ----------------- | ---------------------------------------------------------------------- | ------- |
| params | Object | Yes (Returns all) | Query parameters for filtering, sorting, selecting and excluding data. | Below:  |
#### Params:
| Name                 | Optional | Type                   | Description                                                      | Example                                                   |
| -------------------- | -------- | ---------------------- | ---------------------------------------------------------------- | --------------------------------------------------------- |
| **id**               | Yes      | `number` \| `null`     | Unique identifier for a specific master study planner. Optional. | `123`                                                     |
| **course_intake_id** | Yes      | `number` \| `number[]` | One or multiple course intake IDs to filter by.                  | `1` or `[3, 4]`                                           |
| **status**           | Yes      | `string`               | Status of the master study planner.                              | `"complete"`                                              |
| **return**           | Yes      | `string[]`             | List of attributes to include in the response.                   | `["ID", "Status"]`                                        |
| **order_by**         | Yes      | `Array<Object>`        | Sorting rules (column + order).                                  | `[ { "column": "ID", "ascending": true } ]`               |
| **exclude**          | Yes      | `Object`               | Exclude certain values from results (per field).                 | `{ ID: [1,2], CourseIntakeID: [1,2], Status: ["Empty"] }` |

```JS
{
  id: 123,
  course_intake_id: 1 OR [3, 4],
  status: "complete",
  return: ["ID", "Status"],
  order_by: [
	  { column: "ID", ascending: true }
  ],
  exclude: { 
    ID: [1, 2],
    CourseIntakeID: [1, 2]
    Status: ["Empty"] 
  }
}
```
:::note
The following query modifiers apply to all columns in the **MasterStudyPlanner** table.  
Ensure that the names used are consistent with the database schema:

- **`.return`** → Must match the column names in the database.  
- **`.order_by`** → Must match the column names in the database.  
- **`.exclude`** → Must match the column names in the database.  
:::
#### Returns
- Array of `MasterStudyPlanner` instances.

---

### AddMasterStudyPlanner

Adds a new master study planner to the server.

```js
MasterStudyPlannerDB.AddMasterStudyPlanner(planner)
```

#### Parameters
| Name    | Type               | Description                     | Optional | Example                                     |
| ------- | ------------------ | ------------------------------- | -------- | ------------------------------------------- |
| planner | MasterStudyPlanner | The planner object to be added. | No       | `{course_intake_id: 101, status: 'Empty' }` |

#### Returns
| Field     | Type    | Description                                                                                                                        | Example Value                                                                   |
| --------- | ------- | ---------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------- |
| `success` | `boolean` | Indicates whether the request was successful or not.                                                                               | `true` / `false`                                                                |
| `message` | `string`  | Human-readable message explaining the result.                                                                                      | `"Master study planners added successfully"` / `"Course Intake does not exist"` |
| `data`    | `array`   | Array of newly created master study planners (only present on success). Each object contains `ID`, `CourseIntakeID`, and `Status`. | `[{"ID": 1, "CourseIntakeID": 3, "Status": "Complete"}]`                        |
| `ids`     | `array`   | Array of IDs of the newly created planners (only present on success).                                                              | `[1, 2, 3]`                                                                     |
| `status`  | `number`  | HTTP status code for the response                                                                                                  | `400` / `500`                                                                   |

---

### UpdateMasterStudyPlanner

Updates an existing master study planner on the server.

```js
MasterStudyPlannerDB.UpdateMasterStudyPlanner(planner)
```

#### Parameters
| Name    | Type               | Description                                                                                           | Optional | Example                         |
| ------- | ------------------ | ----------------------------------------------------------------------------------------------------- | -------- | ------------------------------- |
| planner | MasterStudyPlanner | The planner object to be updated. Only the status can be updated ('`Complete'`, `'Empty'`, `'Draft'`) | No       | `{ id: 1, status: 'Complete' }` |
#### Returns
| Field     | Type    | Description                                                                                                         | Example Value                                                                  |
| --------- | ------- | ------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------ |
| `success` | boolean | Indicates whether the request was successful.                                                                       | `true` / `false`                                                               |
| `message` | string  | Human-readable message explaining the result.                                                                       | `"Master study planner added successfully"` / `"Course Intake does not exist"` |
| `planner` | object  | The master study planner that was created (only present on success). Contains `ID`, `CourseIntakeID`, and `Status`. | `{"ID": 1, "CourseIntakeID": 3, "Status": "Complete"}`                         |

---
## Example Usage

```js
import MasterStudyPlannerDB from './MasterStudyPlannerDB';

// Fetch planners
const planners = await MasterStudyPlannerDB.FetchMasterStudyPlanners({ order_by: [{ column: 'ID', ascending: true }] });

// Add a planner
const addResult = await MasterStudyPlannerDB.AddMasterStudyPlanner({ course_intake_id: 101, status: 'Complete' });

// Update a planner
const updateResult = await MasterStudyPlannerDB.UpdateMasterStudyPlanner({ id: 1, status: 'Complete' });
```

---

:::note
- All methods are asynchronous and return Promises.
- Error handling is performed internally and errors are logged to the console.
- The API endpoint is determined by the `NEXT_PUBLIC_SERVER_URL` environment variable.
:::

```js

export default MasterStudyPlannerDB;

```