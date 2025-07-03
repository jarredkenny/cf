# cf - Claude Flow SPARC Launcher

A convenient command-line tool for launching Claude Flow SPARC sessions with integrated git worktree management and tmux support.

[claude-flow](https://github.com/ruvnet/claude-code-flow)

[tmux](https://github.com/tmux/tmux/wiki)

## Features

- 🚀 **Quick SPARC Sessions**: Launch Claude Flow SPARC with prompts or interactively
- 🌿 **Git Worktree Integration**: Automatically create feature branches and worktrees
- 📺 **Tmux Window Support**: Run sessions in dedicated tmux windows
- ⚙️ **Configurable**: Customize editor, paths, and naming conventions
- 🔧 **Smart Dependencies**: Auto-detects and guides installation of required tools

## Installation

### Prerequisites

- **Node.js & npm/npx** - For claude-flow installation
- **git** - For version control and worktree management
- **tmux** - Optional, only needed for `--window` option

### Quick Setup

1. Make the script executable:
   ```bash
   chmod +x cf
   ```

2. Place it in your PATH:
   ```bash
   # Example: copy to a directory in your PATH
   cp cf ~/.local/bin/cf
   # or
   sudo cp cf /usr/local/bin/cf
   ```

3. Run it - it will automatically initialize claude-flow if needed:
   ```bash
   cf
   ```

## Usage

### Basic Usage

```bash
# Interactive mode - opens editor for prompt
cf

# Direct prompt mode
cf "Build a REST API for user authentication"

# Show help
cf --help
```

### Git Worktree Integration

```bash
# Create feature branch and worktree
cf --worktree auth "Implement user authentication"

# This creates:
# - Branch: feature/auth
# - Worktree: ./worktrees/auth/
# - Runs claude-flow in the worktree directory
```

### Tmux Window Mode

```bash
# Run in new tmux window (requires active tmux session)
cf --window api "Build REST API endpoints"

# Combine with worktree
cf --window auth --worktree auth "User authentication system"
```

### Configuration

Create a configuration file at one of these locations:
- `./.cf/config` (project-specific)
- `~/.cf/config` (user-specific)
- `~/.config/cf/config` (XDG config directory)

#### Configuration Options

```bash
# ~/.cf/config
CF_EDITOR="code"                    # Override default editor
CF_WORKTREE_DIR="./workspace"       # Change worktree location
CF_BRANCH_PREFIX="feat/"            # Change branch prefix
```

#### Editor Configuration

The tool respects your editor preference in this order:
1. `CF_EDITOR` from config file
2. `$EDITOR` environment variable
3. `vi` (fallback)

```bash
# Set editor via environment variable
export EDITOR=nano

# Or via config file
echo 'CF_EDITOR="code --wait"' > ~/.cf/config
```

## Examples

### Development Workflow

```bash
# Start a new feature
cf --worktree user-profile --window profile "Build user profile management"

# Quick iteration
cf "Add validation to user profile form"

# Code review preparation
cf --worktree review "Prepare code for review, add tests and documentation"
```

### Different Use Cases

```bash
# Architecture planning
cf "Design microservices architecture for e-commerce platform"

# Debugging
cf --worktree bugfix "Fix authentication timeout issue"

# Documentation
cf "Generate API documentation for user endpoints"

# Testing
cf "Create comprehensive test suite for payment processing"
```

## Troubleshooting

### Common Issues

**"command not found: cf"**
- Ensure the script is in your PATH
- Check that it's executable: `chmod +x cf`

**"❌ Missing required dependencies"**
- Install the missing tools as indicated in the error message
- For tmux: only required when using `--window` option

**"❌ Error: Failed to open editor"**
- Check that your editor is properly installed
- Try setting a different editor: `export EDITOR=nano`

**"❌ Not in a tmux session"**
- Start tmux first: `tmux`
- Or use without `--window` option

**"❌ Not in a git repository"**
- Initialize git: `git init`
- Or use without `--worktree` option

### Configuration Issues

**Config file not loading**
- Check file permissions: `chmod 644 ~/.cf/config`
- Verify bash syntax in config file
- Check for typos in variable names

**Editor not working**
- For VS Code: use `CF_EDITOR="code --wait"`
- For GUI editors: ensure they support command-line usage

## Advanced Usage

### Custom Worktree Structure

```bash
# Use custom worktree location
echo 'CF_WORKTREE_DIR="../features"' > .cf/config
cf --worktree api "Build API endpoints"
# Creates: ../features/api/
```

### Branch Naming Conventions

```bash
# Custom branch prefix
echo 'CF_BRANCH_PREFIX="task/"' > .cf/config
cf --worktree 123 "Implement task #123"
# Creates: task/123 branch
```

### Integration with Other Tools

```bash
# Use with different editors
CF_EDITOR="vim" cf "Quick edit session"
CF_EDITOR="emacs" cf "Emacs session"
CF_EDITOR="code --wait" cf "VS Code session"
```

## How It Works

1. **Dependency Check**: Verifies required tools are installed
2. **Configuration Loading**: Loads settings from config files
3. **Git Worktree**: Creates feature branch and worktree if requested
4. **Tmux Window**: Creates new tmux window if requested
5. **Prompt Collection**: Opens editor for interactive prompt or uses provided prompt
6. **Claude Flow Execution**: Runs `claude-flow sparc` with the prompt
7. **Cleanup**: Removes temporary files

## Contributing

This tool is designed to be simple and focused. When contributing:

1. Maintain backward compatibility
2. Add comprehensive error handling
3. Update documentation for new features
4. Test on multiple platforms
5. Keep dependencies minimal

## License

MIT License - Feel free to use and modify as needed.

## Known Limitations

- **Platform Support**: Primarily tested on macOS/Linux
- **Claude Flow Dependency**: Requires claude-flow to be installed
- **Tmux Requirement**: Window mode requires active tmux session
- **Git Requirement**: Worktree mode requires git repository

## Future Enhancements

- Windows/PowerShell support
- Integration with more task management tools
- Project templates and presets
- Session history and replay
- Multi-project workspace support
