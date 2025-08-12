# Black Atom Industries Organization Guide

## Project Overview

Black Atom Industries is a collection of cohesive, elegant dark/light themes for various applications and platforms. The themes are organized into collections, each with distinctive visual styles and color palettes.

## Repository Structure

All repositories in the organization can be found at: https://github.com/orgs/black-atom-industries/repositories

### Key Repositories

- **core**: Central source of truth for theme definitions and CLI tool

  - Contains TypeScript definitions for all theme collections  
  - Provides a CLI tool for generating theme files for various platforms
  - Uses an adapter pattern to generate platform-specific files

- **Adapter repositories**: Platform-specific theme implementations
  - Each adapter repository contains platform-specific theme files
  - Generated from core definitions using the adapter pattern
  - See organization repositories page for complete list

## Theme Collections

Theme collections are organized by concept and aesthetic. Each collection contains multiple theme variants with different color palettes and appearances (dark/light).

**Current collections can be discovered by exploring**: `core/src/themes/`

Each collection directory contains:
- Individual theme definition files  
- Shared UI components (`ui_dark.ts`, `ui_light.ts`)
- Shared syntax highlighting (`syntax_dark.ts`, `syntax_light.ts`)

## Project Status & Tasks

For current project status, active tasks, and roadmap, see the [Linear Black Atom Industries Team](https://linear.app/black-atom-industries/settings/teams/DEV).

**Development principles:**
- Maintain core as the single source of truth for theme definitions
- Follow established patterns and maintain consistency  
- Test themes across all platforms after changes
- Use the adapter pattern for platform-specific implementations

## Core Architecture

### Theme Definition Structure

Each theme is defined with the following components:

- **Meta**: Theme metadata (key, label, collection, appearance, status)
- **Primaries**: Object with semantic color ranges:
  - Dark range (d10-d40): Darkest colors in the palette
  - Middle range (m10-m40): Mid-tone colors in the palette
  - Light range (l10-l40): Lightest colors in the palette
- **Palette**: Standard color definitions (red, green, blue, etc.)
- **UI**: User interface color definitions (derived from primaries)
- **Syntax**: Syntax highlighting color definitions (derived from primaries)

**Important:** Adapters should never directly access the primaries in their templates. Instead, they should use the UI, syntax, or palette colors which are derived from primaries. This creates a consistent semantic layer and makes theme adaptation more maintainable.

### Adapter Pattern

The core project uses an adapter pattern to generate platform-specific theme files:

1. Each platform repository contains a `black-atom-adapter.json` file
2. This config specifies which templates to use for each theme
3. The core CLI tool reads this config and generates the appropriate files

## Development Workflow

### Theme Creation Process

1. Define theme in core repository (TypeScript)
2. Create adapter templates for target platforms
3. Generate platform-specific files using core CLI
4. Test themes in respective applications

### Migrating Themes to Core

If migrating themes from platform-specific implementations to core:

1. Copy original theme files to `/migrations/collection-name/` for reference
2. Create TypeScript files following the core repository structure:
   - Create shared UI/syntax files (ui_dark.ts, syntax_dark.ts, etc.)
   - Create theme definition files that use these shared components
3. Update config.ts to import and use the new themes
4. Verify with typechecking and testing

### Adapter Template Development

For creating new platform adapters:

1. Create a `black-atom-adapter.json` file in the platform repository
2. Define template files that will be used for theme generation
3. Templates use the Eta template engine syntax
4. The core CLI will process these templates and generate platform-specific files

#### Adapter Best Practices

Follow these important best practices when developing adapters:

1. **Never access primaries directly** - Instead, use UI, syntax, or palette colors from the theme
2. Templates can be in any format (JSON, Lua, CSS, CONF, YAML, etc.)
3. Use the adapter-template repository as a reference for creating new adapters
4. Refer to ADAPTER_DEVELOPMENT.md in the core repository for detailed guidelines
5. Follow the established patterns seen in existing adapters like Neovim and Ghostty
6. Use `<%= theme.ui.bg.default %>` pattern instead of `<%= theme.primaries.d10 %>`
7. **Always verify token naming** - Make sure you're using the exact token names from the theme definition
8. **Test generated output** - Run `deno task dev:adapters:adapt` from the core to verify no `undefined` values appear in generated files

## Creating a New Adapter

When creating a new adapter for a platform, follow these steps:

1. **Study the platform's theme format**:
   - Look at examples of themes for the target platform
   - Identify key color tokens needed for the platform

2. **Create the adapter.json file**:
   - Include template paths for each theme you want to support
   - Example: `{ "black-atom-crbn-null": { "templates": ["./themes/crbn/black-atom-crbn-null.template.json"] } }`

3. **Create template directory structure**:
   - Organize templates by collection (crbn, jpn, stations, terra)
   - Each collection folder should contain template files for each theme

4. **Create template files**:
   - Study the core token system in `core/src/types/theme.ts`
   - Replace hardcoded colors with template variables using Eta syntax
   - Ensure you're using correct token names: `<%= theme.ui.fg.default %>`, `<%= theme.palette.red %>`, etc.

5. **Testing your adapter**:
   - Run `cd core && deno task dev:adapters:adapt` to generate theme files
   - Check for any `undefined` values in the output
   - If issues occur, verify your token names match those in `theme.ts`

## Theme Development Prompts

The core repository contains comprehensive guides in the `prompts/` directory for common theme development tasks:

- **`new-theme.md`** - Step-by-step guide for creating new themes within existing collections, including testing procedures
- **`new-collection.md`** - Creating entirely new theme collections with consistent design philosophy
- **`migration-rename.md`** - Renaming themes or collections across all repositories
- **`migration-properties.md`** - Updating theme property structures when the schema changes (if exists)

These prompts include detailed testing instructions for verifying themes across all platforms.

## Commands & Tools

### Core Repository Commands

For complete list of available commands and their descriptions:
- **Core repository**: See `core/CLAUDE.md` for detailed command documentation
- **Deno tasks**: Run `deno task` in the core directory to see all available tasks
- **CLI help**: Run `black-atom-core help` for CLI command documentation

### Development Workflow

1. **Theme development**: Work in `core/src/themes/`
2. **Adapter testing**: Use `deno task adapters:gen` to generate all adapters  
3. **Cross-platform verification**: Test generated themes in respective applications

## Token System Reference

When creating templates, use these token paths to access colors correctly:

### Theme Meta
- `<%= theme.meta.label %>` - Theme display name
- `<%= theme.meta.appearance %>` - "dark" or "light"
- `<%= theme.meta.collection.label %>` - Collection name

### UI Colors
- `<%= theme.ui.fg.default %>` - Default text color
- `<%= theme.ui.fg.subtle %>` - Subtle text color
- `<%= theme.ui.fg.accent %>` - Accent/highlight color
- `<%= theme.ui.fg.contrast %>` - High-contrast text color
- `<%= theme.ui.fg.disabled %>` - Disabled element text color
- `<%= theme.ui.fg.negative %>` - Error text color
- `<%= theme.ui.fg.positive %>` - Success text color

- `<%= theme.ui.bg.default %>` - Default background color
- `<%= theme.ui.bg.panel %>` - Panel/sidebar background color
- `<%= theme.ui.bg.disabled %>` - Disabled element background
- `<%= theme.ui.bg.selection %>` - Selection background
- `<%= theme.ui.bg.hover %>` - Hover state background

### Palette (ANSI Colors)
- `<%= theme.palette.black %>` - ANSI Color 0
- `<%= theme.palette.darkRed %>` - ANSI Color 1
- `<%= theme.palette.darkGreen %>` - ANSI Color 2
- `<%= theme.palette.darkYellow %>` - ANSI Color 3
- `<%= theme.palette.darkBlue %>` - ANSI Color 4
- `<%= theme.palette.darkMagenta %>` - ANSI Color 5
- `<%= theme.palette.darkCyan %>` - ANSI Color 6
- `<%= theme.palette.lightGray %>` - ANSI Color 7
- `<%= theme.palette.gray %>` - ANSI Color 8
- `<%= theme.palette.red %>` - ANSI Color 9
- `<%= theme.palette.green %>` - ANSI Color 10
- `<%= theme.palette.yellow %>` - ANSI Color 11
- `<%= theme.palette.blue %>` - ANSI Color 12
- `<%= theme.palette.magenta %>` - ANSI Color 13
- `<%= theme.palette.cyan %>` - ANSI Color 14
- `<%= theme.palette.white %>` - ANSI Color 15

### Syntax Colors
- `<%= theme.syntax.comment.default %>` - Comments
- `<%= theme.syntax.string.default %>` - String literals
- `<%= theme.syntax.keyword.default %>` - Keywords
- `<%= theme.syntax.func.default %>` - Functions
- `<%= theme.syntax.type.default %>` - Type definitions
- `<%= theme.syntax.variable.default %>` - Variables

## Code Style Guide

- **TypeScript/JavaScript**:
  - 4 spaces, 100 char line width
  - Double quotes, semicolons
  - camelCase for variables/functions, PascalCase for types/interfaces
- **Lua (Neovim)**:
  - Follow stylua formatting
  - snake_case for variables/functions

## Project Goals

- Maintain core as the single source of truth for theme definitions
- Enable generation of theme files for multiple platforms
- Ensure consistent theme experience across different tools
- Support all theme collections across all platforms
- Simplify the process of creating and updating themes
