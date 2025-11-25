# Student

import { TransformWrapper, TransformComponent } from "react-zoom-pan-pinch";

## Overview
The Student page displays all students in the system with their information such as Student ID, Name, Course, Major, and Term Intake. You can filter, search, add, edit, and delete students from this page. For detailed CRUD instructions, see the [General](./general.md) page as the same steps apply here.

## Filtering and Search
You can filter students using multiple options:
1. **Student ID** - Search by student ID number
2. **Student Name** - Search by student name
3. **Course** - Filter by course
4. **Major** - Filter by major
5. **Reset Filters** - Clear all filters to see all students

<TransformWrapper
  defaultScale={1}
  defaultPositionX={0}
  defaultPositionY={0}
  wheel={{ step: 0.1 }}
  doubleClick={{ disabled: true }}
>
  <TransformComponent>
    <img src="/img/student_page_add.png" alt="Student Information Page"vwidth="100%" />
  </TransformComponent>
</TransformWrapper>

---

## CRUD Operations

All CRUD operations (Add, Edit, Delete) follow the same steps as shown in the [General](./general.md) page. The only difference is the information fields specific to students (Student ID, Name, Course, Major, Term Intake, Status).

## Student ID Search (Student Personal Study Planner)
By entering the student's id on the search bar, you are able to access any student's personal study planner with ease. Just enter student id in the box and press the "Search Student" button

<TransformWrapper
  defaultScale={1}
  defaultPositionX={0}
  defaultPositionY={0}
  wheel={{ step: 0.1 }}
  doubleClick={{ disabled: true }}
>
  <TransformComponent>
    <img src="/img/searchstudent.png" alt="Student Information Page"vwidth="100%" />
  </TransformComponent>
</TransformWrapper>