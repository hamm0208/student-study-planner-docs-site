---
sidebar_position: 6
---

# Roles

import { TransformWrapper, TransformComponent } from "react-zoom-pan-pinch";

## Overview
The Roles page is where you manage user roles and assign permissions within the system. Each role controls which pages and functions users can access. For example, some roles can add or edit units, while others may only have read-only access to certain pages. Roles are essential for controlling user access and maintaining system security. Active roles have higher privileges and are typically assigned to administrators.

## Role Management Page

The Roles page displays all available roles in the system. From here, you can view, create, edit, or delete roles as needed.

<TransformWrapper
  defaultScale={1}
  defaultPositionX={0}
  defaultPositionY={0}
  wheel={{ step: 0.1 }}
  doubleClick={{ disabled: true }}
>
  <TransformComponent>
    <img src="/img/rolemanagementadd.png" alt="Role Management Page"vwidth="100%" />
  </TransformComponent>
</TransformWrapper>

---

## Create Role

To create a new role, click the "Create Role" button on the Roles page. This will open the Create Role dialog where you can:
1. Define the role name
2. Set the role as Active or Inactive (Active roles have higher privileges, typically for Administrators and SuperAdmins)
3. Assign specific permissions for pages and functions (e.g., add, edit, delete units)

<TransformWrapper
  defaultScale={1}
  defaultPositionX={0}
  defaultPositionY={0}
  wheel={{ step: 0.1 }}
  doubleClick={{ disabled: true }}
>
  <TransformComponent>
    <img src="/img/createroll.png" alt="Create Role Dialog"vwidth="100%" />
  </TransformComponent>
</TransformWrapper>

:::note
**Active Roles**: Roles marked as Active have elevated privileges in the system and are typically granted to Administrator and SuperAdmin users. Inactive roles have limited permissions.
:::