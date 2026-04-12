# Example Outputs

## Example 1: Flutter Project (Deep Analysis)

```markdown
## Codebase Analysis: kasir-app

### Project Overview
- **Type:** Flutter
- **Detection Method:** pubspec.yaml found
- **Analysis Mode:** Deep Analysis
- **Path:** /Users/macbook/projects/kasir-app
- **Analyzed At:** 2026-04-12 14:30:00

### Architecture Pattern
- **Primary Pattern:** GetX Modular
- **Description:** Well-structured GetX modular architecture with clear separation
- **Project-Specific Details:**
  - 8 modules in lib/app/modules/
  - Route definitions in lib/routes/
  - Service layer in lib/services/

### Dependencies
- **Package Manager:** pub (Flutter 3.16.0)
- **Key Dependencies:**
  - get: ^4.6.6
  - cloud_firestore: ^4.14.0
  - firebase_auth: ^4.16.0
  - intl: ^0.18.1
- **Coolvacore Usage:** Partial
  - coolvacore: ^1.2.0 ✓
  - coolvacore_getx: ^1.1.0 ✓
  - coolvacore_firebase: Not detected ✗

### Existing Features/Modules
- **Auth:** Authentication flow with login/register @ modules/auth/
- **Dashboard:** Main dashboard with summary cards @ modules/dashboard/
- **Transactions:** CRUD for sales transactions @ modules/transactions/
- **Products:** Product management @ modules/products/
- **Reports:** Sales reports and analytics @ modules/reports/

### Key Patterns
- **State Management:** Loadable/UiState pattern via coolvacore
- **API/Data Layer:** Firestore with repository pattern
- **UI Patterns:** Mobile-first with card-based layouts
- **Testing Approach:** Unit tests present, widget tests minimal

### File Structure
```
lib/
├── app/
│   ├── modules/
│   │   ├── auth/
│   │   ├── dashboard/
│   │   ├── products/
│   │   ├── reports/
│   │   └── transactions/
│   ├── routes/
│   └── services/
├── core/
├── models/
└── main.dart
```

### Code Quality Observations
- **Documentation:** Good README, inline comments adequate
- **Consistency:** Consistent naming, follows Dart style guide
- **Dependencies:** All dependencies up to date
- **Testing:** ~15% coverage, needs improvement
- **Maintainability:** Clean architecture, low complexity

### Universal Gap Analysis
- [x] README.md exists and is comprehensive
- [x] Documentation folder/docs present
- [x] .env.example exists
- [x] .gitignore properly configured
- [x] pubspec.lock committed
- [x] No critical security vulnerabilities
- [ ] Test structure present (minimal coverage)
- [ ] CI/CD configuration (if applicable)
- [x] Consistent naming conventions
- [x] Clear project organization

### Project-Specific Gaps/Issues
- **Medium:** coolvacore_firebase not used - inconsistent Firebase pattern
- **High:** Test coverage only 15%, recommend adding widget tests
- **Low:** Some unused imports in dashboard controller

### Recommendations
- **[Priority: High]** Increase test coverage to at least 60%
- **[Priority: Medium]** Migrate Firebase calls to coolvacore_firebase for consistency
- **[Priority: Low]** Add CI/CD pipeline for automated testing
```

---

## Example 2: Firebase Functions (Quick Scan)

```markdown
## Codebase Analysis: backend-functions

### Project Overview
- **Type:** Firebase Functions
- **Detection Method:** firebase.json + functions/ directory found
- **Analysis Mode:** Quick Scan
- **Path:** /Users/macbook/projects/backend-functions
- **Analyzed At:** 2026-04-12 14:35:00

### Architecture Pattern
- **Primary Pattern:** Function-based microservices
- **Description:** HTTP and trigger-based functions organized by feature

### Dependencies
- **Package Manager:** npm (Node.js 18)
- **Key Dependencies:**
  - firebase-functions: ^4.5.0
  - firebase-admin: ^11.11.0
  - express: ^4.18.0
- **Coolvacore Usage:** Not Applicable (not a Flutter project)

### Existing Features/Modules
- **API:** Express-based REST API @ functions/src/api/
- **Auth Hooks:** User creation/deletion triggers @ functions/src/auth/
- **Notifications:** Push notification handlers @ functions/src/notifications/

### File Structure
```
functions/
├── src/
│   ├── api/
│   ├── auth/
│   └── notifications/
├── .env
├── package.json
└── index.js
```

### Code Quality Observations
- **Documentation:** Minimal README, needs API documentation
- **Consistency:** Good separation of concerns
- **Dependencies:** All current
- **Testing:** No test files detected

### Universal Gap Analysis
- [ ] README.md exists but minimal
- [ ] Documentation folder/docs present
- [x] .env.example exists
- [x] .gitignore properly configured
- [x] package-lock.json committed
- [x] No critical security vulnerabilities
- [ ] Test structure present
- [ ] CI/CD configuration
- [x] Consistent naming conventions
- [x] Clear project organization

### Recommendations
- **[Priority: High]** Add comprehensive README with API documentation
- **[Priority: High]** Add unit tests for critical functions
- **[Priority: Medium]** Set up CI/CD for automated deployment
```

---

## Example 3: React Web (Quick Scan)

```markdown
## Codebase Analysis: admin-dashboard

### Project Overview
- **Type:** React Web
- **Detection Method:** package.json with react dependency + src/ folder
- **Analysis Mode:** Quick Scan
- **Path:** /Users/macbook/projects/admin-dashboard
- **Analyzed At:** 2026-04-12 14:40:00

### Architecture Pattern
- **Primary Pattern:** Feature-based organization
- **Description:** Components organized by feature domain

### Dependencies
- **Package Manager:** npm (Node.js 20)
- **Key Dependencies:**
  - react: ^18.2.0
  - react-router-dom: ^6.20.0
  - @mui/material: ^5.14.0
  - zustand: ^4.4.0
- **Coolvacore Usage:** Not Applicable

### Existing Features/Modules
- **Dashboard:** Analytics dashboard @ src/features/dashboard/
- **Users:** User management @ src/features/users/
- **Settings:** App configuration @ src/features/settings/

### Key Patterns
- **State Management:** Zustand for global state
- **API Layer:** REST with axios
- **UI Patterns:** Material-UI components
- **Testing:** Jest configured, minimal tests

### File Structure
```
src/
├── features/
│   ├── dashboard/
│   ├── users/
│   └── settings/
├── components/
├── hooks/
└── store/
```

### Universal Gap Analysis
- [x] README.md exists
- [ ] Documentation folder/docs present
- [x] .env.example exists
- [x] .gitignore configured
- [x] package-lock.json committed
- [x] No critical security issues
- [ ] Test coverage adequate
- [ ] CI/CD not configured

### Recommendations
- **[Priority: Medium]** Add comprehensive tests
- **[Priority: Low]** Set up CI/CD pipeline
```

---

## Example 4: Generic/Unknown (Quick Scan)

```markdown
## Codebase Analysis: legacy-project

### Project Overview
- **Type:** Generic (could not auto-detect specific type)
- **Detection Method:** No characteristic files found
- **Analysis Mode:** Quick Scan
- **Path:** /Users/macbook/projects/legacy-project
- **Analyzed At:** 2026-04-12 14:45:00

### Architecture Pattern
- **Primary Pattern:** Unknown
- **Description:** Could not determine architecture from available files

### Dependencies
- **Package Manager:** Unknown
- **Key Dependencies:** None detected
- **Coolvacore Usage:** Not Applicable

### Existing Features/Modules
- Could not identify specific features

### File Structure
```
./
├── data/
├── scripts/
├── docs/
└── README.md
```

### Code Quality Observations
- **Documentation:** README present but minimal
- **Consistency:** Unclear project structure
- **Dependencies:** None detected

### Universal Gap Analysis
- [ ] README.md comprehensive
- [ ] Documentation folder structure unclear
- [ ] .env.example not found
- [x] .gitignore present
- [ ] Dependency management unclear
- [ ] Test structure not detected

### Recommendations
- **[Priority: High]** Add package.json, pubspec.yaml, or equivalent
- **[Priority: High]** Expand README with project description and setup
- **[Priority: Medium]** Establish clear project structure
```
