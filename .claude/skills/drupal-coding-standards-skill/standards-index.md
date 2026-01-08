# Drupal Coding Standards Index

This file maps file types and extensions to the corresponding Drupal coding standards documentation files.

## File Type Routing

| File Extensions | Category | Primary Standard File | Additional Files |
|----------------|----------|----------------------|-----------------|
| `.php`, `.module`, `.inc`, `.install`, `.test` | PHP | `assets/standards/php/coding.md` | `documentation.md`, `naming-services.md`, `namespaces.md`, `psr4.md`, `exceptions.md`, `e_all.md`, `placeholders-delimiters.md` |
| `.js`, `.jsx` | JavaScript | `assets/standards/javascript/coding.md` | `best-practice.md`, `documentation.md`, `eslint.md`, `jquery.md` |
| `.css`, `.scss`, `.sass` | CSS | `assets/standards/css/coding.md` | `architecture.md`, `format.md`, `file-organization.md`, `csscomb.md`, `review.md` |
| `.twig` | Twig | `assets/standards/twig/coding.md` | - |
| `.yml`, `.yaml` | YAML | `assets/standards/yaml/configuration-files.md` | - |
| `.sql` | SQL | `assets/standards/sql/conventions.md` | `keywords.md`, `select-from.md` |
| `.html`, `.htm` | Markup | `assets/standards/markup/style.md` | - |
| `composer.json` | Composer | `assets/standards/composer/package-name.md` | - |

## Usage Instructions

1. **Identify the file extension** of the code being reviewed
2. **Find the corresponding row** in the table above
3. **Load the primary standard file** for the initial review
4. **Reference additional files** as needed for specific topics (documentation, naming, etc.)

## General Concerns
- **Accessibility**: `assets/standards/accessibility/accessibility.md` (applies to all frontend code)
- **Spelling**: `assets/standards/spelling/spelling.md` (applies to all documentation and user-facing strings)
