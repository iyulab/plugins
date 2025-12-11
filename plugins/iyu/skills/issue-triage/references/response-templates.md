# Issue Response Templates

Complete response templates for each decision type, ready to customize.

## ACCEPT Response Template

```markdown
Thank you for this suggestion! [Demonstrate understanding of their need]

This aligns well with [specific project goal/philosophy], and we're happy to move forward with it.

**Implementation plan:**
- [Timeline or milestone]
- [Any scope adjustments if needed]

[If applicable: PRs are welcome if you'd like to contribute!]

We'll update this issue when [milestone]. Thanks for helping improve [project name]!
```

### ACCEPT Variations

**Quick acceptance (obvious fit)**:
```markdown
Great idea - this is exactly the kind of [improvement/feature] we want.

Implemented in [version/PR]. Thanks for the suggestion!
```

**Acceptance with scope adjustment**:
```markdown
Thank you for this! We love the core idea.

We'll implement [slightly adjusted version] because [reason]. This achieves your goal while [benefit of adjustment].

Targeting [version/milestone].
```

**Acceptance inviting contribution**:
```markdown
This is a great fit for [project]. We'd love to include it!

If you're interested in contributing:
- [Pointer to relevant code]
- [Key considerations]
- [Link to contributing guide]

Otherwise, we'll add this to our queue for [timeline].
```

---

## ADAPT Response Template

```markdown
Thank you for raising this! [Show you understand the underlying need]

We'd like to address this slightly differently than proposed:

**Our approach:** [Explain the adapted solution]

**Why this works better:**
- [Benefit 1]
- [Benefit 2]
- [How it still solves their problem]

Would this work for your use case? Happy to discuss further if you have concerns.
```

### ADAPT Variations

**Technical alternative**:
```markdown
Interesting idea! The approach you suggested would [limitation], so we're thinking:

Instead of:
[Their proposed approach]

We'd do:
[Our alternative]

This gives you [same benefit] while [avoiding limitation]. Thoughts?
```

**Scope reduction**:
```markdown
Thank you for this comprehensive suggestion!

We'd like to start with a focused version:
- [Core feature we'll implement]
- [What we're deferring for now]

This covers your main use case while keeping the API surface manageable. We can expand based on feedback.
```

---

## DEFER Response Template

```markdown
Thank you for this thoughtful suggestion! [Show understanding]

This is valuable, but we're currently focused on [current priority]. Here's our thinking:

**Why not now:**
- [Reason 1 - be specific and honest]
- [Reason 2 if applicable]

**Roadmap placement:** [Where this fits - milestone, version, or conditions]

**What would accelerate this:**
- [Community interest/votes]
- [Sponsored development]
- [Community PR]

We've added the `[label]` label to track this. Feel free to add more use cases or 👍 to help us prioritize.
```

### DEFER Variations

**Waiting for more input**:
```markdown
Interesting idea! We'd like to gather more feedback before committing.

**Questions for the community:**
- How would you use this?
- What's your current workaround?
- Priority vs. other requested features?

Adding to our discussion backlog. More input helps us design the right solution.
```

**Resource-constrained**:
```markdown
We agree this would be valuable! Currently, our bandwidth is focused on [priority].

**Options:**
1. Wait for our team to get to this (~[timeline])
2. Community contribution (we'll review and guide)
3. Sponsorship for prioritized development

Which works best for you?
```

---

## REDIRECT Response Template

```markdown
Thank you for reaching out! [Show understanding of the need]

This falls outside our current scope - we focus on [our focus], not [what they're asking for].

**Why we maintain this boundary:**
- [Philosophy reason]
- [Practical reason]

**Alternatives that might help:**

1. **[Tool/Library 1]**: [How it solves their need]
2. **[Workaround using our tool]**: [How to achieve similar result]
3. **[Extension point]**: [How they could build this on top of us]

[If applicable: See our guide on [topic] for more details.]

Would any of these work for your use case? Happy to help you evaluate options.
```

### REDIRECT Variations

**To another project**:
```markdown
Great idea, but this is really [other tool]'s territory!

[Other tool] handles [this concern] well. You can use it alongside [our tool]:
- [How they work together]
- [Example if helpful]

We're happy to improve our [integration/interoperability] if you run into friction.
```

**To extension/plugin**:
```markdown
This is a great use case, but it's specialized enough that we'd keep it out of core.

Good news: our [extension/plugin/middleware] system supports this!

Here's how you could implement it:
```[code example]```

We could add this as an official example/recipe if useful. Would that help?
```

---

## DECLINE Response Template

```markdown
Thank you for taking the time to suggest this! [Show you understand what they want]

After careful consideration, we've decided this doesn't align with [project]'s direction. Here's our reasoning:

**Our concern:**
- [Primary reason - be specific and respectful]
- [How this conflicts with project philosophy if relevant]

**What we would welcome instead:**
- [Alternative contribution that would be accepted]
- [Related area where they could contribute]

We appreciate your interest in improving [project]. Even though we can't move forward with this specific suggestion, we hope you'll continue engaging with the project.
```

### DECLINE Variations

**Philosophical misalignment**:
```markdown
Thank you for the suggestion! We've given this serious thought.

This conflicts with our core philosophy of [principle]. We believe [explanation of why we hold this view].

**The trade-off:**
- Your suggestion optimizes for [X]
- We've chosen to optimize for [Y]
- These goals are in tension here

**What we'd welcome:**
- [Contribution that aligns with our philosophy]
- [Discussion about the philosophy itself if they want to challenge it]

We know this might be disappointing. Happy to discuss our reasoning further if helpful.
```

**Already considered and rejected**:
```markdown
Thank you for raising this! We've actually discussed this before - see [link to previous discussion/ADR].

**Our decision:** [Brief summary]
**Key reasons:** [Main factors]

If you have new information that changes the calculus, we're open to revisiting. Otherwise, we'll keep with our current approach.
```

**Precedent concern**:
```markdown
Interesting idea! Our concern is the precedent this sets.

If we add [this], we'd need to consider:
- [Similar request 1]
- [Similar request 2]

This would expand our scope in ways we're not ready for.

**Alternative:**
- [How to achieve this outside our project]
- [Or: what scoped version we might accept]
```

---

## Universal Response Elements

### Opening Lines (Vary These)

- "Thank you for this suggestion!"
- "Thanks for raising this!"
- "Appreciate you taking the time to write this up!"
- "Great question/idea!"
- "Thank you for the detailed proposal!"
- "Thanks for thinking about [project]'s future!"

### Closing Lines (Vary These)

- "Let us know if you have questions!"
- "Happy to discuss further."
- "Thanks again for your contribution to [project]!"
- "Looking forward to your feedback."
- "Feel free to reopen if circumstances change."
- "We appreciate your understanding."

### Tone Checklist

Before posting any response, verify:

- [ ] **Grateful**: Acknowledged their effort
- [ ] **Understanding**: Showed you understood their need
- [ ] **Transparent**: Explained reasoning clearly
- [ ] **Constructive**: Provided a path forward
- [ ] **Respectful**: Professional even in disagreement
- [ ] **Inviting**: Left door open for engagement
