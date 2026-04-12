# Universal Output Template

Use this template for ALL project type analysis outputs:

```markdown
## Codebase Analysis: [Project-Name]

### Project Overview
- **Type:** [Flutter/Android/Firebase Functions/React Web/Static Web/Generic]
- **Detection Method:** [files that triggered detection]
- **Analysis Mode:** [Quick Scan/Deep Analysis]
- **Path:** [absolute path]
- **Analyzed At:** [timestamp]

### Architecture Pattern
- **Primary Pattern:** [pattern name]
- **Description:** [brief explanation]
- **Project-Specific Details:**
  - [type-specific architecture info]

### Dependencies
- **Package Manager:** [pub/npm/gradle/etc.]
- **Key Dependencies:**
  - [dependency]: [version]
  - [dependency]: [version]
- **Coolvacore Usage:** [Not Applicable / Not Detected / Partial / Full]
  - [Flutter only: list detected packages]

### Existing Features/Modules
- **[Feature/Module 1]:** [purpose] @ [location]
- **[Feature/Module 2]:** [purpose] @ [location]

### Key Patterns
- **State Management:** [pattern/type-specific]
- **API/Data Layer:** [Firestore/Functions/REST/Local]
- **UI Patterns:** [type-specific]
- **Testing Approach:** [type-specific or "Not detected"]

### File Structure
```
[directory tree output]
```

### Code Quality Observations
- **Documentation:** [README quality, inline docs, architecture docs]
- **Consistency:** [naming conventions, code style, project structure]
- **Dependencies:** [outdated packages, security concerns, unused deps]
- **Testing:** [test coverage indicators, test organization]
- **Maintainability:** [code complexity indicators, duplication]

### Universal Gap Analysis
- [ ] README.md exists and is comprehensive
- [ ] Documentation folder/docs present
- [ ] .env.example exists (if applicable)
- [ ] .gitignore properly configured
- [ ] Dependency lock file committed
- [ ] No critical security vulnerabilities
- [ ] Test structure present
- [ ] CI/CD configuration (if applicable)
- [ ] Consistent naming conventions
- [ ] Clear project organization

### Project-Specific Gaps/Issues
- [Issue 1 with severity and recommendation]
- [Issue 2 with severity and recommendation]

### Recommendations
- **[Priority: High]** [Recommendation with rationale]
- **[Priority: Medium]** [Recommendation with rationale]
- **[Priority: Low]** [Recommendation with rationale]
```

## Quick Scan vs Deep Analysis Differences

### Quick Scan Output
- 2-level directory tree only
- Top 10 dependencies maximum
- README summary (first 200 chars)
- Coolvacore status only (no detailed analysis)
- 3-5 key observations max
- Universal gap checklist (checked/unchecked only, no details)
- 1-2 recommendations max

### Deep Analysis Output
- 4-level directory tree
- Complete dependency list
- Full README analysis
- Coolvacore + architecture patterns
- Comprehensive quality observations
- Detailed gap analysis with explanations
- 3-5 prioritized recommendations
