# Terms

import { TransformWrapper, TransformComponent } from "react-zoom-pan-pinch";

## Overview
The Terms page displays all academic terms in the system with information such as Term Name, Year, Month, Semester Type, and Status. Terms represent specific academic periods (e.g., 2025_MAR_S1) and can be filtered, searched, added, edited, deleted, and imported. For detailed CRUD instructions, see the [General](./general.md) page as the same steps apply here.

## Filtering and Search
You can filter terms using multiple options:
1. **Term Name** - Search by term name
2. **Advanced Filters** - Filter by:
   - Term Name
   - Year
   - Month
   - Status (Published, Unpublished, Unavailable)
3. **Reset Filters** - Clear all filters to see all terms

<TransformWrapper
  defaultScale={1}
  defaultPositionX={0}
  defaultPositionY={0}
  wheel={{ step: 0.1 }}
  doubleClick={{ disabled: true }}
>
  <TransformComponent>
    <img src="/img/termspage.png" alt="Term Management Page"vwidth="100%" />
  </TransformComponent>
</TransformWrapper>

---

## Advanced Filters Example

<TransformWrapper
  defaultScale={1}
  defaultPositionX={0}
  defaultPositionY={0}
  wheel={{ step: 0.1 }}
  doubleClick={{ disabled: true }}
>
  <TransformComponent>
    <img src="/img/termfilters.png" alt="Advanced Filters Dialog"vwidth="100%" />
  </TransformComponent>
</TransformWrapper>

---

## CRUD Operations

All CRUD operations (Add, Edit, Delete) follow the same steps as shown in the [General](./general.md) page. The only difference is the information fields specific to terms (Term Name, Year, Month, Semester Type, Status).

<TransformWrapper
  defaultScale={1}
  defaultPositionX={0}
  defaultPositionY={0}
  wheel={{ step: 0.1 }}
  doubleClick={{ disabled: true }}
>
  <TransformComponent>
    <img src="/img/addterm.png" alt="Add Term Form"vwidth="100%" />
  </TransformComponent>
</TransformWrapper>

---

## Import Terms

:::note
The Import function is available on the Terms page and follows the same process as shown in the [General](./general.md) page.
:::

When importing terms, ensure your CSV file includes the correct column headers and formatting:
- **name** - Term name (e.g., 2024_FEB_S1)
- **year** - Year (e.g., 2024)
- **month** - Month number (1-12) or month names (January, Feb, etc.)
- **semtype** - Semester type: "Long Semester" or "Short Semester"
- **status** - Status: "Published", "Unpublished", or "Unavailable"

<TransformWrapper
  defaultScale={1}
  defaultPositionX={0}
  defaultPositionY={0}
  wheel={{ step: 0.1 }}
  doubleClick={{ disabled: true }}
>
  <TransformComponent>
    <img src="/img/termimportrules.png" alt="Term Import Rules"vwidth="100%" />
  </TransformComponent>
</TransformWrapper>