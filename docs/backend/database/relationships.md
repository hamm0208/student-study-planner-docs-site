---
sidebar_position: 4
---

# Database Relationships

This document explains the key relationships between entities in the database and how data flows through the system.

---

## Core Relationship Patterns

### 1. Student Enrollment Hierarchy

**Flow**: User → Student → CourseIntake → Course

A user (via UserProfile) can be associated with a **Student** record. The Student is enrolled in a specific **CourseIntake** (intake cohort), which links to a **Course** (degree program) and **Major** (specialization).

**Key Points**:
- One user can have multiple Student records (multi-enrollment)
- Student ID is a Decimal to accommodate institutional formats
- CourseIntake represents a specific cohort/batch
- Major is a specialization within a Course

**Query Example**: Get all courses a student is enrolled in
```sql
SELECT c.* FROM Student s
JOIN CourseIntake ci ON s.IntakeID = ci.ID
JOIN Course c ON s.CourseID = c.ID
WHERE s.StudentID = [student_id]
```

---

### 2. Study Planning Hierarchy

**Flow**: CourseIntake → MasterStudyPlanner → SemesterInStudyPlannerYear → UnitInSemesterStudyPlanner → Unit

A **CourseIntake** has a **MasterStudyPlanner** that represents the overall study plan. This breaks down into **SemesterInStudyPlannerYear** for each year/semester combination. Each semester contains **UnitInSemesterStudyPlanner** entries that reference actual **Unit** records.

**Key Points**:
- One MasterStudyPlanner per CourseIntake
- Semesters are uniquely identified by Year + SemType
- Units are placed in specific semester slots
- Multiple semesters can span across years

**Query Example**: Get all units in Year 1, Semester 1 for a specific course intake
```sql
SELECT u.* FROM UnitInSemesterStudyPlanner usp
JOIN SemesterInStudyPlannerYear ssp ON usp.SemesterInStudyPlannerYearID = ssp.ID
JOIN Unit u ON usp.UnitID = u.ID
WHERE ssp.MasterStudyPlannerID = [planner_id]
  AND ssp.Year = 1
  AND ssp.SemType = 'Semester 1'
```

---

### 3. Unit Prerequisites & Requirements

**Flow**: Unit ↔ UnitRequisiteRelationship ↔ Unit

Units can have **prerequisites** and **corequisites** via the **UnitRequisiteRelationship** table. This creates relationships between units with specific conditions (e.g., "must complete Unit A with minimum C+ OR Unit B").

**Key Points**:
- Self-referential through UnitRequisiteRelationship
- Supports complex logic via LogicalOperators field
- MinCP field specifies minimum credit points requirement
- UnitRelationship describes the type (prerequisite, corequisite, etc.)

**Query Example**: Get all prerequisites for a unit
```sql
SELECT ru.* FROM UnitRequisiteRelationship ur
JOIN Unit ru ON ur.RequisiteUnitID = ru.ID
WHERE ur.UnitID = [unit_id]
  AND ur.UnitRelationship = 'prerequisite'
```

---

### 4. Student Progress Tracking

**Flow**: Student → UnitHistory

**Student** records are linked to **UnitHistory**, which tracks every unit a student has completed or attempted. This includes completion status, term taken, and year.

**Key Points**:
- Multiple UnitHistory entries per student (one per unit taken)
- Status indicates pass/fail/incomplete
- TermID and Year identify when unit was taken
- UnitID references the actual Unit definition

**Query Example**: Get all units completed by a student
```sql
SELECT u.*, uh.Status, uh.Year FROM UnitHistory uh
JOIN Unit u ON uh.UnitID = u.ID
WHERE uh.StudentID = [student_id]
  AND uh.Status = 'Pass'
ORDER BY uh.Year
```

---

### 5. Study Plan Amendments & Changes

**Flow**: Student → StudentStudyPlannerAmmendments

The **StudentStudyPlannerAmmendments** table tracks all changes made to a student's study plan. This includes unit swaps, additions, removals, and other modifications.

**Key Points**:
- Audit trail of all plan modifications
- Action field describes what was changed
- TimeofAction is automatically timestamped
- Supports before/after unit comparison (UnitID vs NewUnitID)
- References both UnitType and Unit for context

**Common Actions**:
- `ADD_UNIT` - Unit added to plan
- `REMOVE_UNIT` - Unit removed from plan
- `SWAP_UNIT` - Unit replaced with another
- `CHANGE_TYPE` - Unit type/classification changed

**Query Example**: Get amendment history for a student
```sql
SELECT * FROM StudentStudyPlannerAmmendments
WHERE StudentID = [student_id]
ORDER BY TimeofAction DESC
```

---

### 6. Role-Based Access Control

**Flow**: User → UserProfile → UserRole → Role → RolePermission → Permission

Users have access control through a hierarchical permission system:
- **User** has a **UserProfile**
- **UserProfile** has assigned **UserRole**(s)
- **Role** has multiple **Permission**(s) via **RolePermission**
- **Permission** defines what actions are allowed on which resources

**Key Points**:
- Role-based not attribute-based
- Multiple roles per user possible
- Roles can expire (ExpiresAt field)
- Permissions have Resource, Action, and Module fields
- Granted field allows permission denial

**Query Example**: Get all permissions for a user
```sql
SELECT DISTINCT p.* FROM UserProfile up
JOIN UserRole ur ON up.ID = ur.UserProfileID
JOIN RolePermission rp ON ur.RoleID = rp.RoleID
JOIN Permission p ON rp.PermissionID = p.ID
WHERE up.UserID = [user_id]
  AND rp.Granted = true
  AND (ur.ExpiresAt IS NULL OR ur.ExpiresAt > NOW())
```

---

### 7. Unit Availability Across Terms

**Flow**: Unit → UnitTermOffered

**UnitTermOffered** specifies in which academic terms a unit is available. This allows representing that certain units are only offered in specific terms.

**Key Points**:
- Multiple term offerings per unit
- TermType indicates the semester/period
- Helps with study plan validation
- Prevents placing units in terms they're not offered

**Query Example**: Check if unit is offered in a specific term
```sql
SELECT * FROM UnitTermOffered
WHERE UnitID = [unit_id]
  AND TermType = [term_type]
```

---

## Important Relationships to Remember

### 1. User vs Student Separation
- **User** = System user account (login credentials, access)
- **Student** = Academic enrollment record
- One User can have multiple Students (multi-degree students)
- They're connected via `users.ID` → `UserProfile.UserID` → `Student` (indirect)

### 2. Cascade Delete Behaviors
These relationships have **cascade deletes**:
- Delete CourseIntake → deletes MasterStudyPlanner, Student records
- Delete MasterStudyPlanner → deletes SemesterInStudyPlannerYear, UnitInSemesterStudyPlanner
- Delete Student → deletes StudentStudyPlannerAmmendments, UnitHistory
- Delete Unit → deletes UnitInSemesterStudyPlanner, UnitHistory, UnitTermOffered

These relationships use **SetDefault** on delete:
- Course deletion on Major (keeps Major but nullifies Course reference)
- Major deletion on Student

### 3. Audit Trail
**AuditLog** tracks all system changes:
- User login/logout
- Data modifications
- Study plan amendments are manually logged via StudentStudyPlannerAmmendments
- Includes UserID, Action, Module, Details, IP Address, and Timestamp

---

## Common Query Patterns

### Get a Student's Full Study Plan
```sql
SELECT
  sp.Year,
  sp.SemType,
  u.Code,
  u.Name,
  u.CreditPoints,
  ut.Name as UnitType
FROM MasterStudyPlanner msp
JOIN SemesterInStudyPlannerYear sp ON msp.ID = sp.MasterStudyPlannerID
JOIN UnitInSemesterStudyPlanner uis ON sp.ID = uis.SemesterInStudyPlannerYearID
JOIN Unit u ON uis.UnitID = u.ID
LEFT JOIN UnitType ut ON uis.UnitTypeID = ut.ID
WHERE msp.CourseIntakeID = [intake_id]
ORDER BY sp.Year, sp.SemType
```

### Get Student's Completed vs Planned Units
```sql
-- Planned units
SELECT u.* FROM UnitInSemesterStudyPlanner uis
JOIN Unit u ON uis.UnitID = u.ID
WHERE uis.SemesterInStudyPlannerYearID = [semester_id]

-- Completed units
SELECT u.*, uh.Status FROM UnitHistory uh
JOIN Unit u ON uh.UnitID = u.ID
WHERE uh.StudentID = [student_id]
```

### Get Unit Prerequisites
```sql
SELECT u.Code, u.Name, ur.UnitRelationship
FROM UnitRequisiteRelationship ur
JOIN Unit u ON ur.RequisiteUnitID = u.ID
WHERE ur.UnitID = [unit_id]
```

### Get User's Permissions
```sql
SELECT DISTINCT p.*, r.Name as RoleName
FROM UserProfile up
JOIN UserRole ur ON up.ID = ur.UserProfileID
JOIN RolePermission rp ON ur.RoleID = rp.RoleID
JOIN Permission p ON rp.PermissionID = p.ID
JOIN Role r ON rp.RoleID = r.ID
WHERE up.UserID = [user_id]
  AND rp.Granted = true
```

---

## Best Practices When Working with These Relationships

1. **Always consider cascade deletes** - Deleting a parent record will cascade
2. **Use timestamps** - StudentStudyPlannerAmmendments includes UTC timestamps for audit trails
3. **Check user permissions** - Validate against Role/Permission before returning user-specific data
4. **Validate unit offerings** - Check UnitTermOffered before placing units in semesters
5. **Handle self-referential units** - UnitRequisiteRelationship references Unit both ways
6. **Consider multi-enrollment** - Users can have multiple Student records; filter carefully
7. **Track amendments** - Always log study plan changes to StudentStudyPlannerAmmendments

