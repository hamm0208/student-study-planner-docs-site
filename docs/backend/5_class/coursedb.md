# CourseDB

The `CourseDB` class provides static methods for interacting with the Course API. It provides methods to fetch, save, and delete course records from the backend.

---

## Class Directory

```directory
src/app/class/
    ├── Course/
        ├── CourseDB.js
```

---

## Methods

### `static async FetchCourses(params)`

Fetches a list of courses from the server, with optional query parameters for filtering, ordering, and field selection.

#### Parameters

| Name | Type | Description |
|------|------|--------------|
| **params** | `object` | Query parameters for filtering, ordering, and selecting fields. |

#### Returns

An object containing:

- `success`: `boolean` – indicates whether the operation was successful.  
- `data`: `Course[]` – array of `Course` instances.  
- `message`: `string` – error message, if any.  
- `filtered`: `boolean` – indicates if the data was filtered.  

---

### `static async SaveCourse(courseData, method_type)`

Saves a course to the server (create or update).

#### Parameters

| Name | Type | Description |
|------|------|--------------|
| **courseData** | `object` | The course data to save. |
| **method_type** | `string` | HTTP method (`'POST'` for create, `'PUT'` for update). |

#### Returns

- The server response object, including error info if applicable.

---

### `static async deleteCourse(courseCode)`

Deletes a course from the server by its code.

#### Parameters

| Name | Type | Description |
|------|------|--------------|
| **courseCode** | `string` | The code of the course to delete. |

#### Returns

- The server response object, or throws an error on failure.

---

## Example Usage

```js
import CourseDB from '@/app/class/Course/CourseDB.js';

// Fetch all courses
const result = await CourseDB.FetchCourses();

// Save a new course
await CourseDB.SaveCourse({ code: 'COS10001', name: 'Introduction to Programming' }, 'POST');

// Delete a course
await CourseDB.deleteCourse('COS10001');
```

---

## Export

The class is exported as the default export from the module.
