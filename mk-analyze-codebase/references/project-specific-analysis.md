# Project-Specific Analysis Guidelines

## Flutter Projects

### Architecture Detection
- **GetX Modular:** Check `lib/app/modules/`, `lib/routes/`
- **BLoC:** Check `lib/bloc/`, `lib/cubit/`
- **Provider/Riverpod:** Check `lib/providers/`
- **Vanilla:** No clear state management folder

### Coolvacore Analysis Template
```
Coolvacore Status: [Not Used / Partial / Full]
Packages Detected:
- coolvacore: [version] ✓/✗
- coolvacore_getx: [version] ✓/✗
- coolvacore_firebase: [version] ✓/✗
- coolvacore_ui: [version] ✓/✗
```

### Firebase Integration Check
- Authentication: `firebase_auth` dependency
- Firestore: `cloud_firestore` dependency
- Storage: `firebase_storage` dependency
- Functions: `cloud_functions` dependency
- Messaging: `firebase_messaging` dependency

### State Management Pattern
- Loadable/UiState pattern (coolvacore)
- Simple Rx (observables)
- BLoC pattern
- Other/Unclear

---

## Native Android Projects

### Architecture Detection
- **MVVM:** ViewModel usage, data binding
- **MVP:** Presenter classes
- **MVI:** State classes, Intent/Action patterns
- **MVC:** Traditional activity-centric

### Language Distribution Template
```
Language Distribution:
- Kotlin: X files (X%)
- Java: X files (X%)
- Primary: [Kotlin/Java]
```

### Module Structure
- Single module (`app/` only)
- Multi-module (feature modules present)
- Library modules

### Firebase SDK Check
- Check `build.gradle` for Firebase dependencies
- List: Auth, Firestore, Storage, Analytics, Crashlytics

---

## Firebase Functions Projects

### Functions Inventory Template
```
Functions Detected:
- [functionName]: [trigger type] - [description]
  * HTTP endpoint: [path]
  * Firestore trigger: [collection] on [create/update/delete]
  * Auth trigger: [onCreate/onDelete]
  * Scheduled: [schedule expression]
```

### Environment & Config
- `.env` files present
- Functions config values (`functions.config()`)
- Environment variables in `firebase.json`

### Security Rules
- Firestore rules complexity
- Storage rules presence
- Rules testing setup

### Key Dependencies
- `firebase-admin` version
- `firebase-functions` version
- Third-party packages

---

## React Web Projects

### Framework/Tool Detection
- Create React App
- Next.js
- Vite
- Custom Webpack

### Architecture Patterns
- Feature-based folders
- Atomic design
- MVC-like structure
- Flat component organization

### State Management Detection
- Redux (store, actions, reducers)
- Context API
- Zustand
- Jotai/Recoil
- None/Local state only

### Routing Check
- React Router
- Next.js routing
- No routing (single page)

### UI Libraries
- Material-UI (MUI)
- Ant Design
- Tailwind CSS
- Bootstrap
- Custom components

---

## Static Web Projects

### Technology Stack
- Vanilla JavaScript
- jQuery
- Alpine.js
- Other lightweight frameworks

### CSS Approach
- Plain CSS
- SCSS/Sass
- Tailwind CDN
- Bootstrap CDN
- Custom CSS framework

### Asset Organization
- CSS/JS in separate folders
- Inline styles
- CDN dependencies
