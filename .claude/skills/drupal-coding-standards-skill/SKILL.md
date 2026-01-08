---
name: drupal-coding-standards
description: Review code according to Drupal's official coding standards
version: 1.0.0
---

# Drupal Coding Standards Skill

This skill provides AI agents with the ability to review code according to Drupal's official coding standards. The standards are maintained by the Drupal community and automatically kept up-to-date via Composer dependency.

## When to Use

Activate this skill when:
- Reviewing code in Drupal projects (modules, themes, profiles)
- Checking code for compliance with Drupal coding standards
- Performing code reviews on PHP, JavaScript, CSS, Twig, YAML, or SQL files
- Ensuring code follows Drupal best practices and conventions

## How It Works

This skill uses **dynamic context discovery** to load only the relevant standards based on the file type being reviewed:

1. **Identify the file type** being reviewed (`.php`, `.js`, `.css`, `.twig`, `.yml`, etc.)
2. **Consult the routing table** in `standards-index.md` to find the corresponding standard file
3. **Load the specific standard** from `assets/standards/[category]/[file].md`
4. **Apply the standards** to review the code

## Review Workflow

1. Read `standards-index.md` to determine which standard file applies to the code being reviewed
2. Load the relevant standard file from `assets/standards/`
3. Review the code against the standards
4. Provide specific, actionable feedback referencing the relevant sections of the standards
