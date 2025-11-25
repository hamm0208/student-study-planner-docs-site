# Intake
import { TransformWrapper, TransformComponent } from "react-zoom-pan-pinch";

## Overview
Intakes represent the start dates for specific majors (e.g., February 2024 intake, July 2024 intake). Each intake needs a complete study planner (timetable) before it can be published and used. You can add, edit, and delete intakes from the Intake page, which you access by selecting a Major in the Courses page.

## Add (Create) Intake

<iframe width="100%" height="450" src="https://www.youtube.com/embed/-7BiLgANm9s?si=YJev2N-JBPx6wwu2" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>


:::caution
An intake cannot be published until it has a complete study planner (timetable) assigned to it. Make sure to add all required information before publishing.
:::
<TransformWrapper
  defaultScale={1}
  defaultPositionX={0}
  defaultPositionY={0}
  wheel={{ step: 0.1 }}
  doubleClick={{ disabled: true }}
>
<TransformComponent>
    <img src="/img/intakecondition.png" alt="Intake Condition"vwidth="100%" />
  </TransformComponent>
</TransformWrapper>

---

## Edit (Update) Intake

:::note
- Select the intake you want to edit
- Click the "Change to Edit" button
- Update the intake details as needed
- Click "Save Changes"
:::

---

## Delete (Remove) Intake

:::caution
Deleting an intake will remove it and its associated timetable. Be sure you no longer need this intake before deleting.
:::

:::note
- Select the intake you want to remove
- Click the "Change to Edit" button
- Click the "Remove" button to remove the intake from a major
:::