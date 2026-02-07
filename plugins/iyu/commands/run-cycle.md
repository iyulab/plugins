---
description: Execute iterative development cycles - scope→research→implement→test→evaluate→improve loop
argument-hint: "[total_cycles] [start_cycle]" [--dry-run] [--no-commit]
disable-model-invocation: true
allowed-tools: Read, Glob, Grep, Write, Edit, TodoWrite, WebFetch, WebSearch, Bash
---

# Development Cycle Runner

Execute automated iterative development cycles.
Complete all cycles sequentially **without interruption**.

## Philosophy

**"Iterative Excellence"** - Perfection in a single cycle is not required. Each cycle just needs to be better than the last.

**"Research before code"** - When uncertain, investigate first. Never implement based on guesswork.

**"Scope discipline"** - Only take on what can be completed in a single cycle. Ambition kills quality.

**"Honest evaluation"** - Never be lenient with your own code. Record defects openly, never hide them.

## Mindset: Critical but Constructive

**"Passionate Developer"** - Not a passive executor, but a developer deeply invested in the project's success.

**"1 → 10 thinking"** - Derive ten implications from a single implementation.

**"Proactive research"** - When uncertain, actively use WebSearch to investigate best practices, patterns, and pitfalls.

**"Be your own QA"** - Test, verify, simulate. Anticipate failures rather than waiting for them.

**"Defend the architecture"** - Reject architectural violations made for convenience.

## Parameters

- Total cycles: `$ARGUMENTS[0]` (default: 5)
- Starting cycle number: `$ARGUMENTS[1]` (default: 1)

**Flags**:
- `--dry-run`: Preparation + roadmap establishment only (no execution)
- `--no-commit`: Execute cycles but skip commits

---

## Phase 0: Preparation

Must be performed before cycle execution:

### 0.1 Conversation Context Analysis (HIGHEST PRIORITY)

**Before any file exploration, analyze the preceding conversation.**

The user may have already discussed what needs to be done. Look for:
- Explicit statements like "~을 해야 합니다", "~이 필요합니다", "~을 구현해야 합니다"
- Discussed features, bugs, improvements, or refactoring plans
- Agreed-upon next steps or action items
- Any technical discussion that implies development scope

**If conversation context provides clear scope** → Use it directly. Skip 0.4 (Development Plan Discovery).

**If conversation context is ambiguous** → Use it as a starting hint, then verify against project artifacts.

**If no relevant conversation context** → Proceed to 0.2+.

### 0.2 Project Context

```bash
# Understand project principles
Read: CLAUDE.md (or .claude/CLAUDE.md)

# Understand project structure (only if not already known from conversation)
LS: project root
Read: README.md, package.json / Cargo.toml / pyproject.toml / *.csproj (whichever applies)
```

**Items to identify**:
- The project's **core role** (what does this library/app do?)
- The project's **scope boundaries** (what does it NOT do?)
- Architecture principles and coding conventions
- Tech stack and dependencies

### 0.3 Previous Cycle Logs

```bash
# Check previous cycle records
Glob: claudedocs/cycle-logs/cycle-*.md
# Read the most recent log (if any)
```

- Review defects/improvements identified in previous cycles
- Always reference the "Next Cycle Recommendation" section

### 0.4 Current State Assessment

```bash
# Check recent changes
git log --oneline -20
git diff HEAD~5 --stat

# Check build/test status
# (run according to project's build system)
```

### 0.5 Development Plan Discovery

**Only if conversation context did NOT provide clear scope.**

**Search Order** (stop at first found):
1. Previous cycle logs - "Next Cycle Recommendation" sections
2. CLAUDE.md - Development plan section
3. ROADMAP.md / roadmap.md
4. TASKS.md / TODO.md
5. docs/ - Planning documents
6. README.md - "Roadmap", "Planned Features" sections

```bash
Glob: **/ROADMAP.md, **/TASKS.md, **/TODO.md, docs/*.md
Grep: "## Roadmap|## TODO|## Planned|## Tasks|## Development Plan" in *.md
```

**If nothing found anywhere**: Ask the user what to work on. Do not invent scope.

### 0.6 Roadmap Establishment

**If a roadmap exists**: Read it and assess current progress.

**If no roadmap exists**: Analyze project state and create one at `claudedocs/cycle-logs/ROADMAP.md`.
- Phased plan aligned to total cycle count
- Goals and scope outline for each cycle
- Inter-cycle dependencies

```
PREPARATION COMPLETE
+--------------------+--------------------------------------------+
| Project            | [name]                                     |
| Role               | [library / app / framework / tool]         |
| Tech Stack         | [language, key dependencies]               |
| Previous Cycles    | [N completed / fresh start]                |
| Scope Source       | [conversation / roadmap / cycle-log / user] |
| Roadmap            | [existing / newly created / not needed]    |
| Total Cycles       | [N planned]                                |
| Starting From      | Cycle [M]                                  |
+--------------------+--------------------------------------------+
```

**If --dry-run, stop here.**

---

## Per-Cycle Process

**Every cycle strictly follows these 5 steps.**

---

### STEP 1: Scope Setting

Determine this cycle's scope based on the roadmap and previous cycle logs.

#### 1.1 Scope Definition

- Limit to a size **completable within a single cycle**
- Set specific and verifiable goals
- Specify deliverables

#### 1.2 Philosophy Alignment

Evaluate alignment with project philosophy:

| Dimension | Score 1-5 | Assessment |
|-----------|-----------|------------|
| **Core Mission Fit** | | Does it contribute to the project's core role? |
| **Scope Boundaries** | | Is it within the project's scope? |
| **Architecture Patterns** | | Is it consistent with existing architecture patterns? |
| **Dependency Direction** | | Is the dependency direction correct? (No upstream→downstream leakage?) |

**Decision**:
```
             | Scope IN        | Scope OUT       |
-------------|-----------------|-----------------|
Mission HIGH | PROCEED         | ADAPT scope     |
Mission MED  | ADAPT approach  | REDUCE scope    |
Mission LOW  | RECONSIDER      | SKIP            |
```

#### 1.3 Role & Scope Check

Verify the library/project's role and scope:
- Is this implementation the **responsibility** of this project?
- Should this be done in a different module/package instead?
- Are upstream consumer concerns leaking in?

```
CYCLE [N] SCOPE
+--------------------+--------------------------------------------+
| Title              | [cycle title]                              |
| Objective          | [specific goal]                            |
| Deliverables       | [deliverables list]                        |
| Philosophy Score   | [avg / 5]                                  |
| Decision           | [PROCEED / ADAPT / REDUCE / SKIP]          |
+--------------------+--------------------------------------------+
```

---

### STEP 2: Research & Implementation

#### 2.1 Research Phase

**Always investigate before implementing.**

Use the Task tool with subagents or research directly:

| Research Area | Tools | Purpose |
|---------------|-------|---------|
| Best Practices | WebSearch | Industry best practices for the feature |
| Existing Patterns | Grep, Glob | Similar patterns in the codebase |
| External References | WebSearch, WebFetch | Library docs, API references |
| Edge Cases | WebSearch | Known edge cases and pitfalls |
| Security | WebSearch | Security considerations (when applicable) |

**Research Triggers** (automatic):
- New technologies/patterns not in the codebase
- External integration needed
- Security/performance concerns
- Complex algorithms/logic

**Output**:
```
RESEARCH SUMMARY
- [topic]: [findings] ([source])
- [topic]: [findings] ([source])
Key Decision: [implementation direction based on research]
```

#### 2.2 Implementation Phase

Write code based on research findings:

- **Follow project coding conventions** (refer to CLAUDE.md)
- **Write tests alongside implementation**: Create tests together with the code
- **Progress incrementally**: Implement and verify in units, not all at once
- **Root Cause Mindset**: When problems are found, fix the root cause

**Progress tracking**:
```bash
TodoWrite: Track progress of each sub-task
```

---

### STEP 3: Test & Verify

Run tests appropriate to the project's tech stack:

| Tech Stack | Test Commands |
|------------|---------------|
| Node.js/TS | `npm test`, `npm run lint`, `npm run build` |
| Rust | `cargo test`, `cargo clippy -- -D warnings` |
| Python | `pytest`, `ruff check`, `mypy` |
| .NET | `dotnet test`, `dotnet build` |
| Go | `go test ./...`, `go vet ./...` |
| Generic | Determine test commands from README.md or CI config |

**Verification procedure**:
1. Run full test suite
2. Run lint/static analysis
3. Verify build
4. **On failure**: Fix and re-run (self-recovery)
5. If test coverage is insufficient, write additional tests

```
TEST RESULTS
+--------------------+--------------------------------------------+
| Tests              | [passed] / [total] ([failures] failed)     |
| Lint               | [PASS / N warnings / N errors]             |
| Build              | [SUCCESS / FAILURE]                        |
| Coverage           | [sufficient / needs improvement]           |
+--------------------+--------------------------------------------+
```

---

### STEP 4: Objective Evaluation

Evaluate this cycle's results on a **1-10 scale** using the following criteria:

| Criterion | Description | Score |
|-----------|-------------|-------|
| **Correctness** | Does the feature work correctly? Edge cases handled? | /10 |
| **Architecture** | Is the design consistent with project architecture? Appropriate abstractions? | /10 |
| **Philosophy Alignment** | Does it align with project philosophy/principles? Within role scope? | /10 |
| **Test Quality** | Are there sufficient meaningful tests? Do they cover core scenarios? | /10 |
| **Documentation** | Is the code self-explanatory? Is necessary documentation present? | /10 |
| **Code Quality** | Language idioms followed? Readability? Maintainability? | /10 |

**Evaluation principles**:
- **Score 7 or below**: Must record specific defects and improvement directions
- **No self-leniency**: "It works" ≠ "It's good code"
- **No relative grading**: Evaluate on an absolute scale against the project's standards

```
EVALUATION
+------------------------+-------+----------------------------------+
| Criterion              | Score | Notes                            |
+------------------------+-------+----------------------------------+
| Correctness            | /10   |                                  |
| Architecture           | /10   |                                  |
| Philosophy Alignment   | /10   |                                  |
| Test Quality           | /10   |                                  |
| Documentation          | /10   |                                  |
| Code Quality           | /10   |                                  |
+------------------------+-------+----------------------------------+
| AVERAGE                | /10   |                                  |
+------------------------+-------+----------------------------------+
```

---

### STEP 5: Issues & Improvements

#### 5.1 Deficiency Analysis

Record **specific defects** for items scoring below 7:
- What is lacking?
- How can it be improved?
- Can it be resolved in the next cycle?

#### 5.2 Architectural Discoveries

Architecture-related findings during the cycle:
- Need for design changes
- Technical debt identified
- Refactoring proposals

#### 5.3 Next Cycle Recommendation

Specific recommendations for the next cycle:
- Priority items to resolve
- Need for scope adjustments
- Prerequisites

```
ISSUES & IMPROVEMENTS
+------+----------+-----------------------------------------+--------+
| #    | Severity | Description                             | Action |
+------+----------+-----------------------------------------+--------+
| I-01 | [H/M/L]  | [issue description]                     | [next] |
| I-02 | [H/M/L]  | [issue description]                     | [next] |
+------+----------+-----------------------------------------+--------+
```

---

## Cycle Log Format

Create a `claudedocs/cycle-logs/cycle-{NN}.md` file upon each cycle completion.

```markdown
# Cycle {NN}: {Title}

## Date
{YYYY-MM-DD}

## Scope
{Implementation scope for this cycle}

## Philosophy Alignment
| Dimension | Score |
|-----------|-------|
| Core Mission Fit | /5 |
| Scope Boundaries | /5 |
| Architecture Patterns | /5 |
| Dependency Direction | /5 |

## Research Summary
{Research findings summary, references}

## Implementation
{Implementation summary}
- Files added/modified
- Key design decisions

## Test Results
{Test execution results}
- Pass/fail counts
- Lint/build status

## Evaluation
| Criterion | Score | Notes |
|-----------|-------|-------|
| Correctness | /10 | |
| Architecture | /10 | |
| Philosophy Alignment | /10 | |
| Test Quality | /10 | |
| Documentation | /10 | |
| Code Quality | /10 | |
| **Average** | **/10** | |

## Issues & Improvements
{Defects, deficiencies, and improvements identified}

## Pending Human Decisions
{Items requiring human judgment — architectural choices, breaking changes, ambiguous requirements}
{If none, write "None"}

## Next Cycle Recommendation
{Items for the next cycle, or "No further cycles needed" if no issues found}
```

---

## Cycle Completion & Commit

Upon each cycle completion:

1. **Write cycle log** (required)
2. **Commit** (unless --no-commit):
   - Message: `cycle-{NN}: {summary}`
   - Stage only relevant files (no git add -A)

**Version Rules**:
- **NEVER bump MAJOR** - Major version changes require explicit user decision
- **MINOR**: New features/major improvements (0.X.0)
- **PATCH**: Bug fixes/minor improvements (0.0.X)

---

## Execution Rules

1. **No interruptions**: Do not ask the user questions or request confirmation. Make all decisions autonomously.
2. **Self-recovery**: Fix test failures, compilation errors, etc. and continue.
3. **Logs required**: Always write the cycle log before moving to the next cycle.
4. **Quality first**: Quality over quantity. Reduce scope if needed to achieve high quality.
5. **Research required**: Actively use WebSearch to gather evidence. No guesswork implementations.
6. **Defend architecture**: Immediately fix any architectural violations made for convenience.
7. **Honest evaluation**: Never hide defects or inflate scores.
8. **Early termination**: If a cycle's evaluation produces **zero issues/improvements** in STEP 5, terminate the cycle loop early — even if the total cycle count has not been reached. The project has reached a stable state for now.
9. **Human judgment deferral**: When a situation requiring human judgment arises (e.g., major architectural decisions, breaking API changes, ambiguous requirements), do NOT stop the cycle. Instead:
   - Record it in the cycle log under a dedicated **"Pending Human Decisions"** section
   - Continue with the remaining actionable issues
   - Accumulate all deferred items across cycles for the user to review at the end
10. **Conversation context first**: Always check the preceding conversation for scope before searching project files. The user's recent discussion IS the primary input.

## Integration: Claude & iyu Plugins

| Situation | Leverage |
|-----------|----------|
| Bug discovered | `/iyu:issue` root cause analysis framework |
| Code writing | Claude's native ability |
| Code review needed | `feature-dev:code-reviewer` agent |
| Commit | `/commit` or built-in commit phase |
| PR creation | `/commit-push-pr` available |

---

## Output Format (Cycle Complete)

```
================================================================
               CYCLE [N] COMPLETE
================================================================
Title: [cycle title]

SUMMARY
+--------------------+--------------------------------------------+
| Scope              | [scope description]                        |
| Files Changed      | [+N -M modified]                           |
| Tests              | [passed/total]                             |
| Build              | [Success / Failure]                        |
| Evaluation Avg     | [X.X / 10]                                 |
| Commit             | [hash] or [skipped]                        |
+--------------------+--------------------------------------------+

TOP ISSUES
- [I-01]: [description] ([severity])
- [I-02]: [description] ([severity])

NEXT CYCLE
+--------------------+--------------------------------------------+
| Cycle              | [N+1]                                      |
| Planned Scope      | [next scope]                               |
| Priority Fixes     | [from current cycle issues]                |
+--------------------+--------------------------------------------+
================================================================
```

## Early Termination Output

When terminating early due to no issues found:

```
================================================================
         CYCLES COMPLETE (EARLY TERMINATION at [N] / [Total])
================================================================
Reason: No deficiencies, improvements, or defects identified.
The project has reached a stable state for the current scope.

PENDING HUMAN DECISIONS (if any accumulated across cycles)
- [HD-01]: [description] — [context/options]
- [HD-02]: [description] — [context/options]
================================================================
```

## Final Output (All Cycles Complete)

```
================================================================
            ALL CYCLES COMPLETE ([N] / [N])
================================================================

CYCLE SUMMARY
+-------+---------------------------+----------+-----------------+
| Cycle | Title                     | Avg Score| Key Achievement |
+-------+---------------------------+----------+-----------------+
| 1     | [title]                   | [X.X/10] | [achievement]   |
| 2     | [title]                   | [X.X/10] | [achievement]   |
| ...   | ...                       | ...      | ...             |
+-------+---------------------------+----------+-----------------+

OVERALL METRICS
+--------------------+--------------------------------------------+
| Total Files        | [+N -M modified]                           |
| Total Commits      | [N]                                        |
| Average Score      | [X.X / 10]                                 |
| Trend              | [improving / stable / declining]            |
+--------------------+--------------------------------------------+

OUTSTANDING ISSUES
- [unresolved issues across all cycles]

PENDING HUMAN DECISIONS
- [HD-01]: [description] — [context/options]
- (accumulated across all cycles)

RECOMMENDATIONS
- [strategic recommendations for future development]
================================================================
```

## Execution Start

Begin execution immediately following the rules above.
Run fully automatically: Phase 0: Preparation → Roadmap → Cycle 1 → Cycle 2 → ... → Cycle N.
