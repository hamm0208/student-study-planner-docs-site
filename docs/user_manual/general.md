# General

import { TransformWrapper, TransformComponent } from "react-zoom-pan-pinch";

## Overview
This guide covers basic operations used across multiple pages in the system: Units, Terms, Courses, and Students. Each of these pages supports CRUD operations (Create, Read, Update, Delete), and some also support importing data. The examples below use Units, but the same steps apply to other pages. 

:::tip
## Unit
- Remember to add any PRE-, CO-, and ANTI- requisites if the units have any
- All units must have a certain amount of Credit Point and Minimum Credit Point if needed for the unit
:::

## Add (Create) 
<iframe width="100%"
height="450" 
src="https://www.youtube.com/embed/6fbtmFn9h0E?si=drx9ZM9G_7CjQtYU" 
title="YouTube video player" 
frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

---

## Edit (Update) 
<iframe width="100%" height="450" src="https://www.youtube.com/embed/K2YuI8x9ZvU?si=NJ_uxIbmceEoVcLK" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

---

## Delete (Remove) 

:::caution
Deleting certain information is dangerous as it might cause issues to certain completed study planners, be sure to double check information you are deleting. 
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
- Import function is  only available in Units and Terms page
- Be sure to download the csv template file if you want to import a unit or term.
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

<iframe width="100%" height="450" src="https://www.youtube.com/embed/hDfQjBNRpF4?si=1V-Py38KJI5hew35" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>