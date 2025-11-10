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

### [static async FetchCourses(params)](vscode-file://vscode-app/c:/Users/Daniel%20Khaleed/AppData/Local/Programs/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-browser/workbench/workbench.html)

Fetches a list of courses from the server, with optional query parameters for filtering, ordering, and field selection.

#### Parameters

|Name|Type|Description|
|---|---|---|
|**params**|`object`|Query parameters for filtering, ordering, and selecting fields.|

#### Returns

- An object containing:
    - [success](vscode-file://vscode-app/c:/Users/Daniel%20Khaleed/AppData/Local/Programs/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-browser/workbench/workbench.html): `boolean`
    - [data](vscode-file://vscode-app/c:/Users/Daniel%20Khaleed/AppData/Local/Programs/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-browser/workbench/workbench.html): [Course[]](vscode-file://vscode-app/c:/Users/Daniel%20Khaleed/AppData/Local/Programs/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-browser/workbench/workbench.html) (array of [Course](vscode-file://vscode-app/c:/Users/Daniel%20Khaleed/AppData/Local/Programs/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-browser/workbench/workbench.html) instances)
    - [message](vscode-file://vscode-app/c:/Users/Daniel%20Khaleed/AppData/Local/Programs/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-browser/workbench/workbench.html): `string` (on error)
    - [filtered](vscode-file://vscode-app/c:/Users/Daniel%20Khaleed/AppData/Local/Programs/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-browser/workbench/workbench.html): `boolean` (on error)

---

### [static async SaveCourse(courseData, method_type)](vscode-file://vscode-app/c:/Users/Daniel%20Khaleed/AppData/Local/Programs/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-browser/workbench/workbench.html)

Saves a course to the server (create or update).

#### Parameters

|Name|Type|Description|
|---|---|---|
|**courseData**|`object`|The course data to save.|
|**method_type**|`string`|HTTP method (`'POST'` for create, `'PUT'` for update).|

#### Returns

- The server response object, including error info if applicable.

---

### [static async deleteCourse(courseCode)](vscode-file://vscode-app/c:/Users/Daniel%20Khaleed/AppData/Local/Programs/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-browser/workbench/workbench.html)

Deletes a course from the server by its code.

#### Parameters

|Name|Type|Description|
|---|---|---|
|**courseCode**|`string`|The code of the course to delete.|

#### Returns

- The server response object, or throws an error on failure.

---

## Example Usage

- 
- 
- 
- 

---

## Export

The class is exported as the default export from the module.