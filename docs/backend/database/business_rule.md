---
sidebar_position: 2
---

# Business Rules

Business rules define the constraints, validations, and logical requirements that govern how the Academic Study Planner application operates. These rules ensure data integrity, maintain consistency, and enforce domain logic across the system.

:::info
These rules apply to core operations. Additional domain-specific rules may exist in the API layer and should be enforced at both database and application levels.
:::

---

## User & Authentication Rules

### User Account Management
- **Unique Email**: Each user must have a unique email address in the system
- **Email Verification**: Users must verify their email before accessing the application
- **Password Requirements**:
  - Minimum 8 characters
  - Must contain at least one uppercase letter
  - Must contain at least one lowercase letter
  - Must contain at least one digit
- **Account Activation**: New accounts are inactive until email verification is completed
- **Session Timeout**: User sessions expire after 30 minutes of inactivity
- **One Login Per Device**: Users can have multiple active sessions across different devices

### User Roles & Permissions
- **Student**: Can create and manage their own study plans and sessions
- **Admin**: Can manage users, view analytics, and system-wide settings
- **Default Role**: All new users are assigned the "Student" role by default

---

## Student Profile Rules

### Profile Information
- **Required Fields**: 
  - First Name
  - Last Name
  - Student ID (unique)
  - Email (unique)
- **Optional Fields**:
  - Phone Number
  - Department/Faculty
  - Year of Study
  - Profile Picture
- **Profile Completeness**: A profile is considered complete when all required fields are filled
- **Student ID Uniqueness**: Each student ID can only be associated with one user account

### Profile Updates
- **Email Change**: Requires re-verification of new email address
- **Student ID Change**: Can only be changed once per calendar year
- **Data Audit Trail**: All profile changes are logged with timestamps and user information

---

## Study Plan Rules

### Creation & Ownership
- **One Active Plan**: A student can have only one active study plan at a time
- **Plan Ownership**: Students can only view and edit their own study plans
- **Plan Archival**: Completed or obsolete plans must be marked as archived
- **Historical Records**: Archived plans are retained for at least 2 years for academic records

### Plan Structure
- **Mandatory Fields**:
  - Plan Name
  - Start Date
  - Target End Date
  - At least one Subject
- **Date Validation**:
  - Start Date must be before Target End Date
  - Start Date cannot be in the past (unless modified by admin)
  - Target End Date cannot be more than 2 years in the future
- **Plan Duration**: Minimum duration is 1 week, maximum is 2 years

### Plan Status
- **Status Values**: `DRAFT`, `ACTIVE`, `PAUSED`, `COMPLETED`, `ARCHIVED`
- **Status Transitions**:
  - `DRAFT` → `ACTIVE`: Allowed only with at least one subject
  - `ACTIVE` → `PAUSED`: Allowed at any time
  - `PAUSED` → `ACTIVE`: Allowed anytime
  - `ACTIVE`/`PAUSED` → `COMPLETED`: Allowed when plan end date is reached
  - Any status → `ARCHIVED`: Allowed at any time
- **Default Status**: New plans start in `DRAFT` status

---

## Subject Rules

### Subject Management
- **Unique Subject Per Plan**: A student cannot have duplicate subjects in the same plan
- **Subject Categorization**: Each subject must be assigned a category (e.g., Science, Mathematics, Language)
- **Subject Code**: Optional field, must be unique if provided (e.g., CS101, MATH201)
- **Target Grade**: Optional, but if set must be A-F or a percentage (0-100)

### Subject Requirements
- **Mandatory Fields**:
  - Subject Name
  - Plan Association
- **Optional Fields**:
  - Subject Code
  - Description
  - Credits/Units
  - Target Grade
  - Pass/Fail Mark

### Subject Status
- **Status Values**: `NOT_STARTED`, `IN_PROGRESS`, `COMPLETED`, `ABANDONED`
- **Status Progression**: Cannot skip from `NOT_STARTED` to `COMPLETED` without `IN_PROGRESS`
- **Default Status**: New subjects start as `NOT_STARTED`

---

## Study Session Rules

### Session Creation
- **Session Duration**: 
  - Minimum: 15 minutes
  - Maximum: 12 hours
  - Recommended: 30-120 minutes
- **Subject Association**: Each session must be associated with a subject
- **Date & Time Validation**:
  - Session date cannot be in the future (past sessions only)
  - Session cannot extend beyond the study plan's end date
  - Session start time must be before end time
- **One Session Per Timeframe**: No overlapping sessions for the same subject on the same day

### Session Status
- **Status Values**: `SCHEDULED`, `IN_PROGRESS`, `COMPLETED`, `CANCELLED`
- **Default Status**: `SCHEDULED`
- **Completion Rules**:
  - Sessions can only be marked complete after the scheduled end time
  - Cancelled sessions cannot be reactivated
  - Completion time must be within the scheduled timeframe

### Session Tracking
- **Required Metrics**:
  - Session Duration (actual vs. planned)
  - Topics Covered
  - Productivity Rating (1-5 scale)
  - Notes
- **Progress Tracking**: Each session contributes to subject progress percentage

---

## Task & Assignment Rules

### Task Creation
- **Task Association**: Each task must be linked to a subject or study session
- **Priority Levels**: `LOW`, `MEDIUM`, `HIGH`, `URGENT`
- **Task Types**: `READING`, `PRACTICE`, `QUIZ`, `PROJECT`, `ASSIGNMENT`, `REVIEW`
- **Date Constraints**:
  - Due date cannot be in the past
  - Due date must be within the study plan duration
  - Reminder dates must be before the due date

### Task Status & Completion
- **Status Values**: `TODO`, `IN_PROGRESS`, `COMPLETED`, `OVERDUE`, `CANCELLED`
- **Default Status**: `TODO`
- **Overdue Handling**: Tasks automatically transition to `OVERDUE` if not completed by due date
- **Completion Requirement**: Task description or completion notes are mandatory

### Task Organization
- **Ordering**: Tasks can be ordered by priority, due date, or custom sequence
- **Dependencies**: A task can have dependencies on other tasks (optional feature)
- **Subtasks**: Main tasks can have up to 10 subtasks

---

## Progress & Performance Rules

### Progress Calculation
- **Subject Progress**: Calculated as percentage of completed sessions relative to total planned sessions
- **Overall Plan Progress**: Weighted average of all subject progress percentages
- **Session Contribution**: Each completed session contributes equally to subject progress
- **Update Frequency**: Progress is recalculated in real-time upon session completion

### Performance Metrics
- **Completion Rate**: (Completed Sessions / Total Sessions) × 100%
- **Session Adherence**: Measures if actual duration matches planned duration (±10% tolerance)
- **Productivity Trend**: Average productivity rating over the last 7 days
- **Time Management**: Tracks if sessions are completed on schedule

### Target Achievement
- **Grade Targets**: If a target grade is set for a subject, achievement is tracked separately
- **Subject Completion**: A subject is marked complete when all planned sessions are finished
- **Plan Completion**: A plan is complete when all subjects reach completion status

---

## Notification & Reminder Rules

### Automatic Reminders
- **Session Reminders**: Sent 1 day and 1 hour before scheduled session
- **Task Due Date Reminders**: Sent 3 days, 1 day, and 2 hours before due date
- **Overdue Notifications**: Sent daily for overdue tasks until completion
- **Plan Milestone Alerts**: Sent when 25%, 50%, 75%, and 100% of plan is complete

### Notification Preferences
- **Opt-Out Options**: Users can disable reminders for specific session types or task categories
- **Notification Channels**: Email notifications for critical alerts (overdue, plan completion)
- **Quiet Hours**: Users can set quiet hours (e.g., 10 PM - 8 AM) to suppress notifications

---

## Data Integrity & Consistency Rules

### Constraints
- **Referential Integrity**: All foreign key relationships are enforced
- **Cascade Delete Rules**:
  - Deleting a Study Plan cascades to Sessions and Tasks
  - Deleting a Subject cascades to associated Sessions
  - Deleting a Session does NOT cascade (user must manually handle tasks)
- **Orphaned Data Prevention**: No Tasks or Sessions without parent Subject/Plan

### Data Validation
- **Text Fields**: 
  - Maximum 255 characters for names and titles
  - Maximum 2000 characters for descriptions
  - No null values for mandatory fields
- **Date Fields**: All dates must be in valid ISO 8601 format
- **Numeric Fields**: 
  - Duration in minutes (positive integers only)
  - Ratings on 1-5 scale
  - Progress percentage (0-100)

### Timestamps
- **Created At**: Automatically set to current timestamp, immutable after creation
- **Updated At**: Automatically updated whenever record is modified
- **Soft Deletes**: Records are marked as deleted rather than permanently removed (retention period: 90 days)

---

## Security & Privacy Rules

### Data Access Control
- **Data Isolation**: Students can only access their own data
- **Admin Access**: Admins can view all user data but cannot modify without audit trail
- **Read-Only Access**: Users can grant read-only access to specific people (future feature)

### Data Protection
- **Encryption**: Sensitive data (passwords) are encrypted using bcrypt
- **API Security**: All API endpoints require authentication (JWT tokens)
- **Rate Limiting**: API endpoints have rate limits (100 requests per minute per user)
- **Input Sanitization**: All user inputs are sanitized to prevent XSS and SQL injection

### Privacy Compliance
- **GDPR Compliance**: Users can request and download their data
- **Data Deletion**: User can request permanent deletion of all their data (30-day grace period)
- **Activity Logs**: All user actions are logged for security audit purposes

---

## System & Performance Rules

### Data Retention
- **Active Plans**: Retained indefinitely until user deletion
- **Archived Plans**: Retained for minimum 2 years
- **Sessions**: Retained for minimum 5 years (for academic records)
- **Audit Logs**: Retained for minimum 1 year
- **Deleted Data**: Soft-deleted data retained for 90 days before permanent removal

### Backup & Recovery
- **Backup Frequency**: Daily automated backups
- **Backup Retention**: Kept for 30 days
- **Recovery Time Objective (RTO)**: 4 hours
- **Recovery Point Objective (RPO)**: 24 hours

### Concurrent Access
- **Optimistic Locking**: Used for preventing conflicting updates
- **Last-Write-Wins**: If conflicts occur, the most recent update takes precedence
- **User Notifications**: Users are notified if their data has been modified by the system

---

## Business Logic Rules

### Study Plan Intelligence
- **Recommended Session Duration**: Based on subject complexity (range: 30-120 minutes)
- **Study Breaks**: System recommends 5-10 minute breaks for sessions over 60 minutes
- **Load Balancing**: System suggests distributing study sessions evenly across days

### Motivation & Gamification (Optional)
- **Streak Counter**: Tracks consecutive days with completed sessions
- **Badges**: Users earn badges for milestones (e.g., 100 sessions, 30-day streak)
- **Progress Visualization**: Visual charts show progress trends over time

---

## Compliance & Audit Rules

### Audit Logging
- **Auditable Actions**: User login, data modifications, plan status changes, session completion
- **Log Retention**: Audit logs retained for minimum 2 years
- **Admin Review**: Admins can view audit logs for any user

### Regulatory Compliance
- **Educational Standards**: Compliant with institutional study tracking standards
- **Data Protection**: Follows data protection regulations (GDPR, CCPA where applicable)
- **Accessibility**: Interface compliant with WCAG 2.1 Level AA standards

---

## Summary Table

| Entity | Key Rules | Status |
|--------|-----------|--------|
| User | Unique email, verified account, session timeout | Active |
| Study Plan | One active plan, date validation, status transitions | Active |
| Subject | Unique per plan, category required | Active |
| Study Session | No overlap, min 15 min, max 12 hours | Active |
| Task | Due date validation, priority levels | Active |
| Progress | Real-time calculation, performance metrics | Active |
| Security | JWT auth, role-based access, data encryption | Active |