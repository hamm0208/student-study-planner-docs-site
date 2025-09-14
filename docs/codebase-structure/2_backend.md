---
sidebar_position: 2
---
# Backend
The backend is implemented within the **`src/app`** directory and consists of two main parts:

###  `src/app/api`

	-  Contains all the **RESTful API route handlers**.  
	- Each subfolder represents an endpoint that maps directly to the system’s modules.
	- Follow standard REST conventions (`GET`, `POST`, `PUT`, `DELETE`) for managing resources.

```
src/app/api/
├── units/          # Unit-related APIs (CRUD)
├── courses/        # Course and intake APIs
├── students/       # Student records, study planner, unit history
└── ...             # Other module-specific endpoints
```
---
### `src/app/class`

	- Contains **core classes** that represent the system’s entities and encapsulate business logic.  
	- Each entity (e.g., Unit, Course, User, Student) is represented as a class folder. 
	- Typically, there are two files per entity:  
	    - `<Entity>.js` → Defines the object model and its properties.  
	    - `<Entity>DB.js` → Provides methods to communicate with the **RESTful API** for CRUD operations.  
```
src/app/class/
├── Unit/
├───────Unit.js     #Unit object
├───────UnitDB.js   #UnitDB object to communicate with the RESTful API
```