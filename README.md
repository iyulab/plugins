# Iyulab Claude Code Plugins

A collection of Claude Code plugins for library maintainers and developers.

## Installation

```bash
# Add the marketplace
/plugin marketplace add iyulab/plugins

# List available plugins
/plugin list

# Install the iyu plugin
/plugin install iyu@iyulab-plugins
```

## Available Plugins

### iyu

**Iyulab's productivity toolkit for library maintainers and developers**

A collection of commands for issue triage, workflow automation, and developer tools.

```bash
/plugin install iyu@iyulab-plugins
```

#### Commands

| Command | Description |
|---------|-------------|
| `/iyu:issue` | Systematic issue evaluation with 7-phase workflow |

#### /iyu:issue

Evaluate external issues against your project's philosophy and scope.

```bash
# Triage a GitHub issue
/iyu:issue https://github.com/user/repo/issues/123

# Triage a local document
/iyu:issue ./docs/feature-request.md

# Triage from text
/iyu:issue "User requests adding feature X..."
```

**Features**:
- Philosophy alignment scoring (1-5)
- Feasibility assessment
- 5-tier decision matrix (Accept/Adapt/Defer/Redirect/Decline)
- Knowledge capture for project learning
- Professional response draft generation

[Read more](./plugins/iyu/README.md)

## Philosophy

**"Every issue is an opportunity"** - Our plugins help maintainers move beyond simple accept/reject decisions to find the best path forward for their projects.

## Contributing

1. Fork this repository
2. Create your plugin in `plugins/your-plugin-name/`
3. Update `.claude-plugin/marketplace.json`
4. Submit a PR

## Plugin Structure

```
plugins/
└── your-plugin/
    ├── .claude-plugin/
    │   └── plugin.json       # Plugin metadata (required)
    ├── commands/             # Slash commands
    │   └── command-name.md
    ├── agents/               # Custom agents (optional)
    ├── skills/               # Agent skills (optional)
    └── README.md             # Documentation
```

## License

MIT - See [LICENSE](./LICENSE) for details.

## Links

- [Claude Code Documentation](https://docs.anthropic.com/en/docs/claude-code)
- [Plugin Development Guide](https://docs.anthropic.com/en/docs/claude-code/plugins)
- [Iyulab GitHub](https://github.com/iyulab)
