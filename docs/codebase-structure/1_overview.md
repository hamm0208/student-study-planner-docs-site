---
sidebar_position: 1
---
# Overview
The codebase is organized into three main layers: **Frontend**, **Backend**, and **Database**. Each layer is separated into its own directory for clarity and maintainability.

- **[Backend](backend)**
	- Manages the business logic and communication with the database. It uses **Prisma ORM** for database interactions and provides REST APIs that the frontend consumes.
    
- **[Frontend](frontend)**
    - Handles the user interface, built with **Next.js**. It contains pages, components, and static assets that form the student-facing and admin-facing views of the system.
    
- **[Database Layer](database)**  
    - Powered by **Supabase (PostgreSQL)**. The database schema and migrations are managed via Prisma. For development, a **Dockerized PostgreSQL** instance is available.