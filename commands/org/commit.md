# Org: Commit

Commits changes across all Black Atom Industries repositories with consistent messaging.

## Usage

```
/org:commit <message>
```

## Example

```
/org:commit "refactor(themes): rename mnml blue themes to mikado"
```

## Description

This command commits changes across all Black Atom Industries repositories that have uncommitted changes. It performs the following actions:

1. **Check each repository** for uncommitted changes (staged, unstaged, or untracked files)
2. **Stage all changes** in repositories that have modifications
3. **Commit with consistent message** across all affected repositories
4. **Report status** of each repository (committed, clean, or errors)

## Affected Repositories

The command checks and commits changes in:

- **core** - TypeScript theme definitions and CLI
- **nvim** - Neovim theme plugin
- **ghostty** - Ghostty terminal themes
- **zed** - Zed editor themes
- **wezterm** - WezTerm terminal themes
- **tmux** - Tmux terminal themes
- **obsidian** - Obsidian note-taking themes

## Implementation

When executed, this command will:

1. **Iterate through each repository** in the Black Atom Industries organization
2. **Check git status** to identify repositories with changes
3. **For each repository with changes**:
   - Run `git add .` to stage all changes
   - Run `git commit -m "<message>"` with the provided message
   - Report success or failure
4. **Provide summary** of all commit operations

## Output Format

```
🏢 Committing changes across Black Atom Industries repositories...

🔧 core: 
  📝 Changes detected (5 files modified, 2 files added)
  💾 Committed: refactor(themes): rename mnml blue themes to mikado
  
📦 nvim:
  ✅ Repository clean (no changes to commit)
  
📦 ghostty:
  📝 Changes detected (1 file modified)
  💾 Committed: refactor(themes): rename mnml blue themes to mikado

...

📊 Summary:
  ✅ 4 repositories committed successfully
  🔄 2 repositories had no changes
  ❌ 0 repositories failed to commit
```

## Error Handling

- **Repository not found**: Warns and skips missing repositories
- **Git errors**: Reports specific git errors for troubleshooting
- **No changes**: Clearly indicates which repositories are clean
- **Partial failures**: Continues processing other repositories if one fails

## Use Cases

### Theme Updates
```
/org:commit "refactor(themes): rename mnml blue themes to mikado"
```

### Bug Fixes
```
/org:commit "fix(adapter): correct theme generation for dark variants"
```

### New Features
```
/org:commit "feat(themes): add new terra winter collection"
```

### Documentation
```
/org:commit "docs: update adapter development guidelines"
```

## Best Practices

1. **Use semantic commit messages** following conventional commits format:
   - `feat:` for new features
   - `fix:` for bug fixes
   - `refactor:` for code refactoring
   - `docs:` for documentation changes
   - `style:` for formatting changes

2. **Review changes first** before committing across all repositories

3. **Use specific, descriptive messages** that explain what was changed and why

4. **Group related changes** - don't mix unrelated changes in a single commit

## Prerequisites

- Must be run from within the Black Atom Industries organization directory structure
- All repositories must be git repositories with proper remotes configured
- Git must be configured with user name and email for commits

## Safety Features

- **Dry run capability**: The command stages and commits only actual changes
- **Individual repository isolation**: Failure in one repository doesn't affect others
- **Clear reporting**: Shows exactly what was committed in each repository
- **No destructive operations**: Only adds and commits, never removes or resets