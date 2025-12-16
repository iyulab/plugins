# Solution Research Methodology

A systematic approach for researching best practices, latest methods, and proven solutions when resolving complex bugs or implementing features.

## When to Research

Auto-trigger research when ANY of these conditions are met:

| Trigger | Rationale |
|---------|-----------|
| **Technical Complexity** | Affects 5+ files OR requires architectural changes |
| **Unfamiliar Territory** | <3 similar patterns in codebase (new problem space) |
| **Explicit Request** | Issue mentions "best practice", "latest", "modern approach" |
| **External Dependency** | Involves third-party library/API behavior |
| **Security Critical** | Security vulnerability or sensitive data handling |
| **Performance Critical** | Performance-critical code path |

Skip research when:
- `--no-research` flag is set
- `--quick` flag is set
- Issue is straightforward with clear solution in codebase

## Research Process

### Step 1: Query Formulation

Craft effective search queries based on the problem:

**Query Templates:**

| Problem Type | Query Pattern |
|--------------|---------------|
| Bug fix | `"[error message]" [technology] fix solution` |
| Best practice | `[technology] [topic] best practices [year]` |
| Security issue | `[technology] [vulnerability type] prevention OWASP` |
| Performance | `[technology] [operation] performance optimization` |
| API/Library | `[library name] [specific feature] example usage` |
| Architecture | `[pattern name] implementation [technology] [year]` |

**Examples:**

```
# Null pointer bug in React
"Cannot read property of undefined" React hooks fix 2024

# Authentication best practices
Node.js JWT authentication best practices 2024

# Database connection pooling
PostgreSQL connection pool Node.js best practices

# Async error handling
JavaScript async await error handling patterns 2024
```

### Step 2: Tool Selection

Choose the right research tool:

**Tavily MCP (Preferred when available):**
- Structured search results
- Better for technical queries
- Source credibility ranking
- Snippet extraction

**WebSearch (Fallback):**
- Built-in Claude capability
- Broader coverage
- Good for recent information

**Selection Logic:**
```
if (Tavily MCP available) {
  use Tavily
} else {
  use WebSearch
}
```

### Step 3: Source Prioritization

Not all sources are equal. Prioritize in this order:

| Priority | Source Type | Trust Level |
|----------|-------------|-------------|
| 1 | Official documentation | Highest |
| 2 | GitHub issues/discussions on the official repo | High |
| 3 | Stack Overflow accepted answers (< 2 years old) | High |
| 4 | Technical blogs from recognized experts | Medium-High |
| 5 | Security advisories (CVE, OWASP) | High (for security) |
| 6 | Medium/Dev.to articles | Medium |
| 7 | Random blog posts | Low - verify |
| 8 | AI-generated content | Low - verify |

**Red Flags:**
- Content older than 2 years (may be outdated)
- No author attribution
- No working code examples
- Contradicts official documentation
- Promotes deprecated practices

### Step 4: Information Extraction

Extract actionable information from sources:

**For each source, capture:**

1. **Best Practice / Recommendation**
   - What is the recommended approach?
   - Why is it recommended?
   - What version/context does it apply to?

2. **Code Examples**
   - Working code snippets
   - Configuration examples
   - Command examples

3. **Caveats and Trade-offs**
   - When NOT to use this approach
   - Performance implications
   - Security considerations

4. **Source Metadata**
   - URL for reference
   - Publication date
   - Author credibility

### Step 5: Synthesis and Recommendation

Combine findings into actionable recommendations:

**Synthesis Framework:**

```
SOLUTION RESEARCH SUMMARY
=========================

RESEARCH CONTEXT
----------------
Trigger: [Why research was initiated]
Query: [Search query used]
Sources Analyzed: [N sources]

FINDINGS
--------

Best Practice (Primary Recommendation):
+----------------------+----------------------------------------------+
| Approach             | [Specific recommended approach]              |
| Source               | [URL - authoritative source]                 |
| Applicability        | [High/Medium/Low] - [why this fits]          |
| Key Insight          | [Most important takeaway]                    |
+----------------------+----------------------------------------------+

Alternative Approaches:
+----------------------+----------------------------------------------+
| Alternative 1        | [Different valid approach]                   |
| Source               | [URL]                                        |
| Trade-offs           | [Pros vs primary approach]                   |
|                      | [Cons vs primary approach]                   |
+----------------------+----------------------------------------------+
| Alternative 2        | [Another option if applicable]               |
| ...                  |                                              |
+----------------------+----------------------------------------------+

SECURITY CONSIDERATIONS (if applicable)
---------------------------------------
- [Security finding 1 from OWASP/security sources]
- [Security finding 2]

DEPRECATED/AVOID
----------------
- [Approach that should NOT be used and why]

FINAL RECOMMENDATION
--------------------
+----------------------+----------------------------------------------+
| Recommended Approach | [Specific approach to implement]             |
| Rationale            | [Why this is best for THIS context]          |
| Implementation Steps | 1. [Step 1]                                  |
|                      | 2. [Step 2]                                  |
|                      | 3. [Step 3]                                  |
| Key References       | [Most useful URLs for implementation]        |
+----------------------+----------------------------------------------+
```

## Query Examples by Domain

### Security Issues

```
# SQL Injection
"SQL injection prevention" [language] parameterized queries 2024
[framework] ORM security best practices

# XSS Prevention
"XSS prevention" [framework] content security policy
[framework] output encoding sanitization

# Authentication
[framework] authentication best practices 2024
"secure session management" [language]
JWT vs session security comparison
```

### Performance Issues

```
# Database Performance
[database] query optimization explain analyze
[ORM] N+1 query problem solution
connection pooling best practices [database]

# Memory Issues
[language] memory leak detection profiling
[framework] memory optimization techniques

# Caching
[technology] caching strategy best practices
Redis vs Memcached [use case]
```

### Architecture Issues

```
# Design Patterns
[pattern name] implementation [language] example
"when to use" [pattern] vs [alternative pattern]

# Microservices
microservices [specific problem] best practices
service communication patterns 2024

# API Design
REST API design best practices 2024
GraphQL vs REST [use case]
```

### Error Handling

```
# Exception Handling
[language] error handling best practices
"global exception handler" [framework]
async error handling [language] 2024

# Logging
[framework] structured logging best practices
error logging context correlation
```

## Integration with Bug Resolution

Research integrates into the bug resolution workflow:

```
Bug Identified
     ↓
Root Cause Found
     ↓
Pattern Detection (find similar bugs)
     ↓
Complexity Check ──→ Low Complexity → Skip Research
     ↓ High
SOLUTION RESEARCH
     ↓
Synthesize Findings
     ↓
Apply to Fix (N + M patterns)
     ↓
Document for Future Reference
```

## Best Practices

1. **Time-box research**: Don't spend more than 10-15 minutes
2. **Verify with official docs**: Always cross-reference with official documentation
3. **Check dates**: Technology moves fast - prefer recent sources
4. **Test before trusting**: Code examples may not work in your context
5. **Document sources**: Future maintainers need to understand decisions
6. **Consider context**: "Best practice" depends on your specific situation

## Common Pitfalls

- **Analysis paralysis**: Too much research, no action
- **Outdated solutions**: Using old patterns for new problems
- **Over-engineering**: Applying enterprise patterns to simple problems
- **Ignoring context**: Copying solutions without understanding trade-offs
- **Single source**: Relying on one blog post without verification
- **Premature optimization**: Research for performance before measuring
