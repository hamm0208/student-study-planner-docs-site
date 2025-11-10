# Course
The `Course` class represents an academic course entity, typically used to manage and track the details of a course within the study planner system.

---
## Class Directory
```directory
src/app/class/
    ├── Course/
        ├── Course.js
```
---
## Constructor

```js
new Course({ id = null, code, name, credits_required, status })
```


### Parameters

|Name|Type|Description|
|---|---|---|
|**id**|`number`|The unique identifier for the course (optional, defaults to null).|
|**code**|`string`|The code identifying the course.|
|**name**|`string`|The name of the course.|
|**credits_required**|`number`|The number of credits required to complete the course.|
|**status**|`string`|The current status of the course (e.g., "Active", "Inactive").|

### Properties

|Name|Type|Description|
|---|---|---|
|**id**|`number`|Gets or sets the course’s unique identifier.|
|**code**|`string`|Gets or sets the course’s code.|
|**name**|`string`|Gets or sets the course’s name.|
|**credits_required**|`number`|Gets or sets the required credits.|
|**status**|`string`|Gets or sets the course’s status.|

---

## Getters and Setters

- `id`: Get or set the course's ID.
- `code:` Get or set the course's code.
- `name`: Get or set the course's name.
- `credits_required:` Get or set the required credits for the course.
- `status:` Get or set the course's status.

---

## Example Usage
```js
import Course from './Course';

const course = new Course({
    id: 1,
    code: 'CS101',
    name: 'Introduction to Computer Science',
    credits_required: 120,
    status: 'Active'
});

console.log(course.id); // 1
console.log(course.code); // 'CS101'
console.log(course.name); // 'Introduction to Computer Science'
console.log(course.credits_required); // 120
console.log(course.status); // 'Active'
```
---

## Export
The class is exported as the default export from the module.
```js
export default Course;
```