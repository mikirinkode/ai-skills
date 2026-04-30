---
name: mk-create-flutter-widget
description: Use when creating Flutter widgets - scaffolds reusable UI components following coolvacore standards
---

# Skill: Create Flutter Widget

## Purpose

Create reusable Flutter widgets following MikirinKode standards.

## When to Use

- Creating shared UI components
- Building feature-specific widgets
- Refactoring existing UI

## Input Requirements

- Widget purpose and functionality
- Props/parameters needed
- Design reference (if any)
- Placement (shared or module-specific)

## Steps

### 1. Define Widget Spec

Clarify:
- Widget name
- Purpose
- Input parameters
- Expected behavior
- Responsive requirements

### 2. Choose Location

Shared widgets: `lib/app/widgets/[category]/`
Module widgets: `lib/app/modules/[module]/widgets/`

### 3. Generate Widget Structure

```dart
import 'package:flutter/material.dart';
import 'package:coolvacore/coolvacore.dart';

/// [Widget description]
/// 
/// Example:
/// ```dart
/// [WidgetName](
///   [parameter]: [value],
/// )
/// ```
class [WidgetName] extends StatelessWidget {
  final [Type] [parameter];
  final VoidCallback? onTap;
  
  const [WidgetName]({
    super.key,
    required this.[parameter],
    this.onTap,
  });
  
  @override
  Widget build(BuildContext context) {
    return GestureDetector(
      onTap: onTap,
      child: Container(
        // Implementation
      ),
    );
  }
}
```

### 4. Implement Design

Use coolvacore:
- `AppColor` for colors
- `AppStyles` for typography
- `heightSpace`/`widthSpace` for spacing

### 5. Add Tests

Create `test/widgets/[widget_name]_test.dart`:
```dart
void main() {
  testWidgets('[WidgetName] displays correctly', (tester) async {
    await tester.pumpWidget(
      GetMaterialApp(
        home: Scaffold(
          body: [WidgetName](
            [parameter]: [testValue],
          ),
        ),
      ),
    );
    
    expect(find.text('[expectedText]'), findsOneWidget);
  });
}
```

### 6. Document

Add to widget index if shared:
`lib/app/widgets/[category]/index.dart`:
```dart
export '[widget_name].dart';
```

## Widget Templates

### Card Widget
```dart
class InfoCard extends StatelessWidget {
  final String title;
  final String subtitle;
  final IconData icon;
  final Color? color;
  final VoidCallback? onTap;
  
  const InfoCard({
    super.key,
    required this.title,
    required this.subtitle,
    required this.icon,
    this.color,
    this.onTap,
  });
  
  @override
  Widget build(BuildContext context) {
    return Card(
      child: ListTile(
        leading: CircleAvatar(
          backgroundColor: (color ?? AppColor.primary).withOpacity(0.1),
          child: Icon(icon, color: color ?? AppColor.primary),
        ),
        title: Text(title, style: AppStyles.w600.s16),
        subtitle: Text(subtitle, style: AppStyles.w400.s14.secondaryTextColor),
        onTap: onTap,
      ),
    );
  }
}
```

### Loading Widget
```dart
class LoadingOverlay extends StatelessWidget {
  final bool isLoading;
  final Widget child;
  final String? message;
  
  const LoadingOverlay({
    super.key,
    required this.isLoading,
    required this.child,
    this.message,
  });
  
  @override
  Widget build(BuildContext context) {
    return Stack(
      children: [
        child,
        if (isLoading)
          Container(
            color: Colors.black.withOpacity(0.3),
            child: Center(
              child: Column(
                mainAxisSize: MainAxisSize.min,
                children: [
                  CircularProgressIndicator(color: AppColor.primary),
                  if (message != null) ...[
                    heightSpace(16),
                    Text(message!, style: AppStyles.w500.s14.whiteColor),
                  ],
                ],
              ),
            ),
          ),
      ],
    );
  }
}
```

## Output Format

Confirmation:
```
Widget Created: [WidgetName]

Location: [file path]

Parameters:
- [parameter1]: [description]
- [parameter2]: [description]

Features:
✓ Responsive design
✓ coolvacore styling
✓ Accessibility support
✓ Test included

Usage:
```dart
[WidgetName](
  [parameter1]: [value],
  [parameter2]: [value],
)
```
```

## Example Usage

```
Human: "Create a product card widget that shows image, name, price, and rating"

AI:
1. Define spec with user
2. Create ProductCard widget
3. Use coolvacore colors and typography
4. Add tests
5. Confirm completion

Output: "ProductCard created at lib/app/widgets/cards/product_card.dart"
```

## Success Criteria

- [ ] Widget compiles
- [ ] Follows design system
- [ ] Responsive
- [ ] Accessible
- [ ] Documented
- [ ] Tested
