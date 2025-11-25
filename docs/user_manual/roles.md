# Roles
import { TransformWrapper, TransformComponent } from "react-zoom-pan-pinch";

## Overview
:::note
- This page is where permission to certain roles are given
- Certain roles have access to certain page and granted certain functions for example, adding or editting units.
:::

Here is the roles page, and all the roles that are currently available on the system
<TransformWrapper
  defaultScale={1}
  defaultPositionX={0}
  defaultPositionY={0}
  wheel={{ step: 0.1 }}
  doubleClick={{ disabled: true }}
>
  <TransformComponent>
    <img src="/img/rolemanagementadd.png" alt="Role management page"vwidth="100%" />
  </TransformComponent>
</TransformWrapper>

This is the create role page, to access, press the "Create Role" button. Active role means the role has a high function in the system, mostly granted to "Administrator" and "SuperAdmin" role
<TransformWrapper
  defaultScale={1}
  defaultPositionX={0}
  defaultPositionY={0}
  wheel={{ step: 0.1 }}
  doubleClick={{ disabled: true }}
>
  <TransformComponent>
    <img src="/img/createroll.png" alt="Create Roll"vwidth="100%" />
  </TransformComponent>
</TransformWrapper>