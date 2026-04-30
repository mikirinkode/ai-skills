---
name: mk-generate-getx-module
description: Use when scaffolding new features - generates GetX module structure (bindings, controllers, views, routes)
---

# Skill: Generate GetX Module

## Purpose

Scaffold a new GetX feature module with standard structure.

## When to Use

- Starting new feature
- Phase 1 of implementation plan
- Creating consistent module structure

## Input Requirements

- Module name (e.g., "product", "user_profile")
- Project path
- Feature type (list, form, detail, etc.)

## Steps

### 1. Create Folder Structure

```bash
mkdir -p lib/app/modules/[module_name]/{bindings,controllers,views,widgets}
```

### 2. Generate Binding

File: `lib/app/modules/[module_name]/bindings/[module_name]_binding.dart`

```dart
import 'package:get/get.dart';
import '../controllers/[module_name]_controller.dart';

class [ModuleName]Binding extends Bindings {
  @override
  void dependencies() {
    Get.lazyPut<[ModuleName]Controller>(
      () => [ModuleName]Controller(),
    );
  }
}
```

### 3. Generate Controller

File: `lib/app/modules/[module_name]/controllers/[module_name]_controller.dart`

```dart
import 'package:get/get.dart';
import 'package:coolvacore/coolvacore.dart';

class [ModuleName]Controller extends GetxController {
  // State
  final RxBool isLoading = false.obs;
  final RxString errorMessage = ''.obs;
  
  @override
  void onInit() {
    super.onInit();
    // Initialize
  }
  
  @override
  void onClose() {
    // Cleanup
    super.onClose();
  }
}
```

### 4. Generate View

File: `lib/app/modules/[module_name]/views/[module_name]_view.dart`

```dart
import 'package:flutter/material.dart';
import 'package:get/get.dart';
import 'package:coolvacore/coolvacore.dart';
import '../controllers/[module_name]_controller.dart';

class [ModuleName]View extends GetView<[ModuleName]Controller> {
  const [ModuleName]View({super.key});
  
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('[Title]', style: AppStyles.w600.s18),
      ),
      body: Obx(() {
        if (controller.isLoading.value) {
          return const Center(child: CircularProgressIndicator());
        }
        
        if (controller.errorMessage.isNotEmpty) {
          return Center(child: Text(controller.errorMessage.value));
        }
        
        return const Center(child: Text('[ModuleName] View'));
      }),
    );
  }
}
```

### 5. Add Routes

Update `lib/app/routes/app_pages.dart`:

```dart
GetPage(
  name: _Paths.[MODULE_NAME],
  page: () => const [ModuleName]View(),
  binding: [ModuleName]Binding(),
),
```

Add to `lib/app/routes/app_routes.dart`:

```dart
static const [MODULE_NAME] = _Paths.[MODULE_NAME];
```

## Output Format

Confirmation of created files:
```
Created GetX module: [module_name]

Files:
✓ bindings/[module_name]_binding.dart
✓ controllers/[module_name]_controller.dart  
✓ views/[module_name]_view.dart

Updated:
✓ app_pages.dart (route added)
✓ app_routes.dart (constant added)

Next: Implement business logic in controller
```

## Example Usage

```
Human: "Generate GetX module for product_list feature"

AI:
1. Create folder structure
2. Generate binding, controller, view
3. Add routes
4. Confirm completion

Output: "Module 'product_list' created. 5 files generated."
```

## Success Criteria

- [ ] Folder structure created
- [ ] Binding with lazyPut
- [ ] Controller with lifecycle methods
- [ ] View with GetView
- [ ] Routes configured
- [ ] All files compile
