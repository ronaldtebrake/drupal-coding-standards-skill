# AGENTS.md

This file provides instructions for AI agents working on this repository, particularly for updating the Drupal coding standards.

## Project Structure

```
drupal-coding-standards-skill/
├── .claude/
│   └── skills/
│       └── drupal-coding-standards-skill/
│           ├── SKILL.md                    # Entry point for the skill
│           ├── standards-index.md        # File type routing table
│           └── assets/
│               └── standards/              # Scaffolded Drupal coding standards
├── composer.json                           # Package definition with dev dependencies
├── CHANGELOG.md                            # Version history
├── README.md                               # User-facing documentation
└── AGENTS.md                               # This file
```

## Updating Drupal Coding Standards

When a new version of the Drupal coding standards is released, follow these steps:

### Step 1: Update Composer Dependency Version

1. Open `composer.json`
2. In the `repositories[0].package` section, update the `version` field:
   - Current: `"version": "0.1"`
   - New: `"version": "0.2"` (increment as needed)
3. Update the `require-dev` constraint if needed:
   - Current: `"drupal/coding_standards": "^0.1"`
   - New: `"drupal/coding_standards": "^0.2"`

### Step 2: Update Dependencies

Run Composer to fetch the updated standards:

```bash
composer update drupal/coding_standards
```

This will download the latest standards to `vendor/drupal/coding_standards/docs/`.

### Step 3: Copy Updated Standards to Assets

Copy the updated standards from vendor to the skill's assets directory:

```bash
rm -rf .claude/skills/drupal-coding-standards-skill/assets/standards
cp -r vendor/drupal/coding_standards/docs .claude/skills/drupal-coding-standards-skill/assets/standards
```

**Important**: Verify the copy was successful by checking that files exist:
```bash
ls -la .claude/skills/drupal-coding-standards-skill/assets/standards/
```

### Step 4: Update Skill Version

Update the version in the skill's frontmatter:

1. Open `.claude/skills/drupal-coding-standards-skill/SKILL.md`
2. Update the `version` field in the YAML frontmatter:
   ```yaml
   ---
   name: drupal-coding-standards
   description: Review code according to Drupal's official coding standards
   version: 1.1.0  # Update this
   ---
   ```

### Step 5: Update CHANGELOG.md

Add a new entry to `CHANGELOG.md` following the existing format:

```markdown
## [1.1.0] - YYYY-MM-DD

### Updated
- Updated Drupal coding standards to version X.X (from drupal/coding_standards repository)
- Updated standards files in assets/standards/ to match latest Drupal documentation

### Notes
- Standards pulled from drupal/coding_standards repository version X.X
```

### Step 6: Verify Structure

Ensure the standards directory structure is correct:

```bash
# Check main categories exist
ls .claude/skills/drupal-coding-standards-skill/assets/standards/

# Should include: php/, javascript/, css/, twig/, yaml/, sql/, markup/, accessibility/, etc.
```

### Step 7: Report

Let the user know you're done, so they can run their checks before committing and tagging this new version.

## Key Files to Update

When updating standards, always update:

1. ✅ `.claude/skills/drupal-coding-standards-skill/assets/standards/` - The actual standards files
2. ✅ `.claude/skills/drupal-coding-standards-skill/SKILL.md` - Version in frontmatter
3. ✅ `composer.json` - Dependency version in repositories and require-dev
4. ✅ `CHANGELOG.md` - Add entry documenting the update

## Files That Should NOT Change

These files should remain stable unless the Drupal coding standards repository structure changes:

- `standards-index.md` - Only update if file structure in coding_standards repo changes
- `README.md` - Only update if installation/usage process changes
- `AGENTS.md` - Only update if maintenance process changes

## Verification Checklist

Before committing an update, verify:

- [ ] Standards files copied successfully to `assets/standards/`
- [ ] All expected categories present (php, javascript, css, twig, yaml, sql, markup, accessibility)
- [ ] SKILL.md version updated
- [ ] composer.json version updated
- [ ] CHANGELOG.md entry added
- [ ] Git status shows only expected files changed

## Troubleshooting

### Standards directory is empty after copy

- Verify `vendor/drupal/coding_standards/docs` exists
- Check that `composer update` completed successfully
- Ensure you're running commands from the repository root

### Version mismatch errors

- Ensure version in `composer.json` repositories section matches require-dev constraint
- Check that SKILL.md frontmatter version is updated

### Missing categories in standards

- Check the Drupal coding_standards repository structure
- Update `standards-index.md` if new categories are added
- Update README.md if new file types are supported

## Dependencies

- **For end users**: None - the skill is self-contained with all assets scaffolded
- **For maintainers**: `drupal/coding_standards` (dev dependency only)

## Notes

- The coding standards are pulled from: `https://git.drupalcode.org/project/coding_standards.git`
- Standards are manually copied (not symlinked) to ensure the skill is self-contained
- End users don't need any dependencies - they get the complete skill ready to use
- The skill follows the Agent Skills specification with standards in `assets/standards/`
