---
sidebar_position: 1
---
#### **Overview**

Automated testing was implemented using **Jest**, a JavaScript testing framework integrated with the Node.js environment. Jest was chosen for its simplicity, ES module support, and efficient mock handling, which allowed the system to test backend logic independently from external data sources such as APIs and databases on top of it being in the required tech stack.

#### **Testing Process**

**Date Range:** 30th June – ...

The testing process began with the installation and setup of Jest in the development environment:

- Installed Jest globally using `npm install -g jest` and locally as a dev dependency.
    
- Updated the project’s `package.json` file to include `"test": "jest"` under the scripts section.
    
- Installed the VS Code extension `Orta.vscode-jest` for integrated test execution and live feedback.
    

Each class module in the system (e.g., `CourseDB.js`, `MasterStudyPlannerDB.js`, `UserProfileDB.js`) was paired with a corresponding Jest test file (e.g., `CourseDB.test.js`). 
Tests were executed via:

- npx jest               // Run all test 
- npx jest <path>  // Run specific test 
- `jest --watch`: This is the default watch mode behavior. It runs tests related to files that have changed since the last commit (based on version control like Git/Hg) or files modified during the current watch session.
- `jest --watchAll`: This runs all tests when any file changes, rather than only the related/changed files.