# Master Study Planner

import { TransformWrapper, TransformComponent } from "react-zoom-pan-pinch";

:::tip

- For the images, Use scrollwheel to zoom in, and left click to move around
- Ensure that you are in "Edit Mode" and have the required permission given
- Adding/Removing a year/semester will automatically adjust the timeline of each of the semester to the next nearest semester depending on semester type

:::

:::warning
To prevent data loss, ensure that you click:

"Save Changes" for saving the draft

OR

"Save Planner" for saving a completed planner


:::

:::note

Rule:
- In each of the Year within a planner, must **NOT** have more than 2 long semester **AND** 2 short semester
:::

## Planner Information
Within the planner information tab, it contains these details:

| Section          | Description                                                             |
|-----------------|-------------------------------------------------------------------------|
| Last Modified     | Last Modified by who |
| Course Details     | The course information (`Status`, `Credits`, `Total Years`, `Total Semesters`) |
| Unit Types       | All the unit type and their credit points.       |
<TransformWrapper
  defaultScale={1}
  defaultPositionX={0}
  defaultPositionY={0}
  wheel={{ step: 0.1 }}
  doubleClick={{ disabled: true }}
>
  <TransformComponent>
    <img src="/img/msp_planner_info.png" alt="Add Year"vwidth="100%" />
  </TransformComponent>
</TransformWrapper>

---

## Year

### Add New Year
Click on "Add Year"

<TransformWrapper
  defaultScale={1}
  defaultPositionX={0}
  defaultPositionY={0}
  wheel={{ step: 0.1 }}
  doubleClick={{ disabled: true }}
>
  <TransformComponent>
    <img src="/img/msp_add_year.png" alt="Add Year"vwidth="100%" />
  </TransformComponent>
</TransformWrapper>

### Remove Year
Click on "Remove Year"

<TransformWrapper
  defaultScale={1}
  defaultPositionX={0}
  defaultPositionY={0}
  wheel={{ step: 0.1 }}
  doubleClick={{ disabled: true }}
>
  <TransformComponent>
    <img src="/img/msp_remove_year.png" alt="Add Year"vwidth="100%" />
  </TransformComponent>
</TransformWrapper>

---

## Semester

### Add Semester
There are 2 types of semester to choose from:
1. Long Semester (Default to Feb/Mar & Aug/Sep)
2. Short Semester (Default to Jan & Jul)

<TransformWrapper
  defaultScale={1}
  defaultPositionX={0}
  defaultPositionY={0}
  wheel={{ step: 0.1 }}
  doubleClick={{ disabled: true }}
>
  <TransformComponent>
    <img src="/img/msp_semester.png" alt="Add Year"vwidth="100%" />
  </TransformComponent>
</TransformWrapper>

### Remove Semester

<TransformWrapper
  defaultScale={1}
  defaultPositionX={0}
  defaultPositionY={0}
  wheel={{ step: 0.1 }}
  doubleClick={{ disabled: true }}
>
  <TransformComponent>
    <img src="/img/msp_remove_sem.png" alt="Add Year"vwidth="100%" />
  </TransformComponent>
</TransformWrapper>

---

## Unit
### Unit Row Actions
| Action          | Description                                                             |
|-----------------|-------------------------------------------------------------------------|
| Add Unit     | To add a new unit row onto the semester. |
| Select Unit     | To select a unit or click on the desired row's `Unit` column. |
| Clear Row       | Remove all data in the selected row.       |
| Delete Row      | Delete the entire row from the planner.   |

<TransformWrapper
  defaultScale={1}
  defaultPositionX={0}
  defaultPositionY={0}
  wheel={{ step: 0.1 }}
  doubleClick={{ disabled: true }}
>
  <TransformComponent>
    <img src="/img/msp_units.png" alt="Add Year"vwidth="100%" />
  </TransformComponent>
</TransformWrapper>

### Change Unit Type
To change a unit type, click on the round circle (Highlighted in the red square), and click on the desired unit type
<TransformWrapper
  defaultScale={1}
  defaultPositionX={0}
  defaultPositionY={0}
  wheel={{ step: 0.1 }}
  doubleClick={{ disabled: true }}
>
  <TransformComponent>
    <img src="/img/msp_unit_type.png" alt="Add Year"vwidth="100%" />
  </TransformComponent>
</TransformWrapper>

After clicking on the desired Unit Type, the unit row's colour will change
<TransformWrapper
  defaultScale={1}
  defaultPositionX={0}
  defaultPositionY={0}
  wheel={{ step: 0.1 }}
  doubleClick={{ disabled: true }}
>
  <TransformComponent>
    <img src="/img/msp_unit_type_selected.png" alt="Add Year"vwidth="100%" />
  </TransformComponent>
</TransformWrapper>

---

## Import from Planners
Importing other planners data onto the current planner
:::note

Import Planner is only available when the following conditions are met:

1. The target semester belongs to the same course.

2. The semester has the same starting type.

- Example:

  - 2024_FEB_S1 → Starts with a Long Semester

  - 2024_JAN_ST → Starts with a Short Semester

- In this case, the Short Semester option will not appear when importing into a Long Semester, and vice versa.
:::

<iframe
  width="100%"
  height="450"
  src="https://www.youtube.com/embed/uhUpL2iAKUg"
  title="YouTube video player"
  frameBorder="0"
  allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
  allowFullScreen
/>

---

## Conflict Resolution
:::note

1. Next Conflict Button
  - Lead the user to the next conflicting unit in the planner
:::
<iframe
  width="100%"
  height="450"
  src="https://www.youtube.com/embed/rbyNTug3bCM"
  title="YouTube video player"
  frameBorder="0"
  allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
  allowFullScreen
/>

---

## Save as PDF
Click on the `Save as PDF` button, to export a PDF file

Example of the PDF:
<TransformWrapper
  defaultScale={1}
  defaultPositionX={0}
  defaultPositionY={0}
  wheel={{ step: 0.1 }}
  doubleClick={{ disabled: true }}
>
  <TransformComponent>
    <img src="/img/msp_export_pdf.png" alt="Add Year"vwidth="100%" />
  </TransformComponent>
</TransformWrapper>

