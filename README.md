# Drupal Coding Standards Agent Skill

An [Agent Skill](https://agentskills.io/) that provides AI agents with the ability to review code according to Drupal's official coding standards. This skill uses dynamic context discovery to efficiently load only the relevant standards based on the file type being reviewed.

## Features

- ✅ **Official Standards**: Uses the official Drupal coding standards from [drupal/coding_standards](https://git.drupalcode.org/project/coding_standards)
- ✅ **Dynamic Context Discovery**: Loads only relevant standards based on file type
- ✅ **Easy Updates**: Update standards with a simple `composer update` command
- ✅ **Minimal Maintenance**: Only routing logic is maintained, content comes from official source
- ✅ **AGENTS.md Compatible**: Can be referenced from project-level AGENTS.md files.

## Installation

Simply install via Composer:

```bash
composer require ronaldtebrake/drupal-coding-standards-skill --dev
```

The skill will be installed with all assets already scaffolded in `.claude/skills/drupal-coding-standards-skill/`. 
You can manually copy it, or use `drupal/surge` to have it aggregated for you.

### Dynamic Context Discovery

The skill is optimized for dynamic context discovery:

- **SKILL.md**: Minimal entry point (~60 lines)
- **standards-index.md**: Routing table for file types
- **assets/standards/**: Standards directory (symlinked to vendor) following Agent Skills spec
- **Standards files**: Loaded on-demand from assets directory
- **No content duplication**: All standards come from official source

## Contributing

This skill is a routing layer over the official Drupal coding standards. To contribute to the standards themselves, please contribute to the [drupal/coding_standards](https://git.drupalcode.org/project/coding_standards) project.

For issues or improvements to this skill (routing logic, documentation, etc.), please open an issue or pull request.

## Version History

See [CHANGELOG.md](CHANGELOG.md) for version history.

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
Update the version of hte package in composer to indicate we've created a new version
git add composer.json
git commit -m "Update coding standards to version X.X"
```

The standards are manually copied from `vendor/drupal/coding_standards/docs` to `.claude/skills/drupal-coding-standards-skill/assets/standards/`
