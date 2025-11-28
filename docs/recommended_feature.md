---
sidebar_position: 5
---

# Future Development Roadmap

This document outlines the strategic roadmap for future development, categorized by User Experience, AI Capabilities, Administration, and Technical Architecture.

---

## 1. Core Platform Experience

Enhancements focused on usability, speed, and responsiveness for the end-user.

### Enhanced Planner Interface
* **Drag-and-Drop Interaction:** Replace click-based selection with a fluid drag-and-drop interface for moving units between semesters.
* **Keyboard Shortcuts:** Implement hotkeys for power users (e.g., `Ctrl+Z` to undo, `Del` to remove unit) to speed up the planning process.

### Smart Notifications
* **Real-time Alerts:** Implement a dedicated system to notify students immediately when their study plan is updated, approved, or requires action.

---

## 2. Artificial Intelligence Suite

Leveraging AI to automate complex decision-making and support users.

### AI Course Recommendation Engine
A system to suggest relevant elective units based on a specific student's profile:
* **Input:** Completed units, academic performance (GPA), and prerequisite success rates.
* **Output:** tailored unit suggestions to maximize student success.

### AI Conflict Resolution Agent
An intelligent agent designed to solve blocking issues in study plans:
* **Context-Awareness:** Understands timetable clashes, prerequisite chains, and semester availability.
* **Sophisticated Solutions:** Proposes valid alternative paths rather than just flagging errors.

### AI Agent Helper for Administrators
A utility layer specifically for Heads of Department (HoDs) to perform bulk actions via natural language or quick actions:
* **Planner Generation:** Automatically create default study planners for specific course intakes.
* **Conflict Reporting:** Instantly list and generate reports of all students currently exhibiting study plan conflicts.

---

## 3. Administration & Security

Tools for data management, security scoping, and system oversight.

### Access Control & Data Scoping
Implement granular **Role-Based Access Control (RBAC)** to ensure data security and relevance.

* **Departmental Scoping:** Users are assigned strictly to specific departments or courses.
* **Data Visibility:** Views are automatically filtered to show *only* the students, units, and planners relevant to the user's scope, reducing cognitive load and enhancing security.

### Enhanced Dashboard & Analytics
* **Activity Tracking:** Add "last modified" timestamps for study plans and unit selections.
* **Data Analytics:** Visual insights into planning trends and efficiency.
* **Quick Actions:** Immediate access to critical tasks (e.g., "Approve Pending Plans") directly from the dashboard.

---

## 4. Technical Architecture & Modernization

These initiatives focus on **Code Health** and **Scalability** to ensure the system handles university-wide loads.

:::info [Technical Debt & Code Health]
**Migration to TypeScript (TS)**
We plan to convert the existing JavaScript codebase to TypeScript.
* **Why:** To enforce strict typing on complex objects (Student Plans, Units).
* **Benefit:** Drastic reduction in runtime errors and improved maintainability through compile-time checks.
:::

:::tip [Performance Optimization]
**Data Structure Refactor: Graph Model**
We will refactor the core Study Planner data structure from a multi-dimensional array to a **Graph** model.
* **Dependency Management:** Much faster traversal of prerequisites, co-requisites, and exclusions.
* **Pathfinding:** Enables the use of standard graph algorithms (DFS, Topological Sort) for calculating valid study paths efficiently.
:::

### Scalability Strategy
* **Database Infrastructure:** Scale the backend to process university-wide datasets without latency.
* **User Focus Shift:** Strategically pivot the primary user focus from HoDs to **Students**, empowering them with self-management tools to reduce administrative overhead.

## 5. Tech Debt
The current auto solver (SolveConflicts) is a greedy algorithm which is not optimal for finding the best study planner, potentially optimise through **Integer Programming**, or changing the `years` structure within `StudentStudyPlanner` to a graph instead of a multidimensional array and take a look into **Graph Colouring** as a algorithm.