---
description: Execute development tasks - auto-detect from roadmap or use provided input
argument-hint: [task-description] [--dry-run] [--no-commit]
allowed-tools: Read, Glob, Grep, Write, Edit, TodoWrite, WebFetch, WebSearch, Bash
---

# Development Phase Runner

Autonomous agent for roadmap-driven or input-driven development with planning, execution, review, and cleanup.

## Core Philosophy

**"Roadmap-driven, philosophy-aligned"** - Trace changes to planned work, align with project principles.

**"Root cause over symptom"** - No surface fixes. Solve underlying problems.

**"No tech debt"** - Complete implementations only. Partial = forgotten.

**"Bold refactoring"** - Fix inefficiency now; propose large refactors as next phase if scope exceeds.

**"Critical review"** - Every step open to questioning and improvement.

## Integration: /iyu:issue

During execution:
- Bug discovered → `/iyu:issue` root cause analysis
- Pattern found → Check similar across codebase
- Scope exceeded → Document for future phase

## Input Modes

### Mode A: No Input (Roadmap-Driven)
```bash
/iyu:run
```
1. Scan ROADMAP.md for next pending phase
2. If no pending phases → **EXIT** with "All phases complete"
3. Extract tasks from phase scope
4. Execute with full workflow

### Mode B: With Input (Input-Driven)
```bash
/iyu:run "Add caching layer to API endpoints"
/iyu:run "Fix memory leak in worker threads"
```
1. Parse input as task description
2. Derive scope and tasks from input
3. Check philosophy alignment
4. Execute with full workflow
5. Update roadmap if applicable

**Flags** (both modes):
- `--dry-run`: Plan only, no execution
- `--no-commit`: Execute but skip commit

## Pre-Execution Context

**Required** (check in order):
1. **ROADMAP.md** / **docs/roadmap.md** - Phase definitions
2. **CLAUDE.md** - Project philosophy
3. **README.md** - Fallback

**Mode A without roadmap**:
```
No roadmap found. No pending work detected.
Provide task description or create ROADMAP.md.
```
→ EXIT

**Mode B without roadmap**: Proceed with input-driven execution (roadmap optional).

---

## Execution Flow

```
SCAN → EXTRACT → ALIGN → PLAN → EXECUTE → REVIEW → CLEANUP → COMMIT
```

---

### PHASE 1: ROADMAP SCAN

**Objective**: Identify next actionable phase.

**Actions**:
1. Read roadmap file
2. Parse phase statuses: `[ ]` pending, `[x]` done, `[~]` in-progress
3. Find first incomplete phase
4. Extract phase scope and success criteria

**Output**:
```
ROADMAP SCAN
+------------------+------------------------------------------------+
| Roadmap Source   | [file path]                                    |
| Total Phases     | [N]                                            |
| Completed        | [M] ([percentage]%)                            |
| Next Phase       | [phase name/number]                            |
+------------------+------------------------------------------------+
| Phase Title      | [title]                                        |
| Description      | [brief description]                            |
| Success Criteria | [measurable outcomes]                          |
+------------------+------------------------------------------------+
| Status           | [READY / BLOCKED / NONE_PENDING]               |
+------------------+------------------------------------------------+
```

**If NONE_PENDING**:
```
All phases complete. No pending work found.
Consider:
- Adding new phases to roadmap
- Running maintenance tasks
- Reviewing overall project status
```
→ EXIT

**If BLOCKED**:
```
Next phase blocked by: [dependency]
Cannot proceed until: [condition]
```
→ EXIT

---

### PHASE 2: TASK EXTRACTION

**Objective**: Derive concrete, actionable tasks from phase scope.

**Actions**:
1. Break down phase into implementation tasks
2. Research if unfamiliar territory (WebSearch)
3. Identify dependencies between tasks
4. Estimate complexity per task

**Research Triggers** (auto-activate):
- New technology/pattern not in codebase
- Phase mentions "best practice", "modern", "latest"
- External integration required
- Security/performance critical work

**Output**:
```
TASK EXTRACTION
+------------------+------------------------------------------------+
| Phase            | [phase name]                                   |
| Tasks Derived    | [N]                                            |
| Research Done    | [Yes: topic / No]                              |
+------------------+------------------------------------------------+

TASK BREAKDOWN
+----+------------------+------------+------------------+
| #  | Task             | Complexity | Dependencies     |
+----+------------------+------------+------------------+
| 1  | [task name]      | [S/M/L]    | [none / #N]      |
| 2  | [task name]      | [S/M/L]    | [#1]             |
| ...                                                   |
+----+------------------+------------+------------------+

EXECUTION ORDER
1. [task] - [brief reason for order]
2. [task] - [brief reason]
...
```

---

### PHASE 3: PHILOSOPHY ALIGNMENT

**Objective**: Gate execution based on project philosophy fit.

**Actions**:
1. Load CLAUDE.md / README.md
2. Score each task against:
   - Core mission fit (1-5)
   - Scope boundaries (1-5)
   - Architecture patterns (1-5)
   - Naming/style conventions (1-5)
3. Determine overall verdict

**Decision Matrix**:
```
             | Scope IN        | Scope OUT       |
-------------|-----------------|-----------------|
Mission HIGH | PROCEED         | ADAPT           |
Mission MED  | ADAPT           | PARTIAL         |
Mission LOW  | PARTIAL         | REJECT          |
```

**Verdicts**:
| Verdict | Meaning | Action |
|---------|---------|--------|
| **PROCEED** | 철학 일치 | 전체 태스크 실행 |
| **ADAPT** | 변경 수용 | 조정 후 실행 (방향/범위 수정) |
| **PARTIAL** | 부분 수용 | 일부 태스크만 실행, 나머지 제외 |
| **REJECT** | 실행 거부 | EXIT - 철학 위배로 진행 불가 |

**Output**:
```
PHILOSOPHY ALIGNMENT
+------------------+------------------------------------------------+
| Source           | [CLAUDE.md / README.md / None]                 |
+------------------+------------------------------------------------+

TASK ALIGNMENT
+------------------+-------+-------+------------------------------------+
| Task             | Mission | Scope | Notes                            |
+------------------+-------+-------+------------------------------------+
| [task 1]         | 5     | 5     | Core mission, in scope           |
| [task 2]         | 4     | 3     | Aligned but stretches boundary   |
| [task 3]         | 2     | 1     | Out of scope, mission conflict   |
+------------------+-------+-------+------------------------------------+

VERDICT: [PROCEED / ADAPT / PARTIAL / REJECT]
+------------------+------------------------------------------------+
| Rationale        | [1-2 sentence explanation]                     |
+------------------+------------------------------------------------+
```

**If ADAPT**:
```
ADAPTATIONS REQUIRED
- [task N]: [original] → [adjusted approach]
- [task M]: [scope narrowing or direction change]

Proceed with adaptations? [Y/n]
```

**If PARTIAL**:
```
PARTIAL EXECUTION
INCLUDED (philosophy-aligned):
- [task 1]: [reason]
- [task 2]: [reason]

EXCLUDED (philosophy conflict):
- [task 3]: [conflict reason] → Consider for separate discussion
- [task 4]: [out of scope] → May fit different project

Proceed with partial scope? [Y/n]
```

**If REJECT**:
```
================================================================
                    EXECUTION REJECTED
================================================================
Requested work conflicts with project philosophy.

CONFLICTS:
- [Conflict 1]: [explanation]
- [Conflict 2]: [explanation]

ALTERNATIVES:
- [What would be acceptable instead]
- [How to reframe the request]

Cannot proceed. Please revise scope or update project philosophy.
================================================================
```
→ EXIT

---

### PHASE 4: PLANNING

**Objective**: Create detailed execution plan.

**Actions**:
1. Compile tasks with adjustments
2. Define per-task acceptance criteria
3. Identify files to create/modify
4. Note potential risks

**Output**:
```
EXECUTION PLAN
+------------------+------------------------------------------------+
| Phase            | [phase name]                                   |
| Tasks            | [N]                                            |
| Est. Files       | [M files to modify/create]                     |
+------------------+------------------------------------------------+

DETAILED PLAN
+----+------------------+----------------------------------+----------+
| #  | Task             | Deliverables                     | Criteria |
+----+------------------+----------------------------------+----------+
| 1  | [task]           | - [file/feature]                 | [done if]|
|    |                  | - [file/feature]                 |          |
+----+------------------+----------------------------------+----------+
| ...                                                                  |
+----------------------------------------------------------------------+

RISKS & MITIGATIONS
- [Risk 1]: [mitigation approach]

READY TO EXECUTE: [YES / NO - reason]
```

**If --dry-run**: Output plan and EXIT

---

### PHASE 5: EXECUTION

**Objective**: Implement planned tasks with root-cause focus.

**Actions**:
1. Use TodoWrite to track progress
2. Execute tasks in order
3. Run tests after each significant change
4. Document decisions made during implementation

**Root Cause Mindset** (apply throughout):
- Problem encountered? → Ask "why" 5 times before fixing
- Quick fix tempting? → Resist. Find underlying cause.
- Pattern detected? → Check if exists elsewhere (use Grep/Glob)
- Tech debt spotted? → Fix now or log for immediate follow-up

**Issue Integration**:
When bug/defect discovered during execution:
1. Pause current task
2. Apply `/iyu:issue` root cause analysis framework:
   - Symptom isolation
   - Hypothesis generation
   - Evidence gathering (Grep/Glob/Read)
   - Similar pattern detection
3. Fix ALL instances found, not just the triggering one
4. Resume task

**Per-Task Loop**:
```
EXECUTING: [Task N] - [task name]
+------------------+------------------------------------------------+
| Status           | [In Progress / Completed / Blocked]            |
| Files Modified   | [list]                                         |
| Tests            | [Passed / Failed / N/A]                        |
| Issues Found     | [N - all resolved / M pending]                 |
| Patterns Fixed   | [N locations] (if applicable)                  |
| Notes            | [decisions, deviations, discoveries]           |
+------------------+------------------------------------------------+
```

**If task blocked/failed**:
- Analyze root cause (not just symptom)
- Attempt fundamental fix
- If requires large refactoring → Document, propose as next phase
- If unresolvable: pause, report full analysis, ask user

---

### PHASE 6: CRITICAL REVIEW

**Objective**: Verify implementation quality and completeness.

**Actions**:
1. Review all changes against acceptance criteria
2. Check for introduced issues:
   - Test failures
   - Lint/type errors
   - Unused code
   - Missing documentation
3. Identify improvement opportunities
4. Self-critique: what could be better?

**Output**:
```
CRITICAL REVIEW
+------------------+------------------------------------------------+
| Tasks Completed  | [N/M]                                          |
| Tests Status     | [All passing / N failures]                     |
| Build Status     | [Success / Failure]                            |
+------------------+------------------------------------------------+

CRITERIA CHECK
+------------------+--------+------------------------------------+
| Task             | Met?   | Notes                              |
+------------------+--------+------------------------------------+
| [task 1]         | YES    |                                    |
| [task 2]         | PARTIAL| [what's missing]                   |
+------------------+--------+------------------------------------+

ISSUES FOUND
- [Issue 1]: [severity] - [fix required]
- [Issue 2]: [severity] - [fix required]

IMPROVEMENT OPPORTUNITIES
- [Could enhance X by Y - not blocking]

TECH DEBT AUDIT
+------------------+--------+------------------------------------+
| Item             | Action | Rationale                          |
+------------------+--------+------------------------------------+
| [debt item]      | FIXED  | [how it was resolved]              |
| [debt item]      | DEFER  | [why + proposed phase]             |
+------------------+--------+------------------------------------+

REFACTORING PROPOSALS (if scope exceeded)
+------------------+------------------------------------------------+
| Proposal         | [Title]                                        |
| Scope            | [files/modules affected]                       |
| Rationale        | [why this refactoring is needed]               |
| Suggested Phase  | [Next / Phase N+2 / Dedicated refactor phase]  |
+------------------+------------------------------------------------+

SELF-CRITIQUE
- [What could have been done better]
- [Lessons for next phase]

VERDICT: [APPROVED / NEEDS_FIXES / REWORK_REQUIRED]
```

**If NEEDS_FIXES**: Loop back to EXECUTION for fixes
**If REWORK_REQUIRED**: Significant issues, reassess plan
**If REFACTORING_PROPOSED**: Add to roadmap before proceeding

---

### PHASE 7: CLEANUP

**Objective**: Ensure codebase cleanliness post-implementation.

**Actions**:
1. **Documentation**:
   - Update README if public API changed
   - Update CHANGELOG.md
   - Update ROADMAP.md (mark phase complete)
   - Remove/update obsolete docs

2. **Code Hygiene**:
   - Remove debug code
   - Remove unused imports
   - Format code
   - Remove temporary files

3. **Consolidation**:
   - Merge related docs if fragmented
   - Remove duplicate documentation
   - Verify cross-references

**Output**:
```
CLEANUP SUMMARY
+------------------+------------------------------------------------+
| Docs Updated     | [list: README, CHANGELOG, ROADMAP, etc.]       |
| Docs Removed     | [list of obsolete docs removed]                |
| Files Cleaned    | [list of files cleaned]                        |
| Temp Files       | [N removed]                                    |
+------------------+------------------------------------------------+

ROADMAP STATUS
- [x] [Phase N]: [phase name] - COMPLETED
- [ ] [Phase N+1]: [next phase name]
```

---

### PHASE 8: COMMIT

**Objective**: Create clean, meaningful commit.

**Actions**:
1. Stage relevant changes
2. Craft commit message (phase-based)
3. Bump version if warranted (MINOR or BUILD only)
4. Commit

**Version Rules**:
- **NEVER bump MAJOR** - Breaking changes require explicit user decision
- **MINOR**: New feature, significant enhancement
- **BUILD/PATCH**: Bug fixes, small improvements, internal changes

**Output**:
```
COMMIT
+------------------+------------------------------------------------+
| Staged Files     | [N files]                                      |
| Version Bump     | [None / MINOR: X.Y.0 → X.Y+1.0 / BUILD: X.Y.Z+1]|
+------------------+------------------------------------------------+

COMMIT MESSAGE
────────────────────────────────────────────────────────────────────
[type]: [brief description]

[Phase N]: [phase name]

Changes:
- [change 1]
- [change 2]

[optional: breaking changes, migration notes]
────────────────────────────────────────────────────────────────────

STATUS: [COMMITTED / SKIPPED (--no-commit)]
```

---

## Final Report

```
================================================================
                 DEVELOPMENT PHASE COMPLETE
================================================================
Phase: [phase name]
Duration: [start → end time if trackable]
================================================================

SUMMARY
+------------------+------------------------------------------------+
| Tasks Completed  | [N/M]                                          |
| Files Changed    | [+N -M modified]                               |
| Tests            | [All passing]                                  |
| Version          | [X.Y.Z → X.Y.Z']                               |
| Commit           | [hash] or [skipped]                            |
+------------------+------------------------------------------------+

DELIVERABLES
- [Deliverable 1]
- [Deliverable 2]

NEXT PHASE
+------------------+------------------------------------------------+
| Phase            | [next phase name or "None remaining"]          |
| Ready            | [Yes / No - blockers]                          |
+------------------+------------------------------------------------+

LESSONS LEARNED
- [Insight for future phases]
================================================================
```

---

## Error Handling

**No roadmap + no input**: EXIT - nothing to do
**No pending phases**: EXIT - all complete
**Task failure**: Log, attempt recovery, report if unresolvable
**Test failure**: Block commit, require fix
**Git issues**: Report status, suggest manual resolution

---

## Examples

```bash
# Mode A: Auto-detect next phase from roadmap
/iyu:run

# Mode B: Input-driven task
/iyu:run "Implement user authentication with JWT"
/iyu:run "Refactor database layer for connection pooling"

# Plan only (both modes)
/iyu:run --dry-run
/iyu:run "Add rate limiting" --dry-run

# Execute without commit
/iyu:run --no-commit
```
