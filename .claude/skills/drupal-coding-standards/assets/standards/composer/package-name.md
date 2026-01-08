# Composer package name convention

With Drupal adopting [Composer](https://getcomposer.org/) as dependency manager, the community has to follow a naming convention for composer package names to avoid conflicts.

How you define your own package with a composer.json can be found in [Add a composer.json file to define your module as a PHP package](https://www.drupal.org/node/2514612).

In general, Composer [only](https://getcomposer.org/doc/01-basic-usage.md#package-names) allows for package names like so: `vendor/project`. You cannot use more than two levels. This leads to the following conventions.

## Drupal Projects

Any project hosted on drupal.org is considered a _Drupal Project_. This contains Drupal core, modules, themes, profiles, …. Drupal already has an existing namespace for these based on the project URL `https://www.drupal.org/project/PROJECT`.

### Convention

Projects must use the package name:

```php
drupal/PROJECT
```

where `PROJECT` is the part from the URL.

### Examples

* _Drupal_ is a project: [https://www.drupal.org/project/drupal](https://www.drupal.org/project/drupal) → `drupal/drupal`
* _ctools_ is a contrib project: [https://www.drupal.org/project/ctools](https://www.drupal.org/project/ctools) → `drupal/ctools`
* _Views_ in Drupal 7 is a contrib project: [https://www.drupal.org/project/views](https://www.drupal.org/project/views) → `drupal/views`
* _Core_ is a subtree of Drupal: [https://www.drupal.org/project/core](https://www.drupal.org/project/core) → `drupal/core`
* _Datetime_ is a module within Drupal Core: [https://www.drupal.org/project/datetime](https://www.drupal.org/project/datetime) → `drupal/datetime`

Some project URLs are not accessible or point to another project, as they are reserved names. In most cases those are sub-modules or sub-themes of existing projects, like Drupal core.

## Modules, Themes and Profiles

Multiple modules, themes and profiles might be part of a single Drupal project, for example `system, node, standard` in [Drupal](https://www.drupal.org/project/drupal) or `page_manager, views_content` in [ctools](https://www.drupal.org/project/ctools). As Drupal won't let you run two modules with the same name, modules, themes, profiles and Drupal projects share the same namespace.

### Convention

Sub-modules, -themes and profiles must use the package name.

```php
drupal/SUBPROJECT
```

where `SUBPROJECT` is the machine name of the module, theme or profile.

This will not conflict with `drupal/PROJECT`, as Project names share the same namespace with modules, themes and profiles due to the way Drupal works.

Over time, some projects may merge into or were split from another project. For example _Views_ was merged into _Drupal 8_ or _[Page Manager](https://www.drupal.org/project/page_manager)_ was separated from _ctools_ for a Drupal 8 release. Because of different version numbers, these cases can be resolved by utilizing [Composer's replace property](https://getcomposer.org/doc/04-schema.md#replace) and/or using different composer repositories.

In the case naming conflicts cannot be resolved, _Drupal Projects_ shall take precedence over (sub-)modules, themes and profiles.

For avoiding dependency issues, Drupal projects declaring their own composer.json, should also add their submodules and -themes to the `replace`-section.

### Examples

*   Module `devel_generate` from [Devel](https://www.drupal.org/project/devel) → `drupal/devel_generate`
*   Drupal 8 core's Views module can be specified as `drupal/views`, but the dependency will be resolved as a contrib replacement by `drupal/core`.

Based on this convention, meta-packages or subtree-splits could be provided for every module, theme and profile.

## Components

Drupal projects may also contain custom components (like PHP libraries). As those components are not bound to any namespace, they are likely to conflict with a given Drupal project, module, theme, etc..

### Convention

Package names must be prefixed with their parent's name and a dash (`-`), in the case it will use the `drupal/` vendor:

```php
drupal/PARENT-COMPONENT
```

where `PARENT` is the name of the parent package and `COMPONENT` a sufficient name for the component.

Since the `-` (dash) is **not** used in the existing drupal namespace, it _shouldn't_ conflict in anyway to what is currently in Drupal.

### Examples

*   [Datetime](https://github.com/drupal/drupal/tree/8.0.x/core/lib/Drupal/Component/Datetime) becomes `drupal/core-datetime`
*   [Diff](https://github.com/drupal/drupal/tree/8.0.x/core/lib/Drupal/Component/Diff) becomes `drupal/core-diff`
*   This also allows contrib to expose components like so: `drupal/panels-renderer` or `drupal/ds-builder`
