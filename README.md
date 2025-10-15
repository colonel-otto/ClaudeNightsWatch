# Claude Nights Watch 🌙

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Shell Script](https://img.shields.io/badge/Language-Shell-green.svg)](https://www.gnu.org/software/bash/)
[![Claude Code Plugin](https://img.shields.io/badge/Plugin-Ready-blue.svg)](https://docs.anthropic.com/claude-code)
[![GitHub stars](https://img.shields.io/github/stars/aniketkarne/ClaudeNightsWatch.svg?style=social&label=Star)](https://github.com/aniketkarne/ClaudeNightsWatch)

**🚀 NEW: Now available as a Claude Code Plugin!** 🎉

Autonomous task execution system for Claude CLI that monitors your usage windows and executes predefined tasks automatically. Built on top of the claude-auto-renew concept but instead of simple renewals, it executes complex tasks from a task.md file.

**⚠️ Warning**: This tool uses `--dangerously-skip-permissions` for autonomous execution. Use with caution!

## 🎯 Two Ways to Use Claude Nights Watch

Choose your preferred installation method:

### 🔥 **Recommended: Claude Code Plugin** (NEW!)
- **Seamless Integration**: Works directly within Claude Code
- **7 Slash Commands**: `/nights-watch start/stop/status/logs/task/setup/restart`
- **AI Agent Integration**: Built-in Task Executor agent for autonomous guidance
- **Smart Hooks**: Automatic session integration and activity logging
- **MCP Server**: Programmatic control tools for Claude
- **Enhanced UX**: Better error handling and user experience

### ⚡ **Original Daemon Method** (Legacy)
- **Standalone Operation**: Works independently of Claude Code
- **Script-based Control**: Direct shell script execution
- **Proven Reliability**: Battle-tested daemon implementation
- **Full Feature Set**: All core functionality available

## 🎯 Overview

Claude Nights Watch extends the auto-renewal concept to create a fully autonomous task execution system. When your Claude usage window is about to expire, instead of just saying "hi", it reads your `task.md` file and executes the defined tasks autonomously.

### Key Features

- 🤖 **Autonomous Execution**: Runs tasks without manual intervention
- 📋 **Task-Based Workflow**: Define tasks in a simple markdown file
- 🛡️ **Safety Rules**: Configure safety constraints in `rules.md`
- ⏰ **Smart Timing**: Uses ccusage for accurate timing or falls back to time-based checking
- 📅 **Scheduled Start**: Can be configured to start at a specific time
- 📊 **Comprehensive Logging**: Track all activities and executions
- 🔄 **Based on Proven Code**: Built on the reliable claude-auto-renew daemon

## 🚀 Quick Start

### Prerequisites

1. [Claude CLI](https://docs.anthropic.com/en/docs/claude-code/quickstart) installed and configured
2. (Optional) [ccusage](https://www.npmjs.com/package/ccusage) for accurate timing:
   ```bash
   npm install -g ccusage
   ```

---

## 🔥 Plugin Installation (Recommended)

### Option 1: As Claude Code Plugin
#### Standard Installation 
```bash
# Method 1: From marketplace (when available)
claude plugins marketplace add https://github.com/aniketkarne/claude-plugins-marketplace
claude plugins add claude-nights-watch

# Method 2: Direct from GitHub
claude plugins add https://github.com/aniketkarne/ClaudeNightsWatch

# Method 3: From local directory (for development)
claude plugins add /path/to/ClaudeNightsWatch
```
#### One-Command Installation
Use the [Claude Plugins CLI](https://claude-plugins.dev) to skip the marketplace setup:
```bash
npx claude-plugins install @aniketkarne/claude-nights-watch-marketplace/claude-nights-watch
```

This automatically adds the marketplace and installs the plugin in a single step.

### Plugin Usage

```bash
# Interactive setup
/nights-watch setup

# Start daemon
/nights-watch start

# Check status
/nights-watch status

# View logs in real-time
/nights-watch logs -f

# Stop daemon
/nights-watch stop
```

**Plugin Features:**
- 🎯 **7 Slash Commands**: `/nights-watch start/stop/status/logs/task/setup/restart`
- 🧠 **AI Agent**: Built-in Task Executor for autonomous guidance
- 🔗 **MCP Server**: 8 programmatic tools for Claude control
- 🎣 **Smart Hooks**: Automatic session integration

---

## ⚡ Original Daemon Installation (Legacy)

### Option 2: Install as Standalone Daemon

1. Clone this repository:
   ```bash
   git clone https://github.com/aniketkarne/ClaudeNightsWatch.git
   cd ClaudeNightsWatch
   ```

2. Make scripts executable:
   ```bash
   chmod +x *.sh
   ```

3. Run the interactive setup:
   ```bash
   ./setup-nights-watch.sh
   ```

### Daemon Usage

1. **Create your task file** (`task.md`):
   ```markdown
   # Daily Development Tasks

   1. Run linting on all source files
   2. Update dependencies to latest versions
   3. Run the test suite
   4. Generate coverage report
   5. Create a summary of changes
   ```

2. **Create safety rules** (`rules.md`):
   ```markdown
   # Safety Rules

   - Never delete files without backing up
   - Only work within the project directory
   - Always create feature branches for changes
   - Never commit sensitive information
   ```

3. **Start the daemon**:
   ```bash
   ./claude-nights-watch-manager.sh start
   ```

**Legacy Commands:**
```bash
./claude-nights-watch-manager.sh start [--at TIME]  # Start daemon
./claude-nights-watch-manager.sh stop              # Stop daemon
./claude-nights-watch-manager.sh status            # Check status
./claude-nights-watch-manager.sh logs [-f]         # View logs
./claude-nights-watch-manager.sh task              # View task/rules
./claude-nights-watch-manager.sh restart           # Restart daemon
```

## 📝 Configuration

### Task File (task.md)

The task file contains the instructions that Claude will execute. It should be clear, specific, and well-structured. See `examples/task.example.md` for a comprehensive example.

### Rules File (rules.md)

The rules file defines safety constraints and best practices. It's prepended to every task execution to ensure safe autonomous operation. See `examples/rules.example.md` for recommended rules.

### Environment Variables

- `CLAUDE_NIGHTS_WATCH_DIR`: Set the directory containing task.md and rules.md (default: current directory)

## 🎮 Commands

### 🔥 Plugin Commands (NEW!)

When using as a Claude Code plugin, use slash commands:

```bash
# Start the daemon
/nights-watch start

# Start with scheduled time
/nights-watch start --at "09:00"
/nights-watch start --at "2025-01-28 14:30"

# Stop the daemon
/nights-watch stop

# Check status
/nights-watch status

# View logs
/nights-watch logs
/nights-watch logs -f  # Follow mode

# View current task and rules
/nights-watch task

# Interactive setup
/nights-watch setup

# Restart daemon
/nights-watch restart
```

### ⚡ Original Daemon Commands (Legacy)

For standalone daemon usage:

```bash
# Start the daemon
./claude-nights-watch-manager.sh start

# Start with scheduled time
./claude-nights-watch-manager.sh start --at "09:00"
./claude-nights-watch-manager.sh start --at "2025-01-28 14:30"

# Stop the daemon
./claude-nights-watch-manager.sh stop

# Check status
./claude-nights-watch-manager.sh status

# View logs
./claude-nights-watch-manager.sh logs
./claude-nights-watch-manager.sh logs -f  # Follow mode

# Use interactive log viewer
./view-logs.sh

# View current task and rules
./claude-nights-watch-manager.sh task

# Restart daemon
./claude-nights-watch-manager.sh restart
```

---

## 🧠 Plugin Features (NEW!)

### Task Executor Agent
Built-in AI agent that helps with:
- **Autonomous Task Planning**: Designing effective autonomous workflows
- **Safety Rule Creation**: Building comprehensive safety constraints
- **Daemon Management**: Starting, stopping, and troubleshooting
- **Log Analysis**: Interpreting execution results and identifying issues

**Usage:**
```
User: "Help me create an autonomous code review workflow"
Agent: Provides expert guidance and can execute /nights-watch commands

User: "My daemon isn't working, help debug"
Agent: Analyzes logs and suggests solutions
```

### MCP Server Integration
Provides 8 programmatic tools for Claude:
- `get_daemon_status` - Query daemon state
- `start_daemon` - Start with optional schedule
- `stop_daemon` - Stop the daemon
- `get_logs` - Retrieve log entries
- `read_task` - Read task.md content
- `read_rules` - Read rules.md content
- `write_task` - Update task.md
- `write_rules` - Update rules.md

### Smart Hooks
Automatic integration with Claude Code sessions:
- **SessionStart**: Shows daemon status when starting Claude Code
- **SessionEnd**: Prompts to start daemon if tasks are configured
- **PostToolUse**: Logs file modifications for audit trail

These hooks run silently in the background, enhancing your workflow.

## 🔧 How It Works

1. **Monitoring**: The daemon continuously monitors your Claude usage windows
2. **Timing**: When approaching the 5-hour limit (within 2 minutes), it prepares for execution
3. **Task Preparation**: Reads both `rules.md` and `task.md`, combining them into a single prompt
4. **Autonomous Execution**: Executes the task using `claude --dangerously-skip-permissions`
5. **Logging**: All activities are logged to `logs/claude-nights-watch-daemon.log`

### Timing Logic

- **With ccusage**: Gets accurate remaining time from the API
- **Without ccusage**: Falls back to timestamp-based checking
- **Adaptive intervals**:
  - \>30 minutes remaining: Check every 10 minutes
  - 5-30 minutes remaining: Check every 2 minutes
  - <5 minutes remaining: Check every 30 seconds

## ⚠️ Safety Considerations

**IMPORTANT**: This tool runs Claude with the `--dangerously-skip-permissions` flag, meaning it will execute tasks without asking for confirmation. 

### Best Practices:

1. **Always test tasks manually first** before setting up autonomous execution
2. **Use comprehensive rules.md** to prevent destructive actions
3. **Start with simple, safe tasks** and gradually increase complexity
4. **Monitor logs regularly** to ensure proper execution
5. **Keep backups** of important data
6. **Run in isolated environments** when possible

### Recommended Restrictions:

- Limit file system access to project directories
- Prohibit deletion commands
- Prevent system modifications
- Restrict network access
- Set resource limits

## 📁 File Structure

### 🔥 Plugin Structure (NEW!)

When installed as a Claude Code plugin, the following structure is used:

```
claude-nights-watch/
├── .claude-plugin/                    # Plugin metadata (NEW!)
│   └── plugin.json                   # Plugin manifest
├── commands/                         # Slash command definitions (NEW!)
│   ├── bin/nights-watch             # Command wrapper
│   ├── start.md                     # Start command documentation
│   ├── stop.md                      # Stop command documentation
│   ├── status.md                    # Status command documentation
│   ├── logs.md                      # Logs command documentation
│   ├── task.md                      # Task command documentation
│   ├── setup.md                     # Setup command documentation
│   └── restart.md                   # Restart command documentation
├── agents/                           # AI agents (NEW!)
│   └── task-executor.md             # Autonomous task planning agent
├── hooks/                            # Event handlers (NEW!)
│   ├── hooks.json                   # Hook configuration
│   └── scripts/                     # Hook implementation scripts
│       ├── check-daemon-status.sh   # Session start hook
│       ├── session-end-prompt.sh    # Session end hook
│       └── log-file-changes.sh      # File modification hook
├── mcp-server/                       # Model Context Protocol (NEW!)
│   └── nights-watch-server.sh       # MCP server implementation
├── .mcp.json                        # MCP server configuration (NEW!)
└── [original files continue below...]
```

### ⚡ Original Daemon Structure (Legacy)

The original standalone daemon structure:

```
claude-nights-watch/
├── claude-nights-watch-daemon.sh      # Core daemon process
├── claude-nights-watch-manager.sh     # Daemon management interface
├── setup-nights-watch.sh              # Interactive setup script
├── view-logs.sh                       # Interactive log viewer
├── README.md                          # This file
├── LICENSE                            # MIT License
├── CONTRIBUTING.md                    # Contribution guidelines
├── CHANGELOG.md                       # Version history
├── SUMMARY.md                         # Project summary
├── .gitignore                         # Git ignore file
├── .github/                           # GitHub templates
│   ├── ISSUE_TEMPLATE/
│   │   ├── bug_report.md
│   │   └── feature_request.md
│   └── pull_request_template.md
├── logs/                              # All logs stored here (created on first run)
├── examples/                          # Example files
│   ├── task.example.md                # Example task file
│   └── rules.example.md               # Example rules file
└── test/                              # Test scripts and files
    ├── README.md                      # Testing documentation
    ├── test-immediate-execution.sh    # Direct task execution test
    ├── test-simple.sh                 # Simple functionality test
    ├── test-task-simple.md            # Simple test task
    ├── test-rules-simple.md           # Simple test rules
    ├── test-plugin-comprehensive.sh   # Plugin functionality tests (NEW!)
    └── test-functional-realworld.sh   # Real-world testing suite (NEW!)
```

## 📊 Logging

All logs are stored in the `logs/` directory within the project. Each log contains:

- **Timestamps**: Every action is timestamped
- **Full Prompts**: Complete prompt sent to Claude (rules + task)
- **Full Responses**: Everything Claude outputs
- **Status Messages**: Success/failure indicators

### Viewing Logs

Use the interactive log viewer:
```bash
./view-logs.sh
```

Features:
- Browse all log files
- View full logs or last 50 lines
- Filter to see only prompts sent to Claude
- Filter to see only Claude's responses
- Search for errors
- Follow logs in real-time

## 🧪 Testing

### Plugin Testing (NEW!)

Comprehensive test suites for the Claude Code plugin:

```bash
cd test

# Run comprehensive plugin structure tests
./test-plugin-comprehensive.sh

# Run functional and real-world testing
./test-functional-realworld.sh

# Run immediate execution test (no waiting)
./test-immediate-execution.sh

# Run simple functionality test
./test-simple.sh
```

### Original Daemon Testing (Legacy)

```bash
cd test
./test-simple.sh  # Run a simple test
```

See `test/README.md` for detailed testing instructions.

**New Testing Features:**
- **Plugin Structure Validation**: Tests all plugin components
- **Functional Testing**: Validates script execution and integration
- **Real-World Simulation**: Tests complete user workflows
- **Environment Testing**: Validates plugin environment variables
- **Error Handling**: Tests graceful failure scenarios

## 🐛 Troubleshooting

### Plugin Issues (NEW!)

**Plugin not loading:**
```bash
# Check plugin installation
claude plugins list | grep claude-nights-watch

# Reinstall if needed
claude plugins remove claude-nights-watch
claude plugins add https://github.com/aniketkarne/ClaudeNightsWatch

# Debug mode
claude --debug
```

**Commands not working:**
```bash
# Verify command scripts are executable
ls -la ~/.claude/plugins/claude-nights-watch/commands/bin/

# Make executable if needed
chmod +x ~/.claude/plugins/claude-nights-watch/commands/bin/nights-watch
```

**Agent not responding:**
- Ensure plugin is properly installed
- Check that agent file exists in `agents/task-executor.md`
- Verify MCP server is running

### Original Daemon Issues (Legacy)

**Daemon won't start:**
- Check if Claude CLI is installed: `which claude`
- Verify task.md exists in the working directory
- Check logs: `./claude-nights-watch-manager.sh logs`

**Tasks not executing:**
- Verify you have remaining Claude usage: `ccusage blocks`
- Check if past scheduled start time
- Ensure task.md is not empty
- Review logs for errors

**Timing issues:**
- Install ccusage for better accuracy: `npm install -g ccusage`
- Check system time is correct
- Verify `.claude-last-activity` timestamp

### Common Issues (Both Methods)

**Permission errors:**
```bash
# Ensure all scripts are executable
chmod +x *.sh
chmod +x commands/bin/nights-watch
chmod +x hooks/scripts/*.sh
chmod +x mcp-server/*.sh
```

**Missing dependencies:**
- Install Claude CLI: https://docs.anthropic.com/claude-code
- Install ccusage: `npm install -g ccusage` (recommended)
- Verify bash is available: `which bash`

**Configuration issues:**
- Check task.md exists and has content
- Verify rules.md exists (recommended)
- Ensure proper file permissions
- Check environment variables

## 🤝 Contributing

Contributions are welcome! Please follow these steps:

1. **Fork the repository** on GitHub
2. **Clone your fork** locally
3. **Create a feature branch** (`git checkout -b feature/amazing-feature`)
4. **Make your changes** following our guidelines
5. **Test thoroughly** using the test suite
6. **Commit your changes** (`git commit -m 'Add amazing feature'`)
7. **Push to your fork** (`git push origin feature/amazing-feature`)
8. **Create a Pull Request** on GitHub

Please ensure:
- Code follows existing style
- Safety is prioritized  
- Documentation is updated
- Examples are provided
- Tests pass

See [CONTRIBUTING.md](CONTRIBUTING.md) for detailed guidelines.

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- **Created by**: [Aniket Karne](https://github.com/aniketkarne)
- **Built on top of**: The excellent [CCAutoRenew](https://github.com/aniketkarne/CCAutoRenew) project
- **Plugin Development**: Complete Claude Code plugin implementation with slash commands, AI agents, MCP server, and smart hooks
- **Enhanced Features**: Task Executor agent, comprehensive testing suites, and dual installation methods
- **Thanks to**: The Claude CLI team for the amazing tool and plugin system

---

---

## 🔄 Migration Guide

### From Daemon to Plugin

If you're currently using the standalone daemon and want to migrate to the plugin:

1. **Install the plugin**:
   ```bash
   claude plugins add https://github.com/aniketkarne/ClaudeNightsWatch
   ```

2. **Copy your existing files**:
   ```bash
   # Copy your task.md and rules.md to the new location
   cp task.md rules.md ~/.claude/plugins/claude-nights-watch/
   ```

3. **Update your workflow**:
   - Replace `./claude-nights-watch-manager.sh start` with `/nights-watch start`
   - Replace `./claude-nights-watch-manager.sh logs` with `/nights-watch logs`
   - Use `/nights-watch setup` for configuration

4. **Benefits you'll gain**:
   - 🤖 **AI Agent Integration**: Get help from the Task Executor agent
   - 🎯 **Better Commands**: More intuitive slash command interface
   - 🔗 **MCP Integration**: Programmatic control for Claude
   - 🎣 **Smart Hooks**: Automatic session integration

### From Plugin to Daemon

If you prefer the standalone approach:

1. **Install as daemon**:
   ```bash
   git clone https://github.com/aniketkarne/ClaudeNightsWatch.git
   cd ClaudeNightsWatch
   chmod +x *.sh
   ```

2. **Copy your plugin files**:
   ```bash
   # Copy from plugin location to daemon location
   cp ~/.claude/plugins/claude-nights-watch/task.md .
   cp ~/.claude/plugins/claude-nights-watch/rules.md .
   ```

3. **Use daemon commands**:
   ```bash
   ./claude-nights-watch-manager.sh start
   ```

**Note**: The plugin method is recommended for new users due to better integration and enhanced features.

---

**Remember**: With great automation comes great responsibility. Always review your tasks and rules carefully before enabling autonomous execution! 🚨
