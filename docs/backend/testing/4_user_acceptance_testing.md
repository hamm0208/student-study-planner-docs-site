---
sidebar_position: 4
---

# UAT Session

The UAT focused on assessing both the functional performance and user experience of the system to ensure readiness for deployment. Participants were observed not only on the completion of assigned tasks but also on how intuitively they interacted with the system. While specific test scenarios were provided, all users were naturally inclined to explore the system independently, demonstrating engagement. The testing covered the following areas:

## System & General Functions

- Verification of user authentication, session handling, and dashboard responsiveness.
- Review of audit logs, error handling, and system performance.

## Study Planner Management

- Testing of planner import, conflict resolution, and editing features for HODs and course coordinators.

## Role-Based Access & User Management

- Validation of Super Admin capabilities to create, assign, and update user roles and permissions.
- Assessment of role-based dashboards, access restrictions, and account management functions (activation, deactivation, reset).

## Student Access & Interaction

- Testing of personalized study planner viewing, update notifications, and cross-device accessibility.
- Verification of data accuracy and completeness from the student perspective.

## User Experience (UX) Evaluation

- Observation of how easily users could navigate and perform tasks without guidance.
- Evaluation of system intuitiveness, layout clarity, and responsiveness to user actions.

## Participants & Feedbacks

Participants are assigned their own user role to be used for testing the different workflows. The roles are assigned based on their background.

| Participant ID | User Role          | Background         | Device Used |
| :------------- | :----------------- | :----------------- | :---------- |
| 1              | Super Admin        | Senior Lecturer    | Desktop     |
| 2              | Course Coordinator | Head of Department | Laptop      |
| 3              | Super Admin        | Lecturer           | Laptop      |
| 4              | Super Admin        | Head of Department | Laptop      |
| 5              | Super Admin        | Head of Department | Laptop      |

### Feedback

#### Positive Feedback

| Feature                  | Comment                                                    |
| ------------------------ | ---------------------------------------------------------- |
| Study Plan Overview      | Users appreciated the clear overview.                      |
| Amendment History        | The log was found to be very useful.                       |
| Auto-conflict Resolution | This feature was highlighted as a major time-saver.        |
| Conflict Listing         | Clear listing of conflicts was praised.                    |
| Flexibility              | Users liked the ability to easily change and modify units. |

#### Areas for Improvement

| Module                   | Issue                                     | Suggestion                                        |
| ------------------------ | ----------------------------------------- | ------------------------------------------------- |
| Unit                     | Multiple unit versions may confuse users. | Add a log or last modified date/time.             |
| Unit Type                | "Save Changes" button is hidden.          | Enable Enter key shortcut or save.                |
| Term                     | Unsure of required input format.          | Add placeholder text for guidance.                |
| Course Intake            | Clunky workflow for new intakes.          | Go directly to the study planner after creation.  |
| Master Study Planner     | Hard to find conflicting units.           | Add a "Conflict" tab or a "Next Conflict" button. |
|                          | Excessive scrolling to reach buttons.     | Add a sticky toolbar.                             |
|                          | Unit swapping is inconvenient.            | Implement drag-and-drop functionality.            |
|                          | Credit points could be more informative.  | Show total credit points for each unit type.      |
| Student                  | Users see students outside their scope.   | Restrict student visibility by faculty/section.   |
| Student Information      | Lack of conversation logs for HODs.       | Add a communication log.                          |
| Personal Study Planner   | Email to student lacks customisation.     | Add a field for custom messages.                  |
|                          | Can only email one recipient.             | Allow multiple recipients.                        |
|                          | Unit swapping is inconvenient.            | Implement drag-and-drop functionality.            |
| User                     | Hard-deleting users removes audit logs.   | Change user status to "Inactive" instead.         |
| Audit Logs               | Filter input is unclear.                  | Add a border to the input field.                  |
| Overall System (UI/UX)   | Inconsistent layout, overwhelming colors. | -                                                 |
| Overall System (Perf)    | Slow database connection.                 | -                                                 |
| Overall System (General) | Lack of keyboard shortcuts.               | Add common shortcuts (e.g., Ctrl+Z).              |
The suggestions provided have been carefully reviewed, and a majority have been promptly resolved. Certain items, particularly those related to subjective UI/UX preferences, require further discussion. The following points have been earmarked for future consideration:

| Module                   | Issue                                     | Suggestion                             |
| ------------------------ | ----------------------------------------- | -------------------------------------- |
| Unit                     | Multiple unit versions may confuse users. | Add a log or last modified date/time.  |
| Master Study Planner     | Excessive scrolling to reach buttons.     | Add a sticky toolbar.                  |
| Master Study Planner     | Unit swapping is inconvenient.            | Implement drag-and-drop functionality. |
| Student Information      | Lack of conversation logs for HODs.       | Add a communication log.               |
| Personal Study Planner   | Email to student lacks customisation.     | Add a field for custom messages.       |
| Personal Study Planner   | Unit swapping is inconvenient.            | Implement drag-and-drop functionality. |
| Overall System (General) | Lack of keyboard shortcuts.               | Add common shortcuts (e.g., Ctrl+Z).   |
