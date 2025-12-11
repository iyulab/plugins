# IYU Plugin

Iyulab's productivity toolkit for library maintainers and developers.

## Philosophy

**"Every issue is an opportunity"** - This plugin helps you move beyond simple accept/reject decisions to find the best path forward for your project.

## Installation

```bash
# Add the marketplace
/plugin marketplace add iyulab/plugins

# Install the plugin
/plugin install iyu@iyulab-plugins
```

## Features

This plugin provides two ways to help with issue triage:

| Component | Activation | Use Case |
|-----------|------------|----------|
| **Skill** | Automatic | Conversational issue analysis and advice |
| **Command** | Manual (`/iyu:issue`) | Full formatted triage report |

## Skills (Auto-Activated)

### Issue Triage

The plugin automatically activates when you discuss issue evaluation. Just ask naturally:

- "Should I accept this feature request?"
- "How should I respond to this issue?"
- "Is this in scope for my project?"
- "Evaluate this GitHub issue for me"
- "Help me triage this pull request"

Claude will automatically apply the triage framework:
1. Understand the request (surface vs. underlying need)
2. Assess philosophy alignment (1-5 scoring)
3. Evaluate feasibility
4. Apply decision matrix
5. Suggest response approach

## Commands

### /iyu:issue

**Systematic issue evaluation with full formatted report**

Use this command when you need a complete, structured triage report.

```bash
# Triage a GitHub/GitLab issue
/iyu:issue https://github.com/user/repo/issues/123

# Quick decision only (skip execution phases)
/iyu:issue https://github.com/user/repo/issues/123 --quick

# Triage and save report to file
/iyu:issue ./docs/feature-request.md --save

# Triage from pasted text
/iyu:issue "User requests adding feature X..."
```

**Workflow**:

```
INTAKE -> PHILOSOPHY -> FEASIBILITY -> DECISION -> EXECUTION -> KNOWLEDGE -> RESPONSE
```

**Decision Matrix**:

| | Philosophy Aligned | Philosophy Misaligned |
|---|---|---|
| **High Feasibility** | ACCEPT | REDIRECT |
| **Medium Feasibility** | ADAPT | DEFER / REDIRECT |
| **Low Feasibility** | DEFER | DECLINE |

**Decision Types**:
- **ACCEPT**: Fully aligned, implement as requested
- **ADAPT**: Good idea, implement differently
- **DEFER**: Valuable but not now, add to roadmap
- **REDIRECT**: Out of scope, provide alternatives
- **DECLINE**: Fundamentally misaligned, explain why

**Output**:
1. **Triage Report**: Complete analysis with scores and reasoning
2. **Knowledge Updates**: Project documentation updates
3. **Response Draft**: Professional reply ready to post

## Best Practices

1. **Document your project philosophy**: Ensure CLAUDE.md describes your project's core principles
2. **Be honest**: The plugin encourages transparent reasoning
3. **Stay constructive**: Even declined requests get alternative suggestions
4. **Capture learning**: Each issue improves project documentation

## License

MIT
