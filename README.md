# Drupal Coding Standards Agent Skill

An [Agent Skill](https://agentskills.io/) that provides AI agents with the ability to review code according to Drupal's official coding standards. This skill uses dynamic context discovery to efficiently load only the relevant standards based on the file type being reviewed.

## Features

- ✅ **Official Standards**: Uses the official Drupal coding standards from [drupal/coding_standards](https://git.drupalcode.org/project/coding_standards)
- ✅ **Dynamic Context Discovery**: Loads only relevant standards based on file type
- ✅ **Easy Updates**: Update standards with a simple `composer update` command
- ✅ **Minimal Maintenance**: Only routing logic is maintained, content comes from official source
- ✅ **AGENTS.md Compatible**: Can be referenced from project-level AGENTS.md files

## Installation

### For End Users

Simply install via Composer:

```bash
composer require ronaldtebrake/drupal-coding-standards-skill
```

The skill will be installed with all assets already scaffolded in `.claude/skills/drupal-coding-standards-skill/`. No additional setup required!

### For Maintainers

To update the coding standards:

```bash
# Clone the repository
git clone <repository-url>
cd drupal-coding-standards-skill

# Install dev dependencies (includes drupal/coding_standards)
composer install

# Copy the updated standards to assets directory
cp -r vendor/drupal/coding_standards/docs .claude/skills/drupal-coding-standards-skill/assets/standards

# Commit the updated assets
git add .claude/skills/drupal-coding-standards-skill/assets/standards/
git commit -m "Update coding standards to version X.X"
```

The standards are manually copied from `vendor/drupal/coding_standards/docs` to `.claude/skills/drupal-coding-standards-skill/assets/standards/` when updating.

## Usage

### For AI Agents

1. Load the skill by reading `.claude/skills/drupal-coding-standards-skill/SKILL.md`
2. When reviewing code, identify the file type (`.php`, `.js`, `.css`, etc.)
3. Consult `standards-index.md` to find the relevant standard file
4. Load the specific standard from `.claude/skills/drupal-coding-standards-skill/assets/standards/` (following Agent Skills specification)
5. Review code against the standards

### Example Workflow

```markdown
# Agent receives request to review a PHP file

1. Read SKILL.md (minimal context)
2. Identify file: example.module (PHP file)
3. Check standards-index.md → PHP standards at .claude/skills/drupal-coding-standards-skill/assets/standards/php/coding.md
4. Load coding.md and review code
5. Reference documentation.md if checking docblocks
```

## Standards Coverage

This skill provides access to Drupal coding standards for:

- **PHP** (`.php`, `.module`, `.inc`, `.install`, `.test`)
- **JavaScript** (`.js`, `.jsx`)
- **CSS** (`.css`, `.scss`, `.sass`)
- **Twig** (`.twig`)
- **YAML** (`.yml`, `.yaml`)
- **SQL** (`.sql`)
- **Markup** (`.html`, `.htm`)
- **Composer** (`composer.json`)
- **Accessibility** (cross-cutting)
- **Spelling** (cross-cutting)

## Updating Standards (Maintainers Only)

To update to the latest Drupal coding standards:

1. **Bump the version** in `composer.json`:
   - Update the `version` field in the `repositories[0].package` section (e.g., from `0.1` to `0.2`)
   - Update the `require-dev` constraint if needed (e.g., `^0.2`)

2. **Run composer update**:
   ```bash
   composer update drupal/coding_standards
   ```

3. **Copy the updated standards**:
   ```bash
   cp -r vendor/drupal/coding_standards/docs .claude/skills/drupal-coding-standards-skill/assets/standards
   ```

4. **Commit the updated assets**:
   ```bash
   git add .claude/skills/drupal-coding-standards-skill/assets/standards/
   git commit -m "Update coding standards to version X.X"
   ```

5. **Update the skill version** in `.claude/skills/drupal-coding-standards-skill/SKILL.md` frontmatter and `CHANGELOG.md` to reflect the new standards version

The standards are updated in `vendor/drupal/coding_standards/` (dev dependency) and manually copied to `.claude/skills/drupal-coding-standards-skill/assets/standards/`. End users receive the skill with standards already included - they don't need the Drupal coding standards dependency.

**Note**: Since we're using a package repository type, Composer won't automatically detect updates. You control when to update by incrementing the version number in `composer.json`.

## Integration with AGENTS.md

Projects can reference this skill in their `AGENTS.md` file:

```markdown
## Code Review Standards

This project follows Drupal coding standards. When reviewing code:

- Load the skill: `.claude/skills/drupal-coding-standards-skill/SKILL.md`
- Reference standards from: `.claude/skills/drupal-coding-standards-skill/assets/standards/[category]/[file].md`
- Use `standards-index.md` to find the correct standard file for each file type
```

## Architecture

This skill acts as a **thin routing layer** on top of the official Drupal coding standards:

```
.claude/skills/drupal-coding-standards-skill/
├── SKILL.md (routing logic)
├── standards-index.md (file type routing)
└── assets/standards/ (scaffolded from drupal/coding_standards)
    ↓
AI Agent loads only relevant files
```

The standards are scaffolded into the repository, so end users don't need any dependencies - they just get the complete skill ready to use.

### Dynamic Context Discovery

The skill is optimized for dynamic context discovery:

- **SKILL.md**: Minimal entry point (~60 lines)
- **standards-index.md**: Routing table for file types
- **assets/standards/**: Standards directory (symlinked to vendor) following Agent Skills spec
- **Standards files**: Loaded on-demand from assets directory
- **No content duplication**: All standards come from official source

## Requirements

- PHP 7.4 or higher
- Composer

## License

GPL-2.0-or-later (same as Drupal)

## Contributing

This skill is a routing layer over the official Drupal coding standards. To contribute to the standards themselves, please contribute to the [drupal/coding_standards](https://git.drupalcode.org/project/coding_standards) project.

For issues or improvements to this skill (routing logic, documentation, etc.), please open an issue or pull request.

## Version History

See [CHANGELOG.md](CHANGELOG.md) for version history.
