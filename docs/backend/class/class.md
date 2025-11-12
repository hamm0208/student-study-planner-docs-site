---
sidebar_position: 1
---

# General Class Structure 

This document provides a comprehensive guide to understanding how classes are structured, what they contain, and how they are implemented in the Student Study Planner System.

---

## Table of Contents

1. [Class Architecture Overview](#class-architecture-overview)
2. [Class Components](#class-components)
3. [Getter and Setter Pattern](#getter-and-setter-pattern)
4. [Class Types](#class-types)
5. [Class Naming Conventions](#class-naming-conventions)
6. [Common Patterns](#common-patterns)
7. [Class Directory Structure](#class-directory-structure)
8. [Example Class Walkthrough](#example-class-walkthrough)

---

## Class Architecture Overview

Classes in the Student Study Planner System are organized within the `src/app/class/` directory. Each class represents a domain entity within the application, such as:

- `Course` - Academic course information
- `Unit` - Academic units/subjects within courses
- `Student` - Student information and related data
- `Role` - User role definitions
- `Permission` - Permission definitions for access control
- `Term` - Academic terms/semesters
- `StudyPlanner` - Complex study plan management system

### Purpose of Classes

Classes serve as:
- **Data Models**: Represent entities in the system
- **Data Containers**: Store and organize related properties
- **Business Logic Holders**: Contain methods for data manipulation
- **Type Safety**: Provide structure for JavaScript objects

---

## Class Components

### 1. **Constructor**

The constructor initializes an instance of the class with provided parameters.

```javascript
constructor({ id = null, name, description, status = 'active' }) {
    this.id = id;
    this.name = name;
    this.description = description;
    this.status = status;
}
```

**Key Features:**
- Uses destructuring for parameter objects
- Provides default values for optional parameters
- Initializes properties with underscore-prefixed private variables

### 2. **Private Properties**

Properties are stored with underscore prefix (`_`) to indicate they are private and should be accessed through getters/setters.

```javascript
this._id = id;
this._name = name;
this._description = description;
this._status = status;
```

### 3. **Getters**

Getters provide controlled access to private properties.

```javascript
get id() {
    return this._id;
}

get name() {
    return this._name;
}
```

**Benefits:**
- Encapsulation: Control how properties are accessed
- Validation: Can add validation logic if needed
- Consistency: Uniform interface for all properties

### 4. **Setters**

Setters provide controlled modification of private properties.

```javascript
set id(value) {
    this._id = value;
}

set name(value) {
    this._name = value;
}
```

**Benefits:**
- Encapsulation: Control how properties are modified
- Validation: Can add validation logic before assignment
- Consistency: Uniform interface for all modifications

### 5. **Validation Methods** (Optional)

Some classes include validation methods to ensure data integrity.

```javascript
validate() {
    const errors = [];
    
    if (!this.name || this.name.trim() === '') {
        errors.push('Name is required');
    }
    
    if (this.name && this.name.length > 100) {
        errors.push('Name must be less than 100 characters');
    }
    
    return errors;
}
```

### 6. **Helper Methods** (Optional)

Classes may include utility methods for common operations.

```javascript
// Conversion methods
toJSON() {
    return {
        id: this.id,
        name: this.name,
        description: this.description
    };
}

static fromJSON(json) {
    return new ClassName(json);
}

// Comparison methods
equals(other) {
    return this.id === other.id && this.name === other.name;
}

// Clone method
clone(modifications = {}) {
    return new ClassName({
        ...this.toJSON(),
        ...modifications,
        id: null
    });
}
```

---

## Getter and Setter Pattern

The getter/setter pattern is consistently used throughout the codebase for property access.

### Standard Pattern

```javascript
class ExampleClass {
    constructor({ propertyName = null }) {
        this.propertyName = propertyName;
    }

    // Getter
    get propertyName() {
        return this._propertyName;
    }

    // Setter
    set propertyName(value) {
        this._propertyName = value;
    }
}
```

### Usage

```javascript
const example = new ExampleClass({ propertyName: 'value' });

// Access via getter
console.log(example.propertyName); // 'value'

// Modify via setter
example.propertyName = 'new value';
```

### Benefits

- **Encapsulation**: Hide internal implementation
- **Validation**: Add validation logic when setting values
- **Consistency**: Uniform property access syntax
- **Future-proofing**: Change implementation without affecting external code

---

## Class Types

### 1. **Entity Classes**

Entity classes represent database entities and domain models.

**Examples:** `Course`, `Unit`, `Student`, `Role`, `Permission`, `Term`

**Characteristics:**
- Typically have an ID property
- May have timestamps (CreatedAt, UpdatedAt)
- Include validation methods
- Represent single entities

**Example:**

```javascript
class Course {
    constructor({ id = null, code, name, credits_required, status }) {
        this.id = id;
        this.code = code;
        this.name = name;
        this.credits_required = credits_required;
        this.status = status;
    }

    // Getters and setters for each property
    get id() { return this._id; }
    set id(value) { this._id = value; }
    
    // ... more getters and setters
}
```

### 2. **Database Classes** (DB Classes)

Database classes handle API calls and database operations for their corresponding entity classes.

**Naming Convention:** `{EntityName}DB` (e.g., `CourseDB`, `UnitDB`, `StudentDB`)

**Characteristics:**
- Static methods for API communication
- Methods for CRUD operations (Create, Read, Update, Delete)
- Return structured responses with `{ success, message, data }`
- Handle error management

**Common Methods:**

```javascript
static async Fetch{Entities}(params) {
    // Fetch list of entities with optional filtering
}

static async Save{Entity}(entityData, method_type) {
    // Save (create or update) an entity
}

static async Delete{Entity}(id) {
    // Delete an entity
}
```

**Example Response Structure:**

```javascript
{
    success: true,        // Operation successful
    message: "...",       // Human-readable message
    data: [],            // Array of entity instances
    filtered: false      // Whether results were filtered
}
```

### 3. **Complex Logic Classes**

Classes that handle complex business logic beyond simple data storage.

**Examples:** `StudyPlanner`, `AuditLogger`, `StudentStudyPlanner`

**Characteristics:**
- Multiple interconnected properties
- Complex methods with business logic
- State management
- May contain nested objects
- Often include async operations

---

## Class Naming Conventions

The codebase follows these naming conventions:

### Entity Classes
- **PascalCase** for class names
- Singular form for entities
- Examples: `Course`, `Unit`, `Student`, `Term`, `Role`

### Database Classes
- Format: `{EntityName}DB`
- Examples: `CourseDB`, `UnitDB`, `StudentDB`
- Located in same directory as entity class

### Properties
- **camelCase** for JavaScript properties
- **PascalCase** for database column names (in API responses)
- **snake_case** for database columns

### Private Properties
- Prefix with underscore: `_propertyName`
- Accessed through getters/setters

### Methods
- **camelCase** for standard methods
- **PascalCase** for static factory methods: `FromJSON`, `Create`, `Fetch`

---

## Common Patterns

### 1. **Constructor with Defaults**

```javascript
constructor({ id = null, name = '', status = 'active' }) {
    this.id = id;
    this.name = name;
    this.status = status;
}
```

### 2. **Getter/Setter for All Properties**

Every property has a corresponding getter and setter:

```javascript
get propertyName() {
    return this._propertyName;
}

set propertyName(value) {
    this._propertyName = value;
}
```

### 3. **Validation Pattern**

```javascript
validate() {
    const errors = [];
    
    // Validation logic
    if (!this.name) {
        errors.push('Name is required');
    }
    
    return errors;
}
```

### 4. **Serialization Pattern**

```javascript
toJSON() {
    return {
        id: this.id,
        name: this.name,
        // ...all properties
    };
}

static fromJSON(json) {
    return new ClassName(json);
}
```

### 5. **Clone Pattern**

```javascript
clone(modifications = {}) {
    return new ClassName({
        ...this.toJSON(),
        ...modifications,
        id: null  // Reset ID for cloned instances
    });
}
```

### 6. **Comparison Pattern**

```javascript
equals(other) {
    if (!other || !(other instanceof ClassName)) {
        return false;
    }
    
    return this.id === other.id && 
           this.name === other.name;
}
```

---

## Class Directory Structure

```
src/app/class/
├── Audit/
│   └── AuditLogger.js              # Audit logging functionality
├── Course/
│   ├── Course.js                   # Course entity
│   └── CourseDB.js                 # Course database operations
├── CourseIntake/
│   ├── CourseIntake.js             # Course intake entity
│   └── CourseIntakeDB.js           # Course intake database operations
├── Major/
│   ├── Major.js                    # Major entity
│   └── MajorDB.js                  # Major database operations
├── MasterStudyPlanner/
│   ├── MasterStudyPlanner.js       # Master study planner entity
│   └── MasterStudyPlannerDB.js     # Master study planner database operations
├── Permission/
│   ├── Permission.js               # Permission entity
│   └── PermissionDB.js             # Permission database operations
├── Role/
│   ├── Role.js                     # Role entity
│   └── RoleDB.js                   # Role database operations
├── Student/
│   ├── Student.js                  # Student entity
│   └── StudentDB.js                # Student database operations
├── StudyPlanner/
│   ├── StudyPlanner.js             # Study planner complex logic
│   └── StudentStudyPlanner.js      # Student-specific study planner
├── Term/
│   ├── Term.js                     # Term entity
│   └── TermDB.js                   # Term database operations
├── Unit/
│   ├── Unit.js                     # Unit entity
│   └── UnitDB.js                   # Unit database operations
└── [Other entity folders...]
```

### Directory Organization

Each entity typically has:
1. **Entity File** (e.g., `Course.js`): Class definition with properties and methods
2. **Database File** (e.g., `CourseDB.js`): Static methods for API/database operations

---

## Example Class Walkthrough

### Example 1: Simple Entity Class

```javascript
// File: src/app/class/UnitType/UnitType.js

class UnitType {
    constructor({ id = null, name, colour = '#000000' }) {
        this.id = id;
        this.name = name;
        this.colour = colour;
    }

    // Getters and Setters
    get id() {
        return this._id;
    }
    set id(value) {
        this._id = value;
    }

    get name() {
        return this._name;
    }
    set name(value) {
        this._name = value;
    }

    get colour() {
        return this._colour;
    }
    set colour(value) {
        this._colour = value;
    }
}

export default UnitType;
```

**Usage:**

```javascript
import UnitType from './UnitType';

// Create new instance
const unitType = new UnitType({
    id: 1,
    name: 'Core Unit',
    colour: '#FF5733'
});

// Access properties via getters
console.log(unitType.name);      // 'Core Unit'
console.log(unitType.colour);    // '#FF5733'

// Modify properties via setters
unitType.name = 'Elective Unit';
console.log(unitType.name);      // 'Elective Unit'
```

---

### Example 2: Entity Class with Validation

```javascript
// File: src/app/class/Permission/Permission.js

export default class Permission {
    constructor(data = {}) {
        this.ID = data.ID || null;
        this.Name = data.Name || '';
        this.Description = data.Description || '';
        this.Resource = data.Resource || '';
        this.Action = data.Action || '';
        this.Module = data.Module || '';
        this.IsActive = data.IsActive !== undefined ? data.IsActive : true;
    }

    validate() {
        const errors = [];

        if (!this.Name || this.Name.trim() === '') {
            errors.push('Permission name is required');
        }

        if (!this.Resource || this.Resource.trim() === '') {
            errors.push('Resource is required');
        }

        if (this.Name && this.Name.length > 100) {
            errors.push('Permission name must be less than 100 characters');
        }

        return errors;
    }

    toJSON() {
        return {
            ID: this.ID,
            Name: this.Name,
            Description: this.Description,
            Resource: this.Resource,
            Action: this.Action,
            Module: this.Module,
            IsActive: this.IsActive
        };
    }

    static fromJSON(json) {
        return new Permission(json);
    }

    equals(other) {
        if (!other || !(other instanceof Permission)) return false;

        return this.Resource === other.Resource &&
            this.Action === other.Action &&
            this.Module === other.Module;
    }
}
```

**Usage:**

```javascript
import Permission from './Permission';

// Create instance
const permission = new Permission({
    Name: 'Create Course',
    Description: 'Allow creating new courses',
    Resource: 'course',
    Action: 'create',
    Module: 'course_management',
    IsActive: true
});

// Validate
const errors = permission.validate();
if (errors.length > 0) {
    console.error('Validation errors:', errors);
}

// Convert to JSON
const json = permission.toJSON();
console.log(json);

// Create from JSON
const clonedPermission = Permission.fromJSON(json);
```

---

### Example 3: Database Class

```javascript
// File: src/app/class/Course/CourseDB.js

import Course from './Course';

export default class CourseDB {
    static async FetchCourses(params) {
        try {
            const queryParams = {
                ...params,
                ...(params.order_by ? { order_by: JSON.stringify(params.order_by) } : {}),
                ...(params.return ? { return: params.return.join(',') } : {})
            };

            const query = new URLSearchParams(queryParams).toString();
            const response = await fetch(
                `${process.env.NEXT_PUBLIC_SERVER_URL}/api/course?${query}`
            );

            if (!response.ok) {
                const error = await response.json();
                return {
                    success: false,
                    message: error.message || 'Failed to fetch data',
                    data: []
                };
            }

            const data = await response.json();
            const arr = Array.isArray(data) ? data : (Array.isArray(data.data) ? data.data : []);

            if (!arr || arr.length === 0) {
                return {
                    success: false,
                    message: 'No courses found',
                    data: []
                };
            }

            const courses = arr.map(course => new Course({
                id: course.ID,
                code: course.Code,
                name: course.Name,
                credits_required: course.CreditsRequired,
                status: course.Status
            }));

            return {
                success: true,
                data: courses
            };
        } catch (err) {
            console.error('FetchCourses error:', err);
            return {
                success: false,
                message: err.message || 'Failed to fetch courses',
                data: []
            };
        }
    }

    static async SaveCourse(courseData, method_type) {
        try {
            const response = await fetch(
                `${process.env.NEXT_PUBLIC_SERVER_URL}/api/course`,
                {
                    method: method_type,
                    headers: {
                        'Content-Type': 'application/json',
                    },
                    body: JSON.stringify({
                        Code: courseData.code,
                        Name: courseData.name,
                        CreditsRequired: courseData.credits_required,
                        Status: courseData.status || 'Draft',
                        ...(method_type === 'PUT' && { id: courseData.id })
                    }),
                }
            );

            const responseData = await response.json();

            if (!response.ok) {
                return {
                    error: true,
                    ...responseData,
                    status: response.status
                };
            }

            return responseData;
        } catch (error) {
            console.error('SaveCourse error:', error);
            throw error;
        }
    }

    static async deleteCourse(courseCode) {
        try {
            const response = await fetch(
                `${process.env.NEXT_PUBLIC_SERVER_URL}/api/course`,
                {
                    method: 'DELETE',
                    headers: {
                        'Content-Type': 'application/json',
                    },
                    body: JSON.stringify({ code: courseCode }),
                }
            );

            if (!response.ok) {
                const errorData = await response.json();
                throw new Error(errorData.message || 'Failed to delete course');
            }

            return await response.json();
        } catch (error) {
            console.error('DeleteCourse error:', error);
            throw error;
        }
    }
}
```

**Usage:**

```javascript
import CourseDB from './CourseDB';

// Fetch courses
const result = await CourseDB.FetchCourses({ 
    order_by: { name: 'asc' } 
});

if (result.success) {
    console.log(result.data); // Array of Course instances
}

// Save (create) new course
const saveResult = await CourseDB.SaveCourse({
    code: 'CS101',
    name: 'Intro to CS',
    credits_required: 120,
    status: 'Active'
}, 'POST');

// Update existing course
const updateResult = await CourseDB.SaveCourse({
    id: 1,
    code: 'CS101',
    name: 'Introduction to Computer Science',
    credits_required: 120,
    status: 'Active'
}, 'PUT');

// Delete course
await CourseDB.deleteCourse('CS101');
```

---

## Best Practices

### 1. **Always Include Defaults**

```javascript
constructor({ id = null, name = '', status = 'active' }) {
    // ...
}
```

### 2. **Use Private Properties with Getters/Setters**

```javascript
this.propertyName = value;  // Constructor sets via setter
get propertyName() { return this._propertyName; }
set propertyName(value) { this._propertyName = value; }
```

### 3. **Implement Validation**

Validate data integrity before storing:

```javascript
validate() {
    const errors = [];
    if (!this.name) errors.push('Name is required');
    return errors;
}
```

### 4. **Provide Serialization Methods**

```javascript
toJSON() { /* ... */ }
static fromJSON(json) { /* ... */ }
```

### 5. **Use Static Methods in DB Classes**

Database operations should be static for easy access:

```javascript
static async FetchData(params) { /* ... */ }
static async SaveData(data) { /* ... */ }
```

### 6. **Consistent Error Handling**

```javascript
return {
    success: boolean,
    message: string,
    data: any
};
```

---

## Summary

Classes in the Student Study Planner System follow a consistent pattern:

1. **Constructor** - Initialize with destructured parameters and defaults
2. **Private Properties** - Store with underscore prefix
3. **Getters/Setters** - Access properties through getter/setter pair
4. **Methods** - Implement validation, serialization, and comparison
5. **Database Classes** - Static async methods for API operations
6. **Response Format** - Consistent `{ success, message, data }` structure

This architecture ensures:
- ✅ Code consistency and maintainability
- ✅ Data encapsulation and validation
- ✅ Clear separation of concerns
- ✅ Easy testing and debugging
- ✅ Type-safe operations
