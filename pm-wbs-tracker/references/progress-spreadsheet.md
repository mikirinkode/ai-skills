# Progress Spreadsheet Formatting Reference

## Table of Contents
1. Sheet 1: Progress by Feature
2. Sheet 2: Progress by Epic
3. Sheet 3: Overall Summary
4. Color System
5. Conditional Formatting Rules

---

## 1. Sheet 1: Progress by Feature

### Columns

| Col | Header | Width | Format |
|---|---|---|---|
| A | Epic | 22 | Text |
| B | Feature | 35 | Text |
| C | Total Tasks | 12 | Number |
| D | Completed | 12 | Number |
| E | Remaining | 12 | Number |
| F | % Complete | 12 | Percentage (0%) |
| G | Est Days (Total) | 15 | Number (0.0) |
| H | Est Days (Done) | 15 | Number (0.0) |
| I | Est Days (Remaining) | 18 | Number (0.0) |
| J | Status | 16 | Text with color |

### Row Grouping

Group rows by Epic. Insert an **Epic header row** before each group:
- Merge columns A-J
- Text: Epic name
- Style: bold, blue background (E0E7FF), dark blue text (1E40AF)

Within each Epic group, sort Features by % Complete descending (most complete first).

### Progress Bar in % Complete Column

Use a formula-driven visual. In column F, set the number format to `0%` and apply conditional fill:

```python
# Conditional fill based on percentage
if pct == 1.0:
    fill = PatternFill("solid", fgColor="D1FAE5")  # green bg
    font_color = "065F46"  # dark green
elif pct >= 0.5:
    fill = PatternFill("solid", fgColor="FEF3C7")  # amber bg
    font_color = "92400E"  # dark amber
elif pct > 0:
    fill = PatternFill("solid", fgColor="FEE2E2")  # red bg
    font_color = "991B1B"  # dark red
else:
    fill = PatternFill("solid", fgColor="F3F4F6")  # gray bg
    font_color = "6B7280"  # gray
```

### Status Column Colors

| Status | Fill | Text Color |
|---|---|---|
| ✅ Complete | D1FAE5 (light green) | 065F46 (dark green) |
| 🔄 In Progress | FEF3C7 (light amber) | 92400E (dark amber) |
| 📋 Not Started | F3F4F6 (light gray) | 6B7280 (gray) |
| ⏸️ Backlog Only | F3F4F6 (light gray) | 6B7280 (gray) |

---

## 2. Sheet 2: Progress by Epic

### Columns

| Col | Header | Width | Format |
|---|---|---|---|
| A | Epic | 25 | Text |
| B | Features | 12 | Number (count of features) |
| C | Total Tasks | 12 | Number |
| D | Completed | 12 | Number |
| E | Remaining | 12 | Number |
| F | % Complete | 12 | Percentage (0%) |
| G | Est Days (Total) | 15 | Number (0.0) |
| H | Est Days (Remaining) | 18 | Number (0.0) |
| I | Status | 16 | Text with color |

Sort by % Complete descending. Apply same conditional formatting as Sheet 1.

---

## 3. Sheet 3: Overall Summary

### Layout

This is a dashboard-style sheet, not a table. Use label-value pairs:

```
Row 1:  [bold] PROJECT PROGRESS SUMMARY
Row 2:  [muted] Generated: 25 Maret 2026
Row 3:  [empty]
Row 4:  [section] OVERALL
Row 5:  Total Tasks          | 213
Row 6:  Completed            | 10
Row 7:  In Progress          | 3
Row 8:  Remaining (To Do)    | 123
Row 9:  Backlog              | 77
Row 10: Overall Completion   | 4.7%     [conditional fill]
Row 11: [empty]
Row 12: [section] BY PHASE
Row 13: MVP Total            | 136
Row 14: MVP Completed        | 10
Row 15: MVP % Complete       | 7.4%     [conditional fill]
Row 16: Phase 2 Total        | 77
Row 17: Phase 2 Completed    | 0
Row 18: Phase 2 % Complete   | 0%       [conditional fill]
Row 19: [empty]
Row 20: [section] TOP PROGRESS (Features with highest completion)
Row 21: [feature name]       | [pct]
Row 22: [feature name]       | [pct]
Row 23: [feature name]       | [pct]
Row 24: [empty]
Row 25: [section] NEEDS ATTENTION (Features with In Progress but low completion)
Row 26: [feature name]       | [pct]    | [note]
```

### Summary Formatting

| Element | Font | Size | Color |
|---|---|---|---|
| Title | Arial Bold | 16pt (32) | 1F2937 |
| Date | Arial | 11pt (22) | 6B7280 |
| Section header | Arial Bold | 12pt (24) | 1E40AF |
| Label | Arial | 11pt (22) | 374151 |
| Value | Arial Bold | 11pt (22) | 1F2937 |
| Percentage | Arial Bold | 11pt (22) | conditional |

Column widths:
- A: 30 (labels)
- B: 15 (values)
- C: 30 (notes, only used in "Needs Attention")

---

## 4. Color System

Consistent across all sheets:

```python
COLORS = {
    # Header
    "header_fill": "2563EB",
    "header_text": "FFFFFF",

    # Epic separator
    "epic_fill": "E0E7FF",
    "epic_text": "1E40AF",

    # Progress: complete
    "complete_fill": "D1FAE5",
    "complete_text": "065F46",

    # Progress: partial (≥50%)
    "partial_fill": "FEF3C7",
    "partial_text": "92400E",

    # Progress: low (<50%)
    "low_fill": "FEE2E2",
    "low_text": "991B1B",

    # Progress: not started
    "notstarted_fill": "F3F4F6",
    "notstarted_text": "6B7280",

    # Borders
    "border": "E5E7EB",

    # Text
    "primary": "1F2937",
    "body": "374151",
    "muted": "6B7280",
    "section": "1E40AF",
}
```

---

## 5. Conditional Formatting Rules

Apply these to % Complete columns on all sheets:

| Condition | Fill | Text |
|---|---|---|
| value = 100% | D1FAE5 | 065F46 |
| value ≥ 50% | FEF3C7 | 92400E |
| value > 0% | FEE2E2 | 991B1B |
| value = 0% | F3F4F6 | 6B7280 |

### General Styles

```python
from openpyxl.styles import Font, PatternFill, Alignment, Border, Side

header_font = Font(name="Arial", size=11, bold=True, color="FFFFFF")
header_fill = PatternFill("solid", fgColor="2563EB")
header_align = Alignment(horizontal="center", vertical="center", wrap_text=True)

body_font = Font(name="Arial", size=10, color="374151")
body_align = Alignment(vertical="center", wrap_text=True)

thin_border = Border(
    left=Side(style="thin", color="E5E7EB"),
    right=Side(style="thin", color="E5E7EB"),
    top=Side(style="thin", color="E5E7EB"),
    bottom=Side(style="thin", color="E5E7EB"),
)
```

### Freeze & Filter

- Sheet 1 & 2: freeze row 1, auto-filter on all columns
- Sheet 3: no freeze/filter (dashboard layout)
