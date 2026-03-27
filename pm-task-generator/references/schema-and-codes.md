# Schema & Codes Reference (v4.7)

## Table of Contents
1. Project Codes
2. Platform Codes
3. Module Values
4. API Specification Rules
5. Full JSON Schemas
6. Definition of Done Templates

---

## 1. Project Codes

| Project | Code |
|---|---|
| RUKUN | RKN |
| Atedia | ATD |
| RSUD | RSU |
| Muara Kasih (RPUK Muara Kasih) | RMK |
| SSI | SSI |
| Panti Werdha (PSTW Jombang Asrama Pare) | PJP |

Allowed `project` values: RKN, PJP, ATD, RSU, SSI, RMK

---

## 2. Platform Codes

| Platform | Code |
|---|---|
| API | API |
| WEB | WEB |
| Staff Tablet | TAB |
| Staff Mobile | MOB |
| Staff Field | FLD |
| Volunteer | VOL |
| NOK | NOK |
| Doctor | DOC |
| PhotoFrame | PFR |
| Email | EML |
| Senior App | SNR |

Allowed `platform` values: API, WEB, TAB, NOK, VOL, DOC, PFR, MOB, EML, FLD, SNR

---

## 3. Module Values (Optional)

Allowed values: Activity, Assessment, Authentication, Authorization, Configuration, Finance, Health, Incident, Integration, Meal, Member, Notification, Reporting, Sales, Volunteer, File Storage

Use only these predefined values. Field is optional.

---

## 4. API Specification Rules

### v4.4 Change
`api_specification` is now an **array of objects**. Each object = one endpoint.
- Single-API tasks → array with one object
- Multi-API tasks → one object per endpoint

### Structure per item

```json
{
  "label": "GET /v1/incidents/summary — New Endpoint",
  "method": "GET",
  "endpoint": "/v1/incidents/summary",
  "parameters": [],
  "response_sample": {}
}
```

### Required fields per item
- `label` (string) — short identifier, e.g. "GET /v2/member/sick — Updated"
- `method` (string) — GET, POST, PUT, PATCH, DELETE
- `endpoint` (string) — full endpoint path
- `parameters` (array) — list of request parameters
- `response_sample` (object) — example response payload

### Optional fields per item (updated endpoints only — omit for new endpoints)

- `backend_changes` (string) — **For Odoo dev.** Describes what changed in the backend: business logic, model fields, data source, computation, or validation rules. Written from the backend implementation perspective.
  - Example: `"Add soap_subjective, soap_objective, soap_assessment, soap_plan fields to medical.care model. All fields are Text type, optional, default empty string."`

- `contract_changes` (string) — **For Flutter dev.** Describes what changed in the API contract that the mobile app must adapt to: new request params, new/changed response fields, removed fields, or behavior changes. Written from the consumer perspective.
  - Example: `"Response now includes 4 new optional fields: soap_subjective, soap_objective, soap_assessment, soap_plan. All are strings, may return empty string. No existing fields changed or removed."`

**When to include these fields:**
- New endpoint → omit both (nothing changed, everything is new)
- Updated endpoint → include whichever are relevant; omit if not applicable to that audience

### Conditional rules
- If `platform` includes "API" → `api_specification` is **required**
- If `platform` does NOT include "API" → `api_specification` must be **omitted entirely**

### New API endpoint — additional DoD items
```
Endpoint implemented and accessible
Params validated
Response structure matches defined contract
Endpoint added to API list documentation
```
Documentation link: https://ww.slack.com/docs/T024FGN56/F09KZGPFXPT

### Updated API endpoint — rules
- Backward compatibility must be preserved
- Response structure MUST NOT change in a breaking way
- Existing mobile app must continue working
- If response change is unavoidable → create new versioned endpoint (e.g. /v2), do NOT override existing

### Updated API endpoint — additional DoD items
```
Existing response fields unchanged
No field removed or renamed
New fields (if any) are optional
Mobile app compatibility validated
Endpoint updated in API list documentation
```

---

## 5. Full JSON Schemas

### Feature / Enhancement / Refactor / Optimization

```json
{
  "task_key": "",
  "task_name": "",
  "type": "Feature | Enhancement | Refactor | Optimization",
  "status": "Draft",
  "priority": 1,
  "due_date": null,
  "platform": [],
  "project": [],
  "module": "",
  "comment": {
    "classification": {
      "severity": "P1 | P2 | P3",
      "ai_risk_level": "Low | Medium | High",
      "ai_risk_note": ""
    },
    "objective": "",
    "scope": {
      "in_scope": [],
      "out_of_scope": []
    },
    "api_specification": [
      {
        "label": "",
        "method": "",
        "endpoint": "",
        "parameters": [],
        "response_sample": {}
      }
    ],
    "action": [],
    "dod": [],
    "test_scenarios": [
      { "label": "Scenario 1", "input": "", "expected": "" }
    ],
    "notes": ""
  }
}
```

### Bug

```json
{
  "task_key": "",
  "task_name": "",
  "type": "Bug",
  "status": "Draft",
  "priority": 1,
  "due_date": null,
  "platform": [],
  "project": [],
  "module": "",
  "comment": {
    "classification": {
      "severity": "P1 | P2 | P3",
      "ai_risk_level": "Low | Medium | High",
      "ai_risk_note": ""
    },
    "problem_statement": {
      "actual": "",
      "expected": "",
      "environment": ""
    },
    "steps_to_reproduce": [],
    "action": [],
    "dod": [],
    "test_scenarios": [
      { "label": "Scenario 1", "input": "", "expected": "" }
    ],
    "notes": ""
  }
}
```

---

## 6. Priority Mapping

| Severity | Priority Value |
|---|---|
| P1 | 1 |
| P2 | 2 |
| P3 | 3 |