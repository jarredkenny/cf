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

# Interactive mode with NAME (uses config defaults)
cf auth

# Direct prompt mode
cf auth --prompt "Build a REST API for user authentication"

# Show help
cf --help
```

### Git Worktree Integration

```bash
# Create feature branch and worktree (explicit)
cf --worktree auth --prompt "Implement user authentication"

# With config enabled (CF_ENABLE_WORKTREE="true"), use NAME directly
cf auth --prompt "Implement user authentication"

# This creates:
# - Branch: feature/auth
# - Worktree: ./worktrees/auth/
# - Runs claude-flow in the worktree directory
```

### Tmux Window Mode

```bash
# Run in new tmux window (explicit)
cf --window api --prompt "Build REST API endpoints"

# With config enabled (CF_ENABLE_TMUX="true"), use NAME directly
cf api --prompt "Build REST API endpoints"

# Combine with worktree
cf --window auth --worktree auth --prompt "User authentication system"
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
CF_ENABLE_TMUX="true"               # Enable tmux integration by default
CF_ENABLE_WORKTREE="true"           # Enable worktree integration by default
CF_BASE_BRANCH="main"               # Base branch for new worktrees (default: current HEAD)
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
# Start a new feature (explicit flags)
cf --worktree user-profile --window profile --prompt "Build user profile management"

# With config enabled (CF_ENABLE_TMUX="true" CF_ENABLE_WORKTREE="true")
cf user-profile --prompt "Build user profile management"

# Quick iteration (interactive)
cf user-profile

# Code review preparation
cf review --prompt "Prepare code for review, add tests and documentation"
```

### Different Use Cases

```bash
# Architecture planning
cf architecture --prompt "Design microservices architecture for e-commerce platform"

# Debugging
cf bugfix --prompt "Fix authentication timeout issue"

# Documentation
cf docs --prompt "Generate API documentation for user endpoints"

# Testing
cf tests --prompt "Create comprehensive test suite for payment processing"
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
echo 'CF_ENABLE_WORKTREE="true"' >> .cf/config
cf api --prompt "Build API endpoints"
# Creates: ../features/api/
```

### Branch Naming Conventions

```bash
# Custom branch prefix
echo 'CF_BRANCH_PREFIX="task/"' > .cf/config
echo 'CF_ENABLE_WORKTREE="true"' >> .cf/config
cf 123 --prompt "Implement task #123"
# Creates: task/123 branch

# Specify base branch for new worktrees
echo 'CF_BASE_BRANCH="develop"' >> .cf/config
cf feature-x --prompt "New feature"
# Creates: feature/feature-x branch from develop
```

### Integration with Other Tools

```bash
# Use with different editors
CF_EDITOR="vim" cf quick-edit
CF_EDITOR="emacs" cf emacs-session
CF_EDITOR="code --wait" cf vscode-session
```

### Configuration-Driven Workflow

When you enable the integration options in your config file, the workflow becomes much simpler:

```bash
# ~/.cf/config
CF_ENABLE_TMUX="true"
CF_ENABLE_WORKTREE="true"
CF_EDITOR="code --wait"
CF_WORKTREE_DIR="./features"
CF_BASE_BRANCH="main"
```

With this configuration:

```bash
# Simple NAME-based workflow
cf auth                              # Interactive: tmux window + worktree
cf auth --prompt "Build auth system" # Direct: tmux window + worktree + prompt

# Still supports explicit overrides
cf --window special-auth auth        # Uses "special-auth" for tmux window
cf --worktree different-auth auth    # Uses "different-auth" for worktree
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
