# yukyu-cc-marketplace

Claude Code plugin marketplace by yukyu30.

## Directory Structure

```
.
├── .claude-plugin/
│   └── marketplace.json      # Marketplace metadata
├── plugins/
│   └── example-plugin/       # Example plugin
│       ├── .claude-plugin/
│       │   └── plugin.json   # Plugin metadata
│       ├── commands/         # Slash commands
│       │   ├── hello.md
│       │   └── summarize.md
│       ├── agents/           # Custom agents
│       │   ├── security-reviewer.md
│       │   └── test-generator.md
│       └── skills/           # Agent skills
│           └── code-review/
│               └── SKILL.md
└── README.md
```

## Installation

### Add the marketplace

```bash
/plugin marketplace add yukyu30/yukyu-cc-marketplace
```

Or for local development:

```bash
/plugin marketplace add ./path/to/yukyu-cc-marketplace
```

### Install a plugin

```bash
/plugin install example-plugin@yukyu-cc-marketplace
```

## Available Plugins

### example-plugin

A demonstration plugin with sample commands, agents, and skills.

**Commands:**
- `/hello` - Greet the user
- `/summarize` - Summarize content

**Agents:**
- `security-reviewer` - Security-focused code reviewer
- `test-generator` - TDD test case generator

**Skills:**
- `code-review` - Code review best practices

## Creating New Plugins

1. Create a new directory under `plugins/`
2. Add `.claude-plugin/plugin.json` with plugin metadata
3. Add commands to `commands/` directory
4. Add agents to `agents/` directory
5. Add skills to `skills/` directory
6. Update `marketplace.json` to include the new plugin

## Validation

```bash
claude plugin validate .
```

## License

MIT
