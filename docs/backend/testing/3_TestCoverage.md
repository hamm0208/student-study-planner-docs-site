---
sidebar_position: 3
---
The Jest testing covered:

1. **Unit Testing:** Verifying the functionality of individual modules and methods.
    
2. **Integration Testing:** Ensuring correct data flow and interaction between modules (e.g., CourseDB ↔ StudyPlannerDB).
    
3. **System Evaluation:** Testing logical flows and combined module behavior.
    
4. **Acceptance Validation:** Confirming outcomes aligned with user requirements and expected workflows.
    

#### **Issues and Fixes**

Throughout testing, several configuration and logic-related issues were identified and resolved:

- **ESM/CommonJS conflict:** Renamed `babel.config.js` to `babel.config.mjs` and updated syntax.
    
- **Mocking issues:** Adjusted mocking order and moduleNameMapper syntax.
    
- **Assertion errors:** Updated equality checks to deep comparisons for nested objects.
    
- **Logic bugs:** Fixed non-boolean return values in `Permission.js` methods using double negation (`!!`).
    
- **Dependency corruption:** Reinstalled `node_modules` after regenerating `package-lock.json` to restore missing dependencies.
    