# Spreadsheet Schema Reference

## Table of Contents
1. Column Definitions
2. Spreadsheet Formatting
3. Sheet Structure
4. Sample Rows
5. Coverage Summary Sheet

---

## 1. Column Definitions

The test case spreadsheet has these columns in order:

| Column | Header | Width | Content |
|---|---|---|---|
| A | TC-ID | 12 | Test case ID: `TC-[TASK_KEY]-[NUMBER]` (e.g., TC-SSI-API-001-001) |
| B | Suite | 15 | Test suite grouping (see Suite Values below) |
| C | Type | 14 | Test type: Happy path, Boundary, Negative, Integration, UI/UX, Data integrity, Regression |
| D | Priority | 10 | Critical, High, Medium, Low |
| E | Title | 40 | Short descriptive title of the test case |
| F | Preconditions | 35 | What must be true before executing this test |
| G | Steps | 45 | Numbered steps to execute (use newlines within cell) |
| H | Test Data | 30 | Specific input values to use |
| I | Expected Result | 35 | What should happen if the test passes |
| J | Actual Result | 25 | QA fills this during execution (leave empty) |
| K | Status | 12 | QA fills this: Pass / Fail / Blocked / Skipped (leave empty) |
| L | Notes | 25 | QA fills this for failure details or observations (leave empty) |

### Suite Values

Group test cases into suites based on what area they test:

| Suite | Use for |
|---|---|
| Scoring Logic | Assessment scoring, calculation thresholds, interpretation rules |
| Business Logic | Workflows, status changes, rules, conditions |
| API | Endpoint requests/responses, parameters, error codes |
| UI Functional | Forms, buttons, navigation, interactions |
| UI Display | Layout, labels, data rendering, responsiveness |
| Data | Save/retrieve, data integrity, data format |
| Access Control | Permissions, roles, authorization |
| Integration | Cross-platform behavior (API → Flutter, API → Web) |
| Regression | Existing functionality not broken |
| Edge Cases | Unusual inputs, concurrent actions, empty states |

### TC-ID Format

`TC-[TASK_KEY]-[3-DIGIT-NUMBER]`

Examples:
- `TC-SSI-API-001-001` (first test case for task SSI-API-001)
- `TC-SSI-TAB-003-012` (12th test case for task SSI-TAB-003)
- `TC-BUG-SSI-001-003` (3rd test case for bug BUG-SSI-001)

---

## 2. Spreadsheet Formatting

### Colors

| Element | Fill Color | Font Color | Font |
|---|---|---|---|
| Header row | 2563EB (blue) | FFFFFF (white) | Arial 11pt Bold |
| Critical priority row | FEE2E2 (light red) | 991B1B (dark red) | Arial 10pt |
| High priority row | No fill | 000000 | Arial 10pt |
| Medium priority row | No fill | 000000 | Arial 10pt |
| Low priority row | F3F4F6 (light gray) | 6B7280 (gray) | Arial 10pt |
| Suite header row | E0E7FF (light blue) | 1E40AF (dark blue) | Arial 10pt Bold |

### Cell Formatting

```python
from openpyxl.styles import Font, PatternFill, Alignment, Border, Side

# Header style
header_font = Font(name="Arial", size=11, bold=True, color="FFFFFF")
header_fill = PatternFill("solid", fgColor="2563EB")
header_alignment = Alignment(horizontal="center", vertical="center", wrap_text=True)

# Body style
body_font = Font(name="Arial", size=10)
body_alignment = Alignment(vertical="top", wrap_text=True)

# Priority fills
critical_fill = PatternFill("solid", fgColor="FEE2E2")
low_fill = PatternFill("solid", fgColor="F3F4F6")

# Suite separator
suite_font = Font(name="Arial", size=10, bold=True, color="1E40AF")
suite_fill = PatternFill("solid", fgColor="E0E7FF")

# Borders
thin_border = Border(
    left=Side(style="thin", color="E5E7EB"),
    right=Side(style="thin", color="E5E7EB"),
    top=Side(style="thin", color="E5E7EB"),
    bottom=Side(style="thin", color="E5E7EB"),
)
```

### Row Heights

- Header row: 30px
- Suite separator rows: 25px
- Body rows: auto (wrap_text handles it)

### Freeze Panes

Freeze row 1 (header) so it stays visible when scrolling:
```python
sheet.freeze_panes = "A2"
```

### Auto-filter

Enable auto-filter on all columns so QA can filter by Suite, Type, Priority, Status:
```python
sheet.auto_filter.ref = f"A1:L{last_row}"
```

---

## 3. Sheet Structure

### Single Task

One sheet named after the task key:
- Sheet name: `TC-SSI-API-001`
- Rows grouped by Suite, with a suite separator row before each group

### Multiple Tasks

One sheet per task, each named by task key. Plus a "Summary" sheet with counts per task.

### Row Ordering

Within the sheet, order rows by:
1. **Suite** (grouped together)
2. Within each suite, by **Priority** (Critical → High → Medium → Low)

Insert a **suite separator row** before each new suite:
- Merge columns A-L
- Text: suite name (e.g., "Scoring Logic")
- Use suite_fill and suite_font styles
- This makes it scannable when printed or viewed in Google Sheets

---

## 4. Sample Rows

For a FRAIL Assessment task (SSI-WEB-005), the spreadsheet would contain:

### Suite: Scoring Logic

| TC-ID | Suite | Type | Priority | Title | Preconditions | Steps | Test Data | Expected Result |
|---|---|---|---|---|---|---|---|---|
| TC-SSI-WEB-005-001 | Scoring Logic | Happy path | Critical | All answers negative → Robust (score 0) | Patient record exists, user on FRAIL form | 1. Select 'Jarang' for Fatigue\n2. Select 'Tidak' for Resistance\n3. Select 'Tidak' for Ambulation\n4. Select fewer than 5 illnesses\n5. Select 'Tidak' for weight loss | Fatigue: Jarang, Resistance: Tidak, Ambulation: Tidak, Illness: 3 selected, Weight loss: Tidak | Total score = 0, interpretation displays "Robust" |
| TC-SSI-WEB-005-002 | Scoring Logic | Boundary | Critical | Score exactly 3 → Frail interpretation | Patient record exists | 1. Select positive for Fatigue (1)\n2. Select Tidak for Resistance (0)\n3. Select Ya for Ambulation (1)\n4. Select ≥5 illnesses (1)\n5. Select Tidak for weight loss (0) | F:1 R:0 A:1 I:1 L:0 = 3 | Total score = 3, interpretation displays "Frail" |
| TC-SSI-WEB-005-003 | Scoring Logic | Boundary | Critical | Score 2 → Pre-frail (boundary between pre-frail and frail) | Patient record exists | 1. Select positive for Fatigue (1)\n2. Select Ya for Resistance (1)\n3. All others negative | F:1 R:1 A:0 I:0 L:0 = 2 | Total score = 2, interpretation displays "Pre-frail" |
| TC-SSI-WEB-005-004 | Scoring Logic | Boundary | Critical | Illness section — exactly 5 of 11 diseases → score 1 | On Illness section | 1. Select exactly 5 chronic illnesses | Hipertensi, Diabetes, Kanker, PPOK, Asma | Illness sub-score = 1 |
| TC-SSI-WEB-005-005 | Scoring Logic | Boundary | Critical | Illness section — 4 of 11 diseases → score 0 | On Illness section | 1. Select exactly 4 chronic illnesses | Hipertensi, Diabetes, Kanker, PPOK | Illness sub-score = 0 |

### Suite: UI Functional

| TC-ID | Suite | Type | Priority | Title | Preconditions | Steps | Test Data | Expected Result |
|---|---|---|---|---|---|---|---|---|
| TC-SSI-WEB-005-006 | UI Functional | Happy path | High | Score auto-updates on selection change | On FRAIL form | 1. Select any answer\n2. Change the answer\n3. Observe score display | Toggle Fatigue between Yes/No | Score recalculates immediately without submit/refresh |
| TC-SSI-WEB-005-007 | UI Functional | Happy path | High | All 11 chronic illnesses clickable/selectable | On Illness section | 1. Click each illness checkbox | All 11 illnesses | Each checkbox toggles, illness count updates |

### Suite: Negative / Edge Cases

| TC-ID | Suite | Type | Priority | Title | Preconditions | Steps | Test Data | Expected Result |
|---|---|---|---|---|---|---|---|---|
| TC-SSI-WEB-005-008 | Edge Cases | Negative | High | Incomplete form — not all sections answered | On FRAIL form | 1. Fill only Fatigue and Resistance\n2. Attempt to save | Only 2 of 5 sections filled | Validation error shown, form not saved |
| TC-SSI-WEB-005-009 | Edge Cases | Negative | Medium | Double-click on illness checkbox | On Illness section | 1. Rapidly double-click a checkbox | Any illness | Checkbox toggles once, no duplicate scoring |

### Suite: Data

| TC-ID | Suite | Type | Priority | Title | Preconditions | Steps | Test Data | Expected Result |
|---|---|---|---|---|---|---|---|---|
| TC-SSI-WEB-005-010 | Data | Data integrity | High | Assessment saved with correct payload | Completed FRAIL form | 1. Complete all sections\n2. Save\n3. Verify saved record | Score 3, Frail interpretation | Record contains patient_id, per-section scores, total score, interpretation, timestamp |

---

## 5. Coverage Summary Sheet

When generating test cases, include a "Coverage" sheet at the end with:

| Row | Content |
|---|---|
| Task Key | The task key being tested |
| Task Name | From the task brief |
| Total Test Cases | Count |
| By Priority | Critical: X, High: X, Medium: X, Low: X |
| By Type | Happy path: X, Boundary: X, Negative: X, etc. |
| DoD Coverage | List each DoD item and its matching TC-ID |
| Gaps | Any DoD items without matching test cases |

### DoD Coverage Table Format

| Column | Content |
|---|---|
| A | DoD Item (text from task brief) |
| B | Covered By (TC-ID or "GAP — needs test case") |

This is the most important part of the coverage sheet — it proves every DoD item has a test.
