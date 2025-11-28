# Course

import { TransformWrapper, TransformComponent } from "react-zoom-pan-pinch";

## Overview
The Course page displays all courses in the system along with their associated majors. Currently, there is only one course—Bachelor of Computer Science (BA-CS)—which contains multiple majors such as Cybersecurity, Data Science, Artificial Intelligence, Internet of Things, and Software Development. Each course displays the number of majors, credits required, and allows you to add, edit, and delete courses. For detailed CRUD instructions, see the [General](./general.md) page as the same steps apply here.

:::note
**Current Structure**: The system currently has one course (BA-CS) with 5 majors. Each major represents a specialization within this course and can have its own intake schedule and timetable.
:::

## Course Structure
When you expand a course row, you can see all the majors associated with it. Each major has its own set of CRUD actions and can be managed individually.

## Filtering and Search
You can filter courses using:
1. **Course Code** - Search by course code (e.g., BA-CS)
2. **Reset Filters** - Clear filters to see all courses

<TransformWrapper
  defaultScale={1}
  defaultPositionX={0}
  defaultPositionY={0}
  wheel={{ step: 0.1 }}
  doubleClick={{ disabled: true }}
>
  <TransformComponent>
    <img src="/img/Coursepics.png" alt="Course Management Page"vwidth="100%" />
  </TransformComponent>
</TransformWrapper>

---

## CRUD Operations

All CRUD operations (Add, Edit, Delete) follow the same steps as shown in the [General](./general.md) page. The only difference is the information fields specific to courses (Code, Name, Credits Required).

### Add Course

<TransformWrapper
  defaultScale={1}
  defaultPositionX={0}
  defaultPositionY={0}
  wheel={{ step: 0.1 }}
  doubleClick={{ disabled: true }}
>
  <TransformComponent>
    <img src="/img/addcourse.png" alt="Add Course Dialog"vwidth="100%" />
  </TransformComponent>
</TransformWrapper>

---

## Managing Majors in a Course

To add, edit, or delete majors for a course, you need to edit the course first. Click the "Edit" button on the course row to open the course editor. In the editor, you'll see a "Majors" section where you can:

1. **Add Major** - Enter a major name in the text field and click the "Add" button to add it to the course
2. **Remove Major** - Click the "X" button next to a major to remove it from the course

Each major added to a course can then be assigned specific intakes (start dates) and study planner timetables through the Intake page.

<TransformWrapper
  defaultScale={1}
  defaultPositionX={0}
  defaultPositionY={0}
  wheel={{ step: 0.1 }}
  doubleClick={{ disabled: true }}
>
  <TransformComponent>
    <img src="/img/addmajor.png" alt="Edit Course with Majors Management"vwidth="100%" />
  </TransformComponent>
</TransformWrapper>

:::note
When you add majors to a course through the Edit Course dialog, you're associating these specializations with the course. Each major can then have multiple intakes with different start dates and their own study planner timetables.
:::

