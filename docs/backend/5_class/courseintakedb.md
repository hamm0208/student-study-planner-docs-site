# CourseIntakeDB
The `CourseIntakeDB` class provides static methods for interacting with the `Course Intake` database via API calls. It handles fetching, adding, updating, deleting, and batch updating `Course Intake` records in the study planner system.

---
## Class Directory
```directory
src/app/class/
    ├── CourseIntake/
        ├── CourseIntakeDB.js
```

---

## Methods

### FetchCourseIntakes

Fetches course intakes from the server with optional query parameters.

```js
CourseIntakeDB.FetchCourseIntakes(params)
```

#### Parameters
| Name   | Type   | Optional          | Description                                                            | Example |
| ------ | ------ | ----------------- | ---------------------------------------------------------------------- | ------- |
| params | Object | Yes (Returns all) | Query parameters for filtering, sorting, selecting and excluding data. | Below:  |

#### Params:
| Name      | Optional | Type                | Description                                | Example                                     |
| --------- | -------- | ------------------- | ------------------------------------------ | ------------------------------------------- |
| **ID**    | Yes      | `number` \| `null`  | Unique identifier for a course intake.     | `123`                                       |
| **Status** | Yes      | `string`            | Status of the course intake.               | `"Open"`                                    |
| **TermID** | Yes      | `number`            | Term identifier to filter by.              | `2025`                                      |
| **MajorID**| Yes      | `number`            | Major identifier to filter by.             | `10`                                        |
| **return** | Yes      | `string[]`          | List of attributes to include in response. | `["ID", "Status"]`                          |
| **order_by**| Yes     | `Array<Object>`     | Sorting rules (column + order).           | `[{ "column": "ID", "ascending": true }]`   |
| **exclude**| Yes      | `Object`            | Exclude certain values from results.       | `{ Status: ["Closed"], MajorID: [1, 2] }`   |

```js
{
  ID: 123,
  Status: "Open",
  TermID: 2025,
  MajorID: 10,
  return: ["ID", "Status"],
  order_by: [
    { column: "ID", ascending: true }
  ],
  exclude: {
    Status: ["Closed"],
    MajorID: [1, 2]
  }
}
```

:::note
The following query modifiers apply to all columns in the **CourseIntake** table.  
Ensure that the names used are consistent with the database schema:

- **`.return`** → Must match the column names in the database.  
- **`.order_by`** → Must match the column names in the database.  
- **`.exclude`** → Must match the column names in the database.  
:::

#### Returns
| Field     | Type      | Description                                                | Example                                    |
| --------- | --------- | ---------------------------------------------------------- | ------------------------------------------ |
| `success` | `boolean` | Indicates whether the request was successful.              | `true` / `false`                           |
| `data`    | `array`   | Array of Course Intake instances (only present on success) | `[{ID: 1, Status: "Open", TermID: 2025}]`  |
| `message` | `string`  | Error message (only present on failure)                    | `"Failed to fetch course intakes"`         |

---

### AddCourseIntake

Adds a new course intake to the server.

```js
CourseIntakeDB.AddCourseIntake(intake)
```

#### Parameters
| Name    | Type         | Description                  | Optional | Example                                        |
| ------- | ------------ | ---------------------------- | -------- | ---------------------------------------------- |
| intake  | CourseIntake | The intake object to be added| No       | `{Status: "Open", TermID: 2025, MajorID: 10}` |

#### Returns
| Field     | Type      | Description                                            | Example Value                                    |
| --------- | --------- | ------------------------------------------------------ | ------------------------------------------------ |
| `success` | `boolean` | Indicates whether the request was successful           | `true` / `false`                                 |
| `message` | `string`  | Human-readable message explaining the result           | `"Course intake added successfully"`              |
| `intake`  | `object`  | The newly created course intake (only on success)     | `{ID: 1, Status: "Open", TermID: 2025}`         |

---

### UpdateCourseIntake

Updates an existing course intake on the server.

```js
CourseIntakeDB.UpdateCourseIntake(intake)
```

#### Parameters
| Name    | Type         | Description                                      | Optional | Example                                               |
| ------- | ------------ | ------------------------------------------------ | -------- | ----------------------------------------------------- |
| intake  | CourseIntake | The intake object to be updated with new values | No       | `{ID: 1, Status: "Closed", TermID: 2025, MajorID: 10}` |

#### Returns
| Field     | Type      | Description                                        | Example Value                                    |
| --------- | --------- | -------------------------------------------------- | ------------------------------------------------ |
| `success` | `boolean` | Indicates whether the request was successful       | `true` / `false`                                 |
| `message` | `string`  | Human-readable message explaining the result       | `"Course intake updated successfully"`            |
| `intake`  | `object`  | The updated course intake (only on success)       | `{ID: 1, Status: "Closed", TermID: 2025}`       |

---

### DeleteCourseIntake

Deletes one or more course intakes from the server by their IDs.

```js
CourseIntakeDB.DeleteCourseIntake(ids)
```

#### Parameters
| Name | Type       | Description                    | Optional | Example        |
| ---- | ---------- | ------------------------------ | -------- | -------------- |
| ids  | `number[]` | Array of intake IDs to delete | No       | `[1, 2, 3]`    |

#### Returns
| Field     | Type      | Description                                  | Example Value                           |
| --------- | --------- | -------------------------------------------- | --------------------------------------- |
| `success` | `boolean` | Indicates whether the request was successful | `true` / `false`                        |
| `message` | `string`  | Human-readable message explaining the result | `"Course intakes deleted successfully"` |

---

### BatchUpdateIntakes

Updates multiple course intakes in a single transaction.

```js
CourseIntakeDB.BatchUpdateIntakes(intakes)
```

#### Parameters
| Name     | Type          | Description                            | Optional | Example                                                          |
| -------- | ------------- | -------------------------------------- | -------- | ---------------------------------------------------------------- |
| intakes  | `CourseIntake[]` | Array of intake objects to update  | No       | `[{ID: 1, Status: "Closed"}, {ID: 2, Status: "Open"}]`          |

#### Returns
| Field     | Type      | Description                                  | Example Value                            |
| --------- | --------- | -------------------------------------------- | ---------------------------------------- |
| `success` | `boolean` | Indicates whether the request was successful | `true` / `false`                         |
| `message` | `string`  | Human-readable message explaining the result | `"Batch update completed successfully"`  |

---

## Example Usage

```js
import CourseIntakeDB from './CourseIntakeDB';

// Fetch course intakes with filtering and sorting
const intakes = await CourseIntakeDB.FetchCourseIntakes({
    Status: 'Open',
    order_by: [{ column: 'TermID', ascending: true }],
    exclude: { MajorID: [1, 2] }
});

// Add a new course intake
const addResult = await CourseIntakeDB.AddCourseIntake({
    Status: 'Open',
    TermID: 2025,
    MajorID: 10
});

// Update an existing course intake
const updateResult = await CourseIntakeDB.UpdateCourseIntake({
    ID: 1,
    Status: 'Closed',
    TermID: 2025,
    MajorID: 10
});

// Delete multiple course intakes
const deleteResult = await CourseIntakeDB.DeleteCourseIntake([1, 2, 3]);

// Batch update multiple intakes
const batchResult = await CourseIntakeDB.BatchUpdateIntakes([
    { ID: 1, Status: 'Closed', TermID: 2025, MajorID: 10 },
    { ID: 2, Status: 'Open', TermID: 2026, MajorID: 11 }
]);
```

---

:::note
- All methods are asynchronous and return Promises
- Error handling is performed internally and errors are logged to the console
- The API endpoint is determined by the `NEXT_PUBLIC_SERVER_URL` environment variable
- All database operations are atomic and will roll back on failure
:::

```js
export default CourseIntakeDB;
```