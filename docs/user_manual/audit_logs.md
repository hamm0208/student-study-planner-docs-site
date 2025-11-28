---
sidebar_position: 8
---

# Audit Logs

import { TransformWrapper, TransformComponent } from "react-zoom-pan-pinch";

## Filtering
Multiple filtering options:
1. Module
2. Action
3. Role
4. User
5. Start Date - End Date

<TransformWrapper
  defaultScale={1}
  defaultPositionX={0}
  defaultPositionY={0}
  wheel={{ step: 0.1 }}
  doubleClick={{ disabled: true }}
>
  <TransformComponent>
    <img src="/img/audit_log_filter.png" alt="Add Year"vwidth="100%" />
  </TransformComponent>
</TransformWrapper>

## List of Logs

### Logs
Each of the logs contains:
1. `Date & Time` of action
2. `User` that did the action
3. `Actions` of the user
4. `Module` that the action was taken
5. `View Details` to view more details about the action

<TransformWrapper
  defaultScale={1}
  defaultPositionX={0}
  defaultPositionY={0}
  wheel={{ step: 0.1 }}
  doubleClick={{ disabled: true }}
>
  <TransformComponent>
    <img src="/img/audit_logs_table.png" alt="Add Year"vwidth="100%" />
  </TransformComponent>
</TransformWrapper>

--- 

### Details
Each of the log contains accurate details of the changes in JSON format
<TransformWrapper
  defaultScale={1}
  defaultPositionX={0}
  defaultPositionY={0}
  wheel={{ step: 0.1 }}
  doubleClick={{ disabled: true }}
>
  <TransformComponent>
    <img src="/img/audit_logs_details.png" alt="Add Year"vwidth="100%" />
  </TransformComponent>
</TransformWrapper>