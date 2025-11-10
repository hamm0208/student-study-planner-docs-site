# CourseIntake 

The `CourseIntake` class represents an academic course intake entity, typically used to manage and track the status and associations of a course intake within the study planner system.

---

## Class Directory

```directory
src/app/class/
    ├── CourseIntake/
        ├── CourseIntake.js
```

---

## Constructor

```js
new CourseIntake({ ID, Status, TermID, MajorID })
```

### Parameters

| Name        | Type     | Description                                  |
| ----------- | -------- | -------------------------------------------- |
| **ID**      | `number` | The unique identifier for the course intake. |
| **Status**  | `string` | The current status of the course intake.     |
| **TermID**  | `number` | The identifier for the associated term.      |
| **MajorID** | `number` | The identifier for the associated major.     |

### Properties

| Name        | Type     | Description                                  |
| ----------- | -------- | -------------------------------------------- |
| **ID**      | `number` | Gets or sets the intake's unique identifier. |
| **Status**  | `string` | Gets or sets the intake's status.            |
| **TermID**  | `number` | Gets or sets the associated term ID.         |
| **MajorID** | `number` | Gets or sets the associated major ID.        |

---

## Getters and Setters

- `ID`: Get or set the intake's ID.
- `Status`: Get or set the intake's status.
- `TermID`: Get or set the associated term ID.
- `MajorID`: Get or set the associated major ID.

---

## Example Usage

```js
import CourseIntake from './CourseIntake';

const intake = new CourseIntake({
    ID: 1,
    Status: 'Open',
    TermID: 2025,
    MajorID: 10
});

console.log(intake.ID); // 1
console.log(intake.Status); // 'Open'
console.log(intake.TermID); // 2025
console.log(intake.MajorID); // 10
```

---

## Export

The class is exported as the default export from the module.
```js
export default CourseIntake;
```