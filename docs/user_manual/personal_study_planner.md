# Personal Study Planner

import { TransformWrapper, TransformComponent } from "react-zoom-pan-pinch";

:::tip

- For the images, Use scrollwheel to zoom in, and left click to move around
- Ensure that you are in "Edit Mode" and have the required permission given
- The Personal Study Planner builds on the Master Study Planner, thus have the same basic features
	- Refer to Master [Study Planner manual](master_study_planner) for the basic features
:::

:::warning
To prevent data loss, ensure that you click:

"Save Amendments" for saving the amendments for the student


:::

## Student Information
Within the student information tab, it contains these details:

| Section          | Description                                                             |
|-----------------|-------------------------------------------------------------------------|
| Student Profile     | Contain student's `Mame`, `Student ID`, `Email` |
| Academic Progress     | Tracks student's progress (Total Credit Points Earned / Required Credit Points of the course ) |
| Unit Progress     | Shows the student's progress via Unit Types |

<TransformWrapper
  defaultScale={1}
  defaultPositionX={0}
  defaultPositionY={0}
  wheel={{ step: 0.1 }}
  doubleClick={{ disabled: true }}
>
  <TransformComponent>
    <img src="/img/psp_student_info.png" alt="Add Year"vwidth="100%" />
  </TransformComponent>
</TransformWrapper>

---

## Send Study Planner
Within the planner information tab, it contains these details:

1. **Receipients**
  - By default, the receipients is only the student
    - To add more receipients just add in a comma "," or a new line

The sample below will send the email to these 3 receipients
```email
123456@students.swinburne.edu.my, JohnDoe@gmail.com
AnotherUser@gmail.com
```

2. **Subject**
- Default subject for the email is `Study Planner for {Student Name} ({Student ID})`, however feel free to change it

3. **Email Content**
- There is a default content for this section, feel free to modify it


<TransformWrapper
  defaultScale={1}
  defaultPositionX={0}
  defaultPositionY={0}
  wheel={{ step: 0.1 }}
  doubleClick={{ disabled: true }}
>
  <TransformComponent>
    <img src="/img/psp_send_email.png" alt="Add Year"vwidth="100%" />
  </TransformComponent>
</TransformWrapper>

---
## Conflicts
The conflicts tab will list all the conflicts within the student's planner with its issue

<TransformWrapper
  defaultScale={1}
  defaultPositionX={0}
  defaultPositionY={0}
  wheel={{ step: 0.1 }}
  doubleClick={{ disabled: true }}
>
  <TransformComponent>
    <img src="/img/psp_conflicts.png" alt="Add Year"vwidth="100%" />
  </TransformComponent>
</TransformWrapper>

---

## Current Amendments
This tab will show all the current amendments that have been done within this session, there are **4** types of possible actions:
1. `Change Unit Type`
	- **Changing** a unit type in the planner
2. `Deleted Unit`
	- **Deleting** a Unit
	- **Deleting** a Semester (Will delete all the units within that semester)
	- **Deleting** a Year (Will delete all the units within that year)
3. `Replaced Unit`
	- **Replacing** a unit with another unit thats **NOT** in the current planner
4. `Swapped Unit`
	- **Swapping** 2 units, whereby both of the units ARE in the current planner

<TransformWrapper
  defaultScale={1}
  defaultPositionX={0}
  defaultPositionY={0}
  wheel={{ step: 0.1 }}
  doubleClick={{ disabled: true }}
>
  <TransformComponent>
    <img src="/img/psp_current_amendments.png" alt="Add Year"vwidth="100%" />
  </TransformComponent>
</TransformWrapper>

---
## Amendment History
Shows all the previous amendments done onto the current planner
<TransformWrapper
  defaultScale={1}
  defaultPositionX={0}
  defaultPositionY={0}
  wheel={{ step: 0.1 }}
  doubleClick={{ disabled: true }}
>
  <TransformComponent>
    <img src="/img/psp_amendment_history.png" alt="Add Year"vwidth="100%" />
  </TransformComponent>
</TransformWrapper>


---
## Solve Conflict
The Auto Solver only works IF:
1. The conflicts **DOES NOT** have any status issue (Unpublished unit)

The Auto Solver will solve units by swapping the conflicitng units around to find the solution:
<iframe
  width="100%"
  height="450"
  src="https://www.youtube.com/embed/JO1xjnB-NzQ"
  title="YouTube video player"
  frameBorder="0"
  allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
  allowFullScreen
/>

