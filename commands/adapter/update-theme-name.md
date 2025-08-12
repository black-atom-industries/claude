# Adapter: Update Theme Name

Updates theme names across all Black Atom repositories (core and adapters).

## Usage

```
/adapter:update-theme-name <old-name> <new-name>
```

## Example

```
/adapter:update-theme-name blue mikado
```

This will update:
- `black-atom-mnml-blue-dark` → `black-atom-mnml-mikado-dark`
- `black-atom-mnml-blue-light` → `black-atom-mnml-mikado-light`

## Core Repository Updates

### Files to Update:
1. **Theme definition files**:
   - `src/themes/mnml/black-atom-mnml-{old-name}-dark.ts` → `src/themes/mnml/black-atom-mnml-{new-name}-dark.ts`
   - `src/themes/mnml/black-atom-mnml-{old-name}-light.ts` → `src/themes/mnml/black-atom-mnml-{new-name}-light.ts`

2. **Configuration files**:
   - `src/config.ts` - Update theme mapping keys in the themes object
   - `adapter.schema.json` - Update theme key references in schema

### Steps:
1. Rename theme definition files
2. Update `config.ts` theme mappings
3. Update `adapter.schema.json` references
4. Run `deno task check` to verify types
5. Commit with: `refactor(themes): rename mnml {old-name} themes to {new-name}`

## Adapter Repository Updates

### nvim
**Files to Update**:
- `black-atom-adapter.json` - Update theme references in mnml collection
- `lua/black-atom/themes/mnml/black-atom-mnml-{old-name}-*.lua` → rename to `{new-name}`
- `colors/black-atom-mnml-{old-name}-*.lua` → rename to `{new-name}`

**⚠️ CRITICAL**: The `colors/` files contain require paths that must be updated!

**Steps**:
1. Update adapter.json theme references
2. Rename theme files in `lua/black-atom/themes/mnml/`
3. Rename theme files in `colors/`
4. **Update require paths in colors files**:
   ```lua
   -- Change this:
   local theme = require("black-atom.themes.mnml.black-atom-mnml-{old-name}-dark")
   -- To this:
   local theme = require("black-atom.themes.mnml.black-atom-mnml-{new-name}-dark")
   ```
5. Commit with: `refactor(themes): rename mnml {old-name} themes to {new-name}`

### ghostty
**Files to Update**:
- `black-atom-adapter.json` - Update theme references in mnml collection
- Generated theme files will be updated when core regenerates adapters

**Steps**:
1. Update adapter.json theme references
2. Commit with: `refactor(adapter): rename mnml {old-name} themes to {new-name}`

### zed
**Files to Update**:
- `black-atom-adapter.json` - Update theme references in mnml collection
- `themes/mnml/black-atom-mnml-{old-name}-*.json` - Will be removed/recreated during regeneration

**Steps**:
1. Update adapter.json theme references
2. Remove old theme files (they'll be regenerated with new names)
3. Commit with: `refactor(adapter): rename mnml {old-name} themes to {new-name}`

### wezterm
**Files to Update**:
- `black-atom-adapter.json` - Update theme references in mnml collection
- `themes/mnml/black-atom-mnml-{old-name}-*.toml` - Will be removed/recreated during regeneration

**Steps**:
1. Update adapter.json theme references
2. Remove old theme files (they'll be regenerated with new names)
3. Commit with: `refactor(adapter): rename mnml {old-name} themes to {new-name}`

### tmux
**Files to Update**:
- `black-atom-adapter.json` - Update theme references in mnml collection
- `themes/mnml/black-atom-mnml-{old-name}-*.conf` - Will be removed/recreated during regeneration

**Steps**:
1. Update adapter.json theme references
2. Remove old theme files (they'll be regenerated with new names)
3. Commit with: `refactor(adapter): rename mnml {old-name} themes to {new-name}`

## Execution Order

1. **Core Repository** - Update theme definitions and config first
2. **Adapter Repositories** - Update adapter.json files in all adapters
3. **Regeneration** - Run `deno task adapters:gen` from core to regenerate all themes
4. **Testing** - Test themes across all platforms

## Common Mistakes to Avoid

### nvim Specific:
- ❌ **Don't forget to update require paths** in `colors/` files
- ❌ Don't just rename files without checking their contents
- ✅ Always verify require paths match the new filenames

### General:
- ❌ Don't update adapters before updating core (order matters)
- ❌ Don't forget to regenerate themes after adapter updates
- ✅ Always test themes after regeneration

## Implementation Notes

Based on current git status, the adapters show:
- Modified `black-atom-adapter.json` files (✅ already done)
- Deleted old theme files and added new theme files (✅ already done)
- Some adapters have additional modified theme files that were regenerated

## Current Status

✅ **Completed**: All adapter repositories have been updated and committed
🔄 **Next**: Core repository needs to be updated with new theme definition filenames and config mappings
📋 **Final**: Regenerate all themes with `deno task adapters:gen`