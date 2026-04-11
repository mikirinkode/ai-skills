---
name: mk-setup-firebase-project
description: Use when initializing Firebase - configures Firebase services (Core, Firestore, Functions, Storage, Analytics, Hosting) for Flutter
---

# Skill: Setup Firebase Project

## Purpose

Initialize Firebase services for a new Flutter project.

## When to Use

- New project setup
- Adding Firebase to existing project
- Configuring new Firebase services

## Steps

### 1. Check Current State

Verify existing Firebase configuration:
- Check `pubspec.yaml` for Firebase dependencies
- Check if `firebase_options.dart` exists
- Check `android/app/google-services.json`
- Check `ios/Runner/GoogleService-Info.plist`

### 2. Install Dependencies

Add to `pubspec.yaml`:
```yaml
dependencies:
  firebase_core: ^3.0.0
  cloud_firestore: ^6.0.0
  cloud_functions: ^6.0.0
  firebase_storage: ^13.0.0
  firebase_analytics: ^11.0.0
```

### 3. Configure Firebase CLI

Commands to run:
```bash
# Install Firebase CLI
curl -sL https://firebase.tools | bash

# Login
firebase login

# Install FlutterFire
dart pub global activate flutterfire_cli
```

### 4. Initialize Firebase

```bash
flutterfire configure
```

Select:
- Firebase project
- Platforms (Android, iOS, Web)

### 5. Update main.dart

```dart
import 'package:firebase_core/firebase_core.dart';
import 'package:firebase_analytics/firebase_analytics.dart';
import 'firebase_options.dart';

void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  
  await Firebase.initializeApp(
    options: DefaultFirebaseOptions.currentPlatform,
  );
  
  await FirebaseAnalytics.instance.setAnalyticsCollectionEnabled(true);
  
  runApp(const MyApp());
}
```

### 6. Setup Hosting (for web)

```bash
firebase init hosting
```

Configure:
- Public directory: `build/web`
- SPA: Yes

### 7. Configure Firestore

Create security rules:
```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if request.auth != null;
    }
  }
}
```

### 8. Add Analytics Events

Create `lib/app/services/analytics_service.dart`:
```dart
import 'package:firebase_analytics/firebase_analytics.dart';

class AnalyticsService {
  static final FirebaseAnalytics _analytics = FirebaseAnalytics.instance;
  
  static Future<void> logScreenView(String screenName) async {
    await _analytics.logScreenView(screenName: screenName);
  }
  
  static Future<void> logEvent(String name, Map<String, dynamic> params) async {
    await _analytics.logEvent(name: name, parameters: params);
  }
}
```

## Output Format

Confirmation of setup:
```
Firebase Setup Complete ✅

Configured:
✓ Firebase Core
✓ Firestore
✓ Cloud Functions
✓ Storage
✓ Analytics
✓ Hosting (web)

Files created:
- firebase_options.dart
- firebase.json
- .firebaserc
- android/app/google-services.json
- ios/Runner/GoogleService-Info.plist

Next steps:
1. Deploy security rules
2. Test Firebase initialization
3. Add analytics events to features
```

## Example Usage

```
Human: "Setup Firebase for my new koperasi project"

AI:
1. Check current pubspec.yaml
2. Add Firebase dependencies
3. Run flutterfire configure
4. Update main.dart
5. Create analytics service

Output: "Firebase setup complete. Run 'flutter run' to test."
```

## Success Criteria

- [ ] Dependencies added
- [ ] Firebase configured for all platforms
- [ ] Initialization code in main.dart
- [ ] Hosting configured (if web)
- [ ] Analytics service created
- [ ] App compiles and runs
