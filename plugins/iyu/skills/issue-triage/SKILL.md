---
name: Issue Triage
description: This skill should be used when the user asks to "triage an issue", "evaluate a feature request", "should I accept this issue", "analyze GitHub issue", "review this pull request scope", "is this in scope", "how should I respond to this issue", or discusses whether to accept, reject, or defer an external contribution to their project.
---

# Issue Triage Framework

A systematic approach for library maintainers to evaluate external issues against project philosophy and scope.

## Core Philosophy

**"Every issue is an opportunity"** - Even declined requests can improve documentation, reveal API gaps, or inspire better alternatives. The goal is not to accept or reject, but to find the best path forward.

## When to Apply This Framework

Apply this framework when:
- Evaluating GitHub/GitLab issues or feature requests
- Deciding whether a contribution fits project scope
- Responding to external pull requests
- Assessing bug reports vs. feature requests
- Determining library vs. application responsibility

## The Triage Process

### Step 1: Understand the Request

Before evaluating, fully understand what is being asked:

1. **Surface Request**: What are they literally asking for?
2. **Underlying Need**: What problem are they actually trying to solve?
3. **Use Case**: What is their specific scenario?
4. **Job to be Done**: What job is this feature being hired to do?

Ask: "Why can't they accomplish this with current capabilities?"

### Step 2: Assess Philosophy Alignment

Evaluate against four dimensions (score 1-5 each):

| Dimension | Question |
|-----------|----------|
| **Core Mission Fit** | Does this serve the project's primary purpose? |
| **Scope Alignment** | Is this library responsibility or application concern? |
| **Pattern Consistency** | Does it fit existing architecture and conventions? |
| **User Base Impact** | Does it benefit the majority or a niche use case? |

**Scoring Guide**:
- 5: Perfect fit, core to mission
- 4: Strong fit, natural extension
- 3: Acceptable, requires careful scoping
- 2: Marginal, stretches boundaries
- 1: Poor fit, conflicts with goals

Calculate average for overall alignment (High: 4-5, Medium: 3-3.9, Low: 1-2.9).

### Step 3: Assess Feasibility

Evaluate practical implementation factors:

| Factor | Rating Options |
|--------|----------------|
| Technical Complexity | Low / Medium / High |
| Breaking Changes | None / Minor / Major |
| Maintenance Burden | Low / Medium / High |
| Dependencies | None / Dev-only / Runtime |

Consider risks: What could go wrong? Impact on existing users?

### Step 4: Apply Decision Matrix

Use philosophy alignment and feasibility to determine verdict:

```
                 | Philosophy HIGH | Philosophy LOW  |
-----------------|-----------------|-----------------|
Feasibility HIGH | ACCEPT          | REDIRECT        |
Feasibility MED  | ADAPT           | DEFER/REDIRECT  |
Feasibility LOW  | DEFER           | DECLINE         |
```

**Decision Types**:

- **ACCEPT**: Fully aligned, implement as requested
- **ADAPT**: Good idea, implement differently than proposed
- **DEFER**: Valuable but not now; add to roadmap with conditions
- **REDIRECT**: Out of scope; provide alternative path (other library, extension point, workaround)
- **DECLINE**: Fundamentally misaligned; explain why respectfully

### Step 5: Craft Response

Every response should include:

1. **Acknowledge**: Thank them, show understanding of their need
2. **Explain**: Share reasoning transparently (reference project philosophy)
3. **Path Forward**: Always provide a constructive next step
4. **Invite**: Keep the door open for continued engagement

**Tone Guidelines**:
- Professional but warm
- Confident but not dismissive
- Educational - help them understand
- Grateful - they care about the project

## Quick Reference: Response Templates

**For ACCEPT**:
> Thank you! This aligns with [goal]. We'll implement it in [timeline]. PRs welcome!

**For ADAPT**:
> Great idea! We'd like to approach this differently: [explanation]. Would this work for you?

**For DEFER**:
> Valuable suggestion! We're prioritizing [focus] now. This is on our roadmap for [condition].

**For REDIRECT**:
> This falls outside our scope, but try: [alternative]. Here's why we maintain this boundary...

**For DECLINE**:
> After consideration, this doesn't align with [reason]. What we would welcome: [alternative].

## Key Questions to Ask

Before making a decision, consider:

1. Does this expand scope in a sustainable direction?
2. Would accepting create precedent for similar requests?
3. Is this library infrastructure or application logic?
4. What would the maintenance burden look like in 2 years?
5. Can this be achieved through extension points instead?

## Knowledge Capture

After each significant triage:

1. Update CLAUDE.md if scope clarification needed
2. Add FAQ entry if common question pattern
3. Consider ADR (Architecture Decision Record) for major decisions

## Additional Resources

### Reference Files

For detailed guidance, consult:
- **`references/decision-examples.md`** - Real-world decision examples with reasoning
- **`references/response-templates.md`** - Complete response templates for each decision type

### Full Workflow Command

For a complete formatted triage report with all phases, use the `/iyu:issue` command:
```bash
/iyu:issue <url | file | "text">
/iyu:issue <input> --quick    # Decision only, skip execution
/iyu:issue <input> --save     # Save report to file
```
