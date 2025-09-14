---
sidebar_position: 3
---
# Frontend 
The frontend is implemented using **Next.js** and organized into several key directories.

---

### `src/app/view`

Contains the **page-level views** for different modules (e.g., Students, Units, Courses).

- Each subfolder maps directly to a route in the application  
```
src/app/view/
	├── unit
	├── component_1.js
	├── component_2.js
```

---

### `src/components`

Houses **reusable UI components** such as buttons and popup modals.

- Promotes modularity
    
- Ensures consistent UI across the application
    

```
src/components/
	├── button.js
	├── nav.js
	├── popup.js 
```

---

### `src/styles`

Contains **global and module-specific stylesheets**.

- Includes CSS and Tailwind customization for consistent design

```
src/styles/
	├── unit.module.css
	├── course.module.css	 
```

---

### `public/`

Stores **static assets** that are directly served by the application.

- Examples: images, icons, and other public files