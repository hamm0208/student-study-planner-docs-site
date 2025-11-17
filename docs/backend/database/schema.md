---
sidebar_position: 3
---

import { TransformWrapper, TransformComponent } from "react-zoom-pan-pinch";

# Database Schema

## Overview

The database schema is designed to support an academic study planning system. It manages course structures, student enrollments, academic units (subjects), and study plan tracking.

## Core Model Groups

### Authentication & Access Control
- **users** - User accounts with email and authentication credentials
- **UserProfile** - Extended user information and group access
- **Role** - Role definitions for access control
- **UserRole** - Assignment of roles to users
- **Permission** - Permission definitions
- **RolePermission** - Mapping of permissions to roles
- **user_group_access** - User group classifications

### Academic Structure
- **Course** - Degree programs/courses
- **Major** - Specializations within a course
- **Unit** - Individual academic units/subjects
- **Term** - Academic terms and periods
- **UnitType** - Classification of unit types
- **UnitTermOffered** - Availability of units in specific terms
- **UnitRequisiteRelationship** - Prerequisites and corequisites between units

### Student & Progress Tracking
- **Student** - Student enrollment records
- **CourseIntake** - Student intake/enrollment cohorts
- **MasterStudyPlanner** - Overall study plans for course intakes
- **SemesterInStudyPlannerYear** - Semester-level study planning
- **UnitInSemesterStudyPlanner** - Units placed in specific semesters
- **StudentStudyPlannerAmmendments** - Change history and amendments to plans
- **UnitHistory** - Student's unit completion history

### Audit & Logging
- **AuditLog** - System activity and change tracking

---

## Database Diagram

### Entity Relationship Diagram (ERD)

<TransformWrapper
  defaultScale={1}
  defaultPositionX={0}
  defaultPositionY={0}
  wheel={{ step: 0.1 }}
  doubleClick={{ disabled: true }}
>
  <TransformComponent>
    <img
      src="/img/db-schema.svg"
      alt="Database Schema"
      style={{ width: "100%", height: "auto", display: "block" }}
    />
  </TransformComponent>
</TransformWrapper>

*Scroll to Zoom, Pinch to Move Around*

---

## Key Relationships

### Student Academic Journey
```
Student → CourseIntake → Course
        → Major
        → UnitHistory (past units taken)
```

### Study Planning Flow
```
CourseIntake → MasterStudyPlanner
            → SemesterInStudyPlannerYear (per year/semester)
            → UnitInSemesterStudyPlanner (units in that semester)
            → Unit
```

### Unit Structure
```
Unit → UnitRequisiteRelationship (prerequisites)
    → UnitTermOffered (when available)
    → UnitType (classification)
```

### User Permissions
```
User → UserProfile → UserRole → Role → RolePermission → Permission
    → AuditLog (activity tracking)
```

---

## Design Notes

### Cascade Behaviors
- Deleting a **CourseIntake** cascades to MasterStudyPlanner and Student records
- Deleting a **MasterStudyPlanner** cascades to semester and unit placements
- Deleting a **Student** cascades to their study amendments and unit history
- Unit deletions cascade to study planner references

### Important Constraints
- **Student ID** is the primary identifier for student records (Decimal type)
- **User ID** is separate from Student ID - one user can have multiple student records
- **Email uniqueness** - enforced at users table level
- **Unit Code** must be unique across the system
- **Course Code** must be unique for primary identification

### Data Types
- Small integers (`SmallInt`) used for IDs to optimize storage
- Decimal used for StudentID to handle institutional ID formats
- Varchar used for most string data
- Timestamps include UTC timezone handling for amendments

---

## Accessing the Schema

To view or modify the schema:

1. **Prisma Schema** - Located at `./prisma/schema.prisma`
2. **Migrations** - Located at `./prisma/migrations/` - contains SQL change history
3. **Visual inspection** - Use the ERD diagram above (scroll to zoom)

For detailed column definitions, constraints, and defaults, refer to the Prisma schema file or the [Useful Commands](./useful_commands) documentation for how to export the schema.
