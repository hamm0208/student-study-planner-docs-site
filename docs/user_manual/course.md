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

:::note
When you add or edit a course, you're managing the course itself. Individual majors within a course are managed separately through the course's detail page by expanding the course row.
:::

