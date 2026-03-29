# FBD Codes Reference (v1.0)

## Project Codes

| Project | Code |
|---|---|
| RUKUN | RKN |
| Atedia | ATD |
| RSUD | RSU |
| Muara Kasih (RPUK Muara Kasih) | RMK |
| SSI | SSI |
| Panti Werdha (PSTW Jombang Asrama Pare) | PJP |

Allowed project values: RKN, PJP, ATD, RSU, SSI, RMK

For new projects not in this list, generate a 3-letter uppercase code from the project name.

## Platform Codes

| Platform | Code |
|---|---|
| API | API |
| WEB | WEB |
| Staff Tablet | TAB |
| Staff Mobile | MOB |
| Staff Field | FLD |
| Volunteer | VOL |
| NOK (Next of Kin / Family) | NOK |
| Doctor | DOC |
| PhotoFrame | PFR |
| Email | EML |
| Senior App | SNR |

## Module Values

Predefined modules (use these when applicable):
Activity, Assessment, Authentication, Authorization, Configuration, Finance, Health, Incident, Integration, Meal, Member, Notification, Reporting, Sales, Volunteer, File Storage

## Module Short Codes (for FBD IDs)

| Module | Short Code |
|---|---|
| Member Management | MBR |
| Activity | ACT |
| Health | HLT |
| Staff & Volunteer | STF |
| Communication | COM |
| Reporting | RPT |
| Billing & Finance | FIN |
| Inventory & Facility | INV |
| Authentication | AUT |
| Authorization | AZN |
| Configuration | CFG |
| Assessment | ASM |
| Incident | INC |
| Meal | MEL |
| Notification | NTF |
| Integration | INT |
| Sales | SLS |
| File Storage | FIL |

For new modules, create a 2-3 letter uppercase abbreviation that is:
- Unique within the project
- Intuitive (reader can guess the module from the code)
- Documented in the Summary sheet of the FBD

## FBD ID Format

`FBD-{PROJECT_CODE}-{MODULE_SHORT}-{NUMBER}`

- PROJECT_CODE: 3 letters from Project Codes table
- MODULE_SHORT: 2-3 letters from Module Short Codes table
- NUMBER: 3-digit zero-padded sequential number within the module

Examples:
- FBD-SSI-MBR-001 — first feature in Member Management for SSI
- FBD-RKN-HLT-003 — third feature in Health for RUKUN
- FBD-ATD-ACT-012 — twelfth feature in Activity for Atedia
