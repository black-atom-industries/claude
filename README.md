# Black Atom Industries - Claude Code Configuration

This repository contains Claude Code configuration and custom slash commands for Black Atom Industries theme development workflow.

## Installation

Clone this repository to your Black Atom Industries organization directory:

```bash
cd /Users/nbr/repos/black-atom-industries
git clone https://github.com/black-atom-industries/claude.git .claude
```

**Important**: The repository must be cloned as `.claude` (note the dot prefix) in the root of your Black Atom Industries organization directory.

### CLAUDE.md Setup

After cloning, copy the main project documentation to the organization root where Claude Code can find it:

```bash
cp /Users/nbr/repos/black-atom-industries/.claude/CLAUDE.md /Users/nbr/repos/black-atom-industries/CLAUDE.md
```

This file contains the main project instructions and must be at the organization root for Claude Code to pick it up automatically.

## Directory Structure

```
/Users/nbr/repos/black-atom-industries/
├── .claude/                    # This repository
│   ├── commands/
│   │   ├── adapter/
│   │   │   └── update-theme-name.md
│   │   └── org/
│   │       └── commit.md
│   ├── settings.local.json     # Local settings (not tracked)
│   └── README.md
├── core/                       # Core theme definitions
├── nvim/                       # Neovim adapter
├── ghostty/                    # Ghostty adapter
├── zed/                        # Zed adapter
├── wezterm/                    # WezTerm adapter
└── tmux/                       # Tmux adapter
```

## Available Slash Commands

See the individual command files in the `commands/` directory for detailed documentation on usage, examples, and implementation details.

## Configuration

### settings.local.json

Create a `settings.local.json` file in this directory for local Claude Code settings. This file is ignored by git and should contain your personal configuration.

Example:
```json
{
  "workingDirectory": "/Users/nbr/repos/black-atom-industries"
}
```

## Development Workflow

1. **Theme Development**: Use the core repository as the source of truth
2. **Adapter Updates**: Use `/adapter:update-theme-name` for renaming operations
3. **Organization Commits**: Use `/org:commit` for consistent messaging across repos
4. **Testing**: Always test themes across all platforms after changes

## Contributing

When adding new slash commands:

1. Create the command documentation in the appropriate `commands/` subdirectory
2. Follow the existing documentation structure and format
3. Include examples, error handling, and implementation details
4. Test the command thoroughly before committing

## Related Repositories

See all Black Atom Industries repositories: https://github.com/orgs/black-atom-industries/repositories