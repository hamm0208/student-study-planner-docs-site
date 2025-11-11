---
sidebar_position: 2
---


# API Endpoints

**Base path:** `src/api`

*Note: Routes shown are derived from the project's `src/app/api` folder structure. Most endpoints are protected and require an `Authorization` Bearer token unless noted otherwise (e.g., login).*

---

## Endpoint Summary Table

| Endpoint | Method(s) | Short description |
|---|---|---|
| `/api/auth/user-login` | `POST` | Authenticate user and return JWT |
| `/api/users` | `GET`, `POST` | List users / create user |
| `/api/users/user-roles` | `GET`, `POST`, `DELETE` | Manage user roles (assign / revoke) |
| `/api/roles` | `GET`, `POST` | List roles / create role |
| `/api/roles/{id}/permissions` | `GET`, `PUT` | Read or update permissions for a role |
| `/api/audit_logs` | `GET` | List audit logs |
| `/api/audit_logs/latest_for_entity` | `GET` | Get latest audit log entries per entity |
| `/api/dashboard` | `GET` | Dashboard metrics / summary |
| `/api/send-email` | `POST` | Send an email (notifications) |
| `/api/course` | `GET`, `POST` | List / create courses |
| `/api/course/course_intake` | `GET`, `POST` | Manage course intakes |
| `/api/course/major` | `GET`, `POST` | List / create majors |
| `/api/course/major/major_intake` | `GET`, `POST` | Manage major-specific intakes |
| `/api/course/master_study_planner` | `GET`, `POST` | Master study planner operations |
| `/api/course/get_available_intakes` | `GET` | Retrieve available intakes for a course/major |
| `/api/course/get_planner_data` | `GET` | Get planner data (for frontend planner) |
| `/api/course-upload` | `POST` | Upload course data (CSV / file) |
| `/api/unit` | `GET`, `POST` | List / create units (courses/modules) |
| `/api/unit/unit_requisite` | `GET`, `POST` | Manage unit requisites (prereqs) |
| `/api/unit/unit_term_offered` | `GET`, `POST` | Manage terms when a unit is offered |
| `/api/unit/unit-upload` | `POST` | Upload unit data (file) |
| `/api/unit/**` | ... | See nested unit endpoints below |
| `/api/unit_history` | `GET`, `POST` | Manage unit history records |
| `/api/unit-history-upload` | `POST` | Upload unit-history data |
| `/api/unit_in_semester_study_planner` | `GET`, `POST` | Manage units within a semester planner |
| `/api/unit_type` | `GET`, `POST` | Manage unit types |
| `/api/students` | `GET`, `POST` | List / create students |
| `/api/students/student_study_planner_ammendments` | `GET`, `POST` | Manage study planner amendments |
| `/api/students/student_unit_history` | `GET`, `POST` | Manage student unit history |
| `/api/semester_in_study_planner_year` | `GET`, `POST` | Manage study-planner year / semester settings |
| `/api/term` | `GET`, `POST` | Manage academic terms |
| `/api/term/term-upload` | `POST` | Upload term data |

---

## Common Usage

> **Common Headers (Protected Endpoints)**
> ```
> Authorization: Bearer <jwt_token>
> Content-Type: application/json (or multipart/form-data for uploads)
> ```

> **Common Query Params**
> * `page` (integer, optional): Pagination page
> * `limit` (integer, optional): Items per page
> * `search` (string, optional): Search term
> * `sort` (string, optional): Sort field (e.g. `createdAt:desc`)

> **Common Error Responses**
> * **400 Bad Request:** Invalid input
> * **401 Unauthorized:** Missing or invalid token
> * **403 Forbidden:** Insufficient permissions
> * **404 Not Found:** Resource not found
> * **500 Internal Server Error:** Server error

---

## Detailed Endpoint Descriptions

### Auth

#### `POST /api/auth/user-login`
* **Description:** Authenticate a user (email/password or other credential) and return an access token.
* **Access:** Public
* **Headers:**
    * `Content-Type: application/json`
* **Request Body:**
    * `email` (string, required): User email
    * `password` (string, required): User password
* **Example Request:**
    ```json
    {
      "email": "student@example.edu",
      "password": "s3cret"
    }
    ```
* **Example Response (200 OK):**
    ```json
    {
      "accessToken": "eyJhbGciOi...",
      "expiresIn": 3600,
      "user": {
        "id": "123",
        "name": "Student Name",
        "roles": ["student"]
      }
    }
    ```
* **Possible Errors:**
    * **400 Bad Request:** Missing fields
    * **401 Unauthorized:** Invalid credentials

---

### Users

#### `GET /api/users`
* **Description:** Retrieve a paginated list of users.
* **Access:** Protected
* **Query Params:**
    * `page` (integer, optional)
    * `limit` (integer, optional)
    * `search` (string, optional)
* **Example Response (200 OK):**
    ```json
    {
      "data": [
        { "id": "u1", "name": "Alice", "email": "alice@example.edu" }
      ],
      "meta": { "page": 1, "limit": 10, "total": 42 }
    }
    ```

#### `POST /api/users`
* **Description:** Create a new user.
* **Access:** Protected (admin)
* **Request Body:**
    * `name` (string, required)
    * `email` (string, required)
    * `password` (string, required)
    * `roles` (array of strings, optional)
* **Example Request:**
    ```json
    {
      "name": "Bob Student",
      "email": "bob@example.edu",
      "password": "password123",
      "roles": ["student"]
    }
    ```
* **Possible Errors:**
    * **400 Bad Request:** Validation failed
    * **403 Forbidden:** Insufficient permissions

#### `POST /api/users/user-roles`
* **Description:** Assign roles to a user (or manage user-role relations).
* **Access:** Protected (admin)
* **Request Body:**
    * `userId` (string, required)
    * `roles` (array of strings, required)
* **Example Request:**
    ```json
    {
      "userId": "u1",
      "roles": ["student","advisor"]
    }
    ```
* **Possible Errors:**
    * **404 Not Found:** User not found

---

### Roles

#### `GET /api/roles`
* **Description:** List roles.
* **Access:** Protected

#### `POST /api/roles`
* **Description:** Create a new role.
* **Request Body:**
    * `name` (string, required)
    * `description` (string, optional)
* **Example Response (201 Created):**
    ```json
    {
      "id": "role_admin",
      "name": "admin",
      "description": "Administrator role"
    }
    ```

#### `GET /api/roles/{id}/permissions`
* **Description:** Get permissions for a specific role.
* **Access:** Protected (admin)
* **Path Params:**
    * `id` (string, required): Role ID

#### `PUT /api/roles/{id}/permissions`
* **Description:** Update permissions for the specified role.
* **Access:** Protected (admin)
* **Path Params:**
    * `id` (string, required): Role ID
* **Request Body:**
    * `permissions` (array of strings, required)
* **Example Request:**
    ```json
    {
      "permissions": ["courses.read", "users.manage"]
    }
    ```
* **Possible Errors:**
    * **404 Not Found:** Role not found
    * **400 Bad Request:** Invalid permissions format

---

### Audit Logs

#### `GET /api/audit_logs`
* **Description:** Retrieve audit trail records (actions performed in the system).
* **Access:** Protected (auditor/admin)
* **Query Params:**
    * `entity` (string, optional)
    * `userId` (string, optional)
    * `startDate`, `endDate` (ISO date strings, optional)
    * `page`, `limit` (pagination)
* **Example Response (200 OK):**
    ```json
    {
      "data": [
        { "id": "al1", "entity": "student", "action": "UPDATE", "userId": "u1", "createdAt": "2025-11-01T10:00:00Z" }
      ],
      "meta": { "page": 1, "limit": 20, "total": 100 }
    }
    ```

#### `GET /api/audit_logs/latest_for_entity`
* **Description:** Return the most recent audit entry per entity (useful for snapshots).
* **Access:** Protected

---

### Dashboard

#### `GET /api/dashboard`
* **Description:** Return aggregated metrics for the dashboard (counts, charts, quick stats).
* **Access:** Protected
* **Query Params:**
    * `range` (string, optional): e.g., `last30`, `last7`
* **Example Response (200 OK):**
    ```json
    {
      "studentsCount": 1200,
      "activePlanners": 350,
      "recentChanges": [ /* list */ ]
    }
    ```

---

### Email

#### `POST /api/send-email`
* **Description:** Send system notification emails (course updates, planner notifications).
* **Access:** Protected (service account or admin)
* **Request Body:**
    * `to` (string | array, required)
    * `subject` (string, required)
    * `body` (string, required)
    * `html` (boolean, optional)
* **Example Request:**
    ```json
    {
      "to": "student@example.edu",
      "subject": "Planner updated",
      "body": "Your study planner was updated."
    }
    ```
* **Possible Errors:**
    * **400 Bad Request:** Invalid email
    * **500 Internal Server Error:** Mail provider failure

---

### Courses & Related

#### `GET /api/course`
* **Description:** List courses.
* **Access:** Protected

#### `POST /api/course`
* **Description:** Create a course.
* **Request Body (Example):**
    * `code` (string, required)
    * `title` (string, required)
    * `level` (string, optional)
* **Example Response (201 Created):**
    ```json
    {
      "id": "CSE101",
      "code": "CSE101",
      "title": "Intro to Computing"
    }
    ```

#### `GET, POST /api/course/course_intake`
* **Description:** Manage course intake terms (opening / closing intakes).
* **Access:** Protected

#### `GET, POST /api/course/major`
* **Description:** List or create majors.
* **Access:** Protected

#### `GET, POST /api/course/major/major_intake`
* **Description:** Manage major-specific intakes.
* **Access:** Protected

#### `GET /api/course/get_available_intakes`
* **Description:** Return intakes available for a given course or major.
* **Access:** Protected
* **Query Params:** `courseId`, `majorId`
* **Query Example:** `/api/course/get_available_intakes?courseId=CSE101&majorId=MAJ1`

#### `GET /api/course/get_planner_data`
* **Description:** Return precomputed planner data for building a study planner.
* **Access:** Protected
* **Query Params:** `courseId`, `majorId`, `intakeId`

#### `POST /api/course-upload`
* **Description:** Upload course data (CSV / spreadsheet).
* **Access:** Protected (file upload, `multipart/form-data`)
* **Form Data Body:**
    * `file` (file, required)
    * `importMode` (string, optional): e.g., `replace` | `append`
* **Possible Errors:**
    * **422 Unprocessable Entity:** File invalid format

---

### Units & Unit-related Endpoints

#### `GET /api/unit`
* **Description:** List units (subjects/modules).
* **Access:** Protected

#### `POST /api/unit`
* **Description:** Create a new unit.
* **Request Body (Example):**
    * `code` (string, required)
    * `title` (string, required)
    * `creditPoints` (number, optional)
    * `unitTypeId` (string, optional)
* **Possible Errors:**
    * **400 Bad Request:** Missing required fields
    * **409 Conflict:** Duplicate unit code

#### `POST /api/unit/unit_requisite`
* **Description:** Create or update prerequisites/corequisites for a unit.
* **Request Body:**
    * `unitId` (string, required)
    * `requisiteType` (string, required): `prereq` | `coreq`
    * `requisiteUnitIds` (array of strings, required)

#### `POST /api/unit/unit_term_offered`
* **Description:** Define which terms a unit is offered in (termId, year, campus).
* **Request Body:**
    * `unitId`, `termId`, `year`, `campusId`, etc.

#### `POST /api/unit/unit-upload`
* **Description:** Upload unit data file (CSV).
* **Access:** Protected (`multipart/form-data`)

#### `GET, POST /api/unit_history`
* **Description:** Manage versioned history for unit changes (create, list).
* **Access:** Protected

#### `POST /api/unit-history-upload`
* **Description:** Upload unit-history records (file import).
* **Access:** Protected (`multipart/form-data`)

#### `GET, POST /api/unit_in_semester_study_planner`
* **Description:** Manage specific unit entries inside a semester in a student's study planner (add / remove / reorder units).
* **Access:** Protected

#### `GET, POST /api/unit_type`
* **Description:** Manage unit types (e.g., lecture, lab, clinical).
* **Access:** Protected

---

### Students & Student Planner

#### `GET /api/students`
* **Description:** List students (search, filter, paginate).
* **Access:** Protected

#### `POST /api/students`
* **Description:** Create a student record.
* **Request Body:**
    * `studentId`, `name`, `email`, `intakeId`, `programId`

#### `GET, POST /api/students/student_study_planner_ammendments`
* **Description:** Retrieve or create amendments to a student's study planner (requests to change planner).
* **Access:** Protected (advisor/admin)

#### `GET, POST /api/students/student_unit_history`
* **Description:** Record or retrieve a student's completed/attempted units (grades, result).
* **Access:** Protected
* **Possible Errors:**
    * **404 Not Found:** Student not found

---

### Term

#### `GET, POST /api/semester_in_study_planner_year`
* **Description:** Manage planner-year specific semester configuration (e.g., semester labels per planner year).
* **Access:** Protected

#### `GET, POST /api/term`
* **Description:** Manage academic terms (e.g., Term 1, Term 2).
* **Access:** Protected

#### `POST /api/term/term-upload`
* **Description:** Upload term definitions file.
* **Access:** Protected (`multipart/form-data`)

---

## Usage Examples

### Example 1: Get first page of units

**Request:**
`GET /api/unit?page=1&limit=20`

**Headers:**
`Authorization: Bearer <token>`

**Example Success Response (200 OK):**
```json
{
  "data": [
    { "id": "1", "title": "Computing Technology Project B", "code": "COS40006" }
  ],
  "pagination": { "page": 1, "limit": 20, "total": 52 }
}