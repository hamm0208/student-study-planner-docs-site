---
sidebar_position: 5
---
# Key Features
The Student Study Planner System is a centralized, web-based platform designed to streamline academic planning, primarily for Heads of Department (HODs) and Course Coordinators, while providing students with easy access to their personal study plans.

1. **Core Functions & Student Planning**
**1.1 Centralized Information Hub**
The system serves as the single source of truth for all essential academic data, replacing disparate spreadsheets and email chains.
- **Purpose:** To provide a central, easily searchable platform for managing Units, Terms, Courses, Course Planners, Students, and their Personal Study Planners.
- **Benefits:** Reduces information scatter and ensures version control, improving data reliability and administrative efficiency.
- **Accessibility:** Various pages are accessible based on the user's Role-Based Access Control (RBAC) permissions (e.g., Unit Management, Course Management, Term Management).

**1.2 Automated Conflict Solver (Auto-Solver)**
A primary feature designed to simplify the complex task of resolving unit scheduling conflicts in student planners.
- **Functionality:** The Auto-Solver automatically detects and suggests resolutions for conflicts within a student's personal study planner.
- **Target User:** Heads of Department and Course Coordinators.
- **Outcome:** Contributes to a smoother and more efficient decision-making process for HoDs and course coordinators.


**1.3 Amendment History Tracking**
The system maintains a comprehensive, auditable log of all changes made to study planners, addressing the issue of tracking amendments that were previously only logged via emails.
- **Feature:** Tracks and displays the history of amendments for a student's study plan.
- **Target User:** Administrators, HoDs, and Course Coordinators for oversight.

2. **Administrative Tools & Efficiency**
**2.1 Bulk Import Functionality (Data Seeding)**
This feature allows high-privilege users to rapidly populate the system with large datasets, dramatically reducing the time-consuming and repetitive manual creation process.
- **Supported Data:** Units, Courses, Terms, and Student Unit History.
- **File Formats:** Accepts standard spreadsheet formats: .csv, .xlsx, and .xls.
- **Rapid Setup:** Includes the ability to import existing planners from previous intakes for quicker setup of new cohorts.
- **Security Note (Admin/Developer):** The system enforces strict validation on file formats and sizes during upload to mitigate memory exhaustion and array-based bypasses.

**2.2 Personalized Email Function**
Users can send customized emails to students directly from their personal study planner page.
- **Use Case:** Sending the finalized study planner or updates directly to the student.
- **Future Enhancement:** Feedback suggested adding a section for the user to customize the message and enabling the ability to send to multiple recipients (cc).

**2.3 User Management and Audit Logs**
The system provides tools for granular control over user access and tracks critical activity within the system.
- **Role-Based Access Control (RBAC):** Access to features is customizable and defined by roles (Super Admin, HOD, Course Coordinator, Student). For example, HOD access is often limited to programs under their care.
- **Audit Logs:** The Super Admin can track all user changes across the system, including major data changes like course and unit uploads, which is essential for system monitoring.
- **Deletion Restriction (Admin Note)**: Hard deletion of a user account is not allowed. Instead, the system changes the user's status to "Inactive" to preserve the integrity of the audit logs.

3. **System Architecture & Technical Features**
**3.1 Web-Based Documentation**
Instead of static PDF or Word documents, the project handover is facilitated through a dynamic, easily navigable web-based documentation site built with Docusaurus.
- **Justification:** Highly readable, easily searchable, can be updated and hosted live alongside the project, and provides a better user experience.
- **Structure:** Organized into three practical guides for different audiences:
- **User Manual:** For the Client and End-Users (non-technical instruction).
- **Admin Guide:** For Superadmin and Client (high-level administrative functions).
- **Developer Handover Guide:** For the IT Department team or future student teams (technical architecture, setup, and development details).

**3.2 Performance Enhancements: Local Caching (IndexedDB)**
To combat initial feedback on slow database connection/loading times, a local caching mechanism was implemented on the frontend.
- **Mechanism:** Uses IndexedDB on the user's browser to store frequently accessed data.
- **Impact:** Significantly improves system responsiveness by reducing the need for redundant server requests.

**3.3 Comprehensive Security Hardening**
A final security review and subsequent patches were implemented across the full stack to ensure a robust system for handover.
- **API Security:** Finalized using JWT tokens for session management and securing endpoints. This ensures the system rejects modified tokens and checks user privileges with the backend.
- **Vulnerability Scanning:** Continuous passive and active security testing using tools like Snyk, Postman, and OWASP ZAP to identify and patch vulnerabilities (misimplementation, vulnerable dependencies).
- **Rate Limiting:** Added internal rate limiting to prevent excessive resource consumption in scenarios like large-scale data imports, particularly protecting against Denial-of-Service (DoS) attacks.

**3.4 Technology Stack (Developer Reference)**
The project is built using a modern, efficient, and scalable technology stack:
- **Frontend/Backend Framework:** Next.js.
- **Database ORM:** Prisma ORM.
- **Database:** Supabase.
- **Styling:** Tailwind CSS.
- **Hosting:** Vercel is the primary hosting solution, utilizing its features like CDN caching and varied domain servers.