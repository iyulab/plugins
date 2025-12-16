# Pattern Detection Guide

A comprehensive methodology for finding similar patterns in a codebase when resolving bugs, to identify and fix latent defects proactively.

## Philosophy

**"Fix N, Prevent M"** - When you find one bug (N), use it as a signal to find all similar patterns (M) that may have the same latent defect. This transforms reactive bug fixing into proactive quality improvement.

## Pattern Detection Process

### Step 1: Extract the Problematic Pattern

After identifying the root cause, abstract it into a searchable pattern:

**Questions to ask:**
- What code construct caused the bug? (null check, error handling, async call, etc.)
- What API or library was misused?
- What assumption was violated?
- What's the signature of the problematic code?

**Pattern Types:**

| Type | Example | Search Strategy |
|------|---------|-----------------|
| Missing null check | `obj.property` without `obj != null` | Grep for `\.property` without preceding null check |
| Incorrect error handling | `catch {}` empty catch | Grep for `catch\s*\{[\s\n]*\}` |
| Race condition | Shared state without lock | Grep for variable access patterns |
| API misuse | Wrong method signature | Grep for method name with wrong params |
| Resource leak | Open without close | Grep for `open\|acquire` without `close\|release` |

### Step 2: Syntactic Pattern Search

Use Grep/Glob to find exact or similar code patterns:

**Search Strategies:**

```bash
# Same function/method name
Grep: "functionName\s*\("

# Same API usage
Grep: "library\.methodName"

# Similar error handling pattern
Grep: "catch\s*\([^)]*\)\s*\{"

# Same variable naming pattern
Grep: "userInput|rawData|untrusted"

# Missing validation pattern
Grep: "request\.body\." # then check for validation
```

**Multi-file scope:**
- Start with the same directory as the bug
- Expand to related modules/features
- Then search project-wide

### Step 3: Semantic Pattern Search

Look for functionally similar code even with different syntax:

**Strategies:**

1. **Parallel Code Paths**
   - Other HTTP endpoints with similar logic
   - Other event handlers for similar events
   - Other CRUD operations on different entities

2. **Copy-Paste Variations**
   - Similar function names with different suffixes
   - Same logic in different files
   - Duplicated code blocks with minor changes

3. **Same Data Flow**
   - Input → Process → Output with same shape
   - Same transformation applied to different data
   - Similar validation/sanitization patterns

4. **Architectural Patterns**
   - Same layer (controller, service, repository)
   - Same responsibility type (auth, logging, caching)
   - Same integration pattern (external API calls)

### Step 4: Architectural Pattern Search

Look for structural issues that may cause similar bugs:

**Anti-patterns to check:**
- **God classes**: Large classes doing too much
- **Circular dependencies**: Module A → B → A
- **Layer violations**: UI calling database directly
- **Missing abstractions**: Copy-pasted code instead of shared function
- **Tight coupling**: Direct instantiation instead of dependency injection

### Step 5: Risk Assessment

For each found pattern, assess the risk level:

| Risk | Criteria | Action |
|------|----------|--------|
| 🔴 Critical | Same exact pattern, likely same bug | Fix immediately with this PR |
| 🟠 High | Very similar pattern, >70% chance of defect | Include in this fix |
| 🟡 Medium | Related pattern, should be reviewed | Schedule follow-up review |
| 🟢 Low | Loosely related, might be fine | Add to monitoring/tech debt |

**Assessment Questions:**
- Does this pattern have the SAME root cause vulnerability?
- Is it in a code path that gets executed similarly?
- Would the same fix apply here?
- What's the blast radius if this fails?

## Search Commands Reference

### Grep Patterns for Common Bug Types

**Null/Undefined Safety:**
```bash
# JavaScript/TypeScript - property access without optional chaining
Grep: "\w+\.\w+\." -exclude "?."

# Java - potential NPE
Grep: "\.get\w+\(\)"
```

**Error Handling:**
```bash
# Empty catch blocks
Grep: "catch\s*\([^)]*\)\s*\{\s*\}"

# Swallowed exceptions
Grep: "catch.*\{[^}]*console\.log"

# Missing try-catch around async
Grep: "await\s+\w+\(" # then verify try-catch exists
```

**Security Patterns:**
```bash
# SQL injection risk
Grep: "query.*\+.*\"|\".*\+.*query"

# XSS risk
Grep: "innerHTML\s*=|dangerouslySetInnerHTML"

# Command injection risk
Grep: "exec\(|spawn\(|system\("
```

**Resource Management:**
```bash
# File handles
Grep: "open\(|fopen\(|createReadStream"

# Database connections
Grep: "getConnection|createConnection"

# Event listeners
Grep: "addEventListener|\.on\("
```

**Async/Concurrency:**
```bash
# Missing await
Grep: "[^await\s]Promise\.|async.*=.*\("

# Race conditions
Grep: "setTimeout.*state|setInterval.*state"
```

## Output Format

When documenting found patterns, use this format:

```
DETECTED PATTERNS
================

Root Cause Pattern: [Description of the bug pattern]
Search Scope: [Directories/files analyzed]
Total Matches: [N patterns found]

+------+------------------+----------------------+---------------------------+
| Risk | Location         | Pattern Match        | Assessment                |
+------+------------------+----------------------+---------------------------+
| 🔴   | src/api/user.js:45 | Missing null check   | Same bug, user data      |
| 🟠   | src/api/order.js:78| Similar null access  | Different entity, same risk|
| 🟡   | src/util/format.js:23| Related null pattern| Utility, less critical   |
| 🟢   | tests/mock.js:12  | Test file pattern    | Not production code      |
+------+------------------+----------------------+---------------------------+

AGGREGATE RISK
--------------
🔴 Critical: 2 patterns - IMMEDIATE ACTION REQUIRED
🟠 High: 5 patterns - Address in this fix
🟡 Medium: 8 patterns - Schedule review
🟢 Low: 3 patterns - Monitor

RECOMMENDATION
--------------
[Fix N+M / Comprehensive refactor / Fix N only with follow-up]

PREVENTIVE FIXES
----------------
For Critical/High patterns:
1. [file:line] - [Specific fix description]
2. [file:line] - [Specific fix description]
```

## Integration with Root Cause Analysis

The pattern detection phase follows root cause analysis:

```
Root Cause Found → Extract Pattern → Search Codebase → Assess Risk → Plan Fix
      ↓                   ↓               ↓              ↓           ↓
 "Null pointer      "obj.prop         15 matches     2 critical   Fix all
  in user.js:45"   without check"     found           5 high      critical+high"
```

## Best Practices

1. **Start narrow, expand gradually**: Begin with the immediate directory, then module, then project
2. **Use multiple search strategies**: Syntactic alone misses semantic similarities
3. **Document everything**: Future bugs will reference this analysis
4. **Don't over-fix**: Low-risk patterns may be intentional or have different context
5. **Consider test coverage**: Patterns in well-tested code are lower risk
6. **Track patterns over time**: Recurring patterns indicate systemic issues

## Common Pitfalls

- **False positives**: Not every pattern match is a bug - context matters
- **Over-scoping**: Trying to fix everything creates unstable PRs
- **Under-scoping**: Fixing only N when M is clearly at risk
- **Missing semantic matches**: Only searching for exact syntax
- **Ignoring test code**: Sometimes test patterns reveal production risks
