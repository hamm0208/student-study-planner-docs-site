# General

import { TransformWrapper, TransformComponent } from "react-zoom-pan-pinch";

## Overview
This is a general user manual for the entire system, some pages have similar functions, such as Units, Terms, Courses, Students. This page has a manual for simple CRUD system, and also Import certain information. 

:::tip

- Ensure that you are in "Edit Mode" and have the required permission given
- Editting a unit might effect already completed study planners
- Remember to add any PRE-, CO-, and ANTI- requisites if the units have any
- All units must have a certain amount of Credit Point and Minimum Credit Point if needed for the unit
:::

## Add 
:::note
- Go to Unit Management's page
- Click on "Add Unit's Button"
- Fill in the necessary information, then click "Add Unit"
:::
<iframe width="100%"
height="450" 
src="https://www.youtube.com/embed/6fbtmFn9h0E?si=drx9ZM9G_7CjQtYU" 
title="YouTube video player" 
frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

---

## Edit 

:::note
- Click on the Unit you wish to edit then click the button "Edit Unit"
- Change the information of the unit you wished you change. 
- Click the button "Save Changes"
:::

<iframe width="100%" height="450" src="https://www.youtube.com/embed/K2YuI8x9ZvU?si=NJ_uxIbmceEoVcLK" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

---

## Delete 

:::caution
Deleting Unit should be done when necessary, be sure to double check the unit you are deleting. 
:::

:::note
- Similar to edit unit, click on the unit you want to delete.
- Click the "Edit Unit" button
- Click the "Delete" button
:::

<iframe width="100%" height="450" src="https://www.youtube.com/embed/7S1NQeQFvH8?si=xplA0Yg6wLyUTl2e" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

---

## Import 

:::note
- Be sure to download the csv template file if you want to import a unit. 
- Be sure the formatting of the information in the csv file is correct to ensure no issues when importing units
:::

Example of format:
<TransformWrapper
  defaultScale={1}
  defaultPositionX={0}
  defaultPositionY={0}
  wheel={{ step: 0.1 }}
  doubleClick={{ disabled: true }}
>
<TransformComponent>
    <img src="/img/format_csv.png" alt="Format CSV"vwidth="100%" />
  </TransformComponent>
</TransformWrapper>

