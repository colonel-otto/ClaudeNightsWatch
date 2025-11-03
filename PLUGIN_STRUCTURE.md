# Claude Code Plugin Structure

This document explains how the Claude Nights Watch plugin is structured to comply with Claude Code's plugin system requirements.

## Directory Layout

```
claude-nights-watch/
├── .claude-plugin/              # Plugin metadata directory
│   ├── plugin.json              # Plugin manifest (required)
│   ├── marketplace.json         # Marketplace metadata
│   ├── hooks.json               # Hook configurations
│   └── .mcp.json                # MCP server configurations
│
├── commands/                    # Slash commands (auto-discovered)
│   ├── start.md                 # /nights-watch start
│   ├── stop.md                  # /nights-watch stop
│   ├── status.md                # /nights-watch status
│   ├── logs.md                  # /nights-watch logs
│   ├── setup.md                 # /nights-watch setup
│   ├── restart.md               # /nights-watch restart
│   └── bin/
│       └── nights-watch         # Command wrapper script
│
├── agents/                      # Custom agents (auto-discovered)
│   └── task-executor.md         # Task Executor agent
│
├── hooks/                       # Hook implementation
│   └── scripts/
│       ├── check-daemon-status.sh
│       ├── session-end-prompt.sh
│       └── log-file-changes.sh
│
├── mcp-server/                  # MCP server implementation
│   └── nights-watch-server.sh
│
├── examples/                    # Example files
│   ├── task.example.md
│   └── rules.example.md
│
├── test/                        # Test suite
│   ├── test-plugin-comprehensive.sh
│   ├── test-simple.sh
│   └── [other test files]
│
├── [Core daemon scripts]
├── [Documentation files]
└── LICENSE
```

## Claude Code Plugin Requirements

### 1. Plugin Manifest Location

The manifest **must** be at `.claude-plugin/plugin.json` in the plugin root directory.

### 2. Auto-Discovery

Claude Code automatically discovers these directories at the plugin root:

- **`commands/`** - Each `.md` file becomes a slash command
- **`agents/`** - Each `.md` file defines a custom agent
- **`skills/`** - Subdirectories containing `SKILL.md` files

**You do NOT need to reference these in the manifest.** They are discovered automatically.

### 3. Path Rules

All paths in `plugin.json` must:

- Start with `./` (relative to `.claude-plugin/` directory)
- NOT use `../` to escape the plugin directory structure
- Reference files that exist within or below `.claude-plugin/`

✅ **Correct:**
```json
{
  "hooks": "./hooks.json",
  "mcpServers": "./.mcp.json"
}
```

❌ **Incorrect:**
```json
{
  "hooks": "../hooks/hooks.json",
  "mcpServers": "../.mcp.json"
}
```

### 4. Environment Variables

Configuration files can use `${CLAUDE_PLUGIN_ROOT}` to reference the plugin root directory:

**`.claude-plugin/hooks.json`:**
```json
{
  "hooks": {
    "SessionStart": [{
      "hooks": [{
        "type": "command",
        "command": "${CLAUDE_PLUGIN_ROOT}/hooks/scripts/check-daemon-status.sh"
      }]
    }]
  }
}
```

This allows config files in `.claude-plugin/` to reference scripts at the root level.

## Plugin Manifest (`plugin.json`)

### Minimal Required Fields

```json
{
  "name": "plugin-name",
  "version": "1.0.0",
  "description": "Plugin description",
  "author": {
    "name": "Author Name",
    "email": "[email protected]",
    "url": "https://github.com/username"
  }
}
```

### Optional Fields

- **`homepage`** - Plugin website URL
- **`repository`** - GitHub repository URL
- **`license`** - License identifier (e.g., "MIT")
- **`keywords`** - Array of keywords for discoverability
- **`hooks`** - Path to hooks configuration (relative to `.claude-plugin/`)
- **`mcpServers`** - Path to MCP configuration (relative to `.claude-plugin/`)

### What NOT to Include

Do not include these fields if using standard directories:

- ❌ `"commands": "./commands/"` - Auto-discovered from root
- ❌ `"agents": "./agents/"` - Auto-discovered from root
- ❌ `"skills": "./skills/"` - Auto-discovered from root

Only include custom paths if you're supplementing the default directories.

## Command Structure

Each command file is a Markdown document with frontmatter:

```markdown
---
description: Brief description of what this command does
---

# Command Instructions

Detailed instructions for Claude on how to execute this command.

You can include:
- Variable placeholders
- Bash commands to run
- File operations
- Multi-step procedures
```

Commands are automatically namespaced: `/plugin-name:command-name`

## Agent Structure

Agent files are Markdown documents that define specialized agents:

```markdown
---
name: Agent Name
description: What this agent does
---

# Agent Instructions

Detailed instructions for the agent's behavior, expertise, and responsibilities.
```

## Hooks Configuration

Hooks allow you to respond to Claude Code events:

```json
{
  "hooks": {
    "SessionStart": [{ ... }],
    "SessionEnd": [{ ... }],
    "PreToolUse": [{ ... }],
    "PostToolUse": [{ ... }],
    "UserPromptSubmit": [{ ... }]
  }
}
```

Each hook can execute commands or scripts using `${CLAUDE_PLUGIN_ROOT}`.

## MCP Server Configuration

MCP servers provide external tools to Claude:

```json
{
  "mcpServers": {
    "server-name": {
      "command": "${CLAUDE_PLUGIN_ROOT}/mcp-server/server-script.sh",
      "env": {
        "SERVER_ROOT": "${CLAUDE_PLUGIN_ROOT}"
      }
    }
  }
}
```

## Common Mistakes

### ❌ Using `../` in manifest paths

```json
{
  "hooks": "../hooks/hooks.json"  // FAILS: cannot escape plugin directory
}
```

**Fix:** Move the file into `.claude-plugin/` and use `./` path.

### ❌ Specifying auto-discovered directories

```json
{
  "commands": "./commands/",  // UNNECESSARY: auto-discovered
  "agents": "./agents/"       // UNNECESSARY: auto-discovered
}
```

**Fix:** Remove these fields. They're discovered automatically from root.

### ❌ Incorrect directory structure

```
plugin/
└── .claude-plugin/
    ├── plugin.json
    ├── commands/        // WRONG LOCATION
    └── agents/          // WRONG LOCATION
```

**Fix:** Move `commands/` and `agents/` to the plugin root level.

## Validation

To verify your plugin structure:

1. **Check directory layout:**
   ```bash
   ls -la .claude-plugin/  # Should contain plugin.json
   ls -la commands/        # Should contain *.md files
   ls -la agents/          # Should contain *.md files
   ```

2. **Verify manifest paths:**
   ```bash
   cd .claude-plugin
   cat plugin.json | jq '.hooks, .mcpServers'
   # All paths should start with ./
   ```

3. **Test config file references:**
   ```bash
   grep -r "CLAUDE_PLUGIN_ROOT" .claude-plugin/*.json
   # Should see ${CLAUDE_PLUGIN_ROOT} in paths
   ```

## Testing

Run the plugin verification script:

```bash
./verify-plugin.sh
```

Or use the comprehensive test suite:

```bash
cd test
./test-plugin-comprehensive.sh
```

## Documentation References

- [Claude Code Plugins](https://docs.claude.com/en/docs/claude-code/plugins)
- [Plugin Reference](https://docs.claude.com/en/docs/claude-code/plugins-reference)
- [Slash Commands](https://docs.claude.com/en/docs/claude-code/slash-commands)
- [Hooks Guide](https://docs.claude.com/en/docs/claude-code/hooks)
- [MCP Integration](https://docs.claude.com/en/docs/claude-code/mcp)

## Need Help?

If your plugin isn't loading:

1. Check the error message for specific validation failures
2. Verify your directory structure matches this guide
3. Ensure all paths in `plugin.json` start with `./`
4. Confirm config files use `${CLAUDE_PLUGIN_ROOT}` for root references
5. Run `./verify-plugin.sh` to catch common issues

For more help, see the [Contributing Guide](CONTRIBUTING.md) or open an issue.
