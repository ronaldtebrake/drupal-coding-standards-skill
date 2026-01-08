# CSS file organization

Note: This document describes a file organization and aggregation strategy that has not yet been implemented. See this core issue for that on-going work: [#1921610: [Meta] Architect our CSS](https://www.drupal.org/project/drupal/issues/1921610 "Status: Active")

## File Structure

Rulesets should be grouped into logical files that enforce the separation of concerns within the CSS, that can be aggregated efficiently and that can be easily overridden by themers.

Drupal follows a SMACSS-style categorization of its CSS rules: The State and Theme categories are not frequently used with modern CSS.

1. Base —Should include HTML element styling (e.g. a, ul, etc), typography, root scooped CSS custom properties, resets and utility classes.
2. Layout —Overall layout of the page including page template layout, region layout, etc.
3. Component (SMACSS “module”) — The majority of a theme’s styles will be within components. Components should be scoped to their own CSS files. Example components include pager, navigation, footer, etc.
4. State (not used often) — This was originally created for styles that modified the states of components, however it is now best practice to include component states within their respective stylesheet.
5. Theme (not used often) — This was originally created to apply styles that would affect the overall look and feel of the theme (such as colors), however this can now be done more effectively by modifying CSS custom properties.

For more information about Drupal’s use of SMACSS, see the [“Separate Concerns” section of the CSS Architecture page](http://drupal.org/node/1887918#separate-concerns).

These categories help us keep CSS styles with different purposes from getting intertwined with one another. However, they don’t directly dictate file structure. Drupal recommends the following guidelines for structuring CSS across files:

### CSS files for Drupal modules

All of a module's styles should be placed in a `css/` sub-directory and broken into one or more of the following files:

`module_name.module.css`: This file should hold the minimal styles needed to get the module's functionality working. This includes layout, component and state styles.

`module_name.theme.css`: This file should hold extra styles to make the module's functionality aesthetically pleasing. This usually just consists of theme styles.

`module_name.admin.css`: This file should hold the minimal styles needed to get the module's admin screens working. This includes layout, component and state styles. On admin screens, the module may choose to load the *.module.css in addition to the *.admin.css file.

`module_name.admin.theme.css`: This file should hold extra styles to make the module's admin screens aesthetically pleasing. This usually just consists of theme styles.

Note: Modules should never have any base styles. Drupal core's modules do not have any base styles. Instead Drupal core uses the Normalize.css library augmented with a drupal.base.css library.

If a module attaches a CSS file to a template file, the CSS file should be named the same as the template file, e.g. the system-plugin-ui-form.html.twig CSS file should be named system-plugin-ui-form.css

### CSS files for Drupal themes

1.  Always separate Base, Layout, and Component styles into their own files. (Drupal will aggregate these separate files into one file, so there is no performance problem with this practice.)
2.  For complex themes, consider placing each component or component family in its own file.
3.  State rules, including media queries, should be included with the component to which they apply.
4.  Theme rules may or may not have their own file(s). [Starter themes](http://drupal.org/node/323993) are encouraged to separate their theme CSS into separate files. Otherwise, include theme rules with their corresponding component.

Most themes will have at least the following:

```yaml
css:
 base: base.css
 layout: layout.css
 component: components.css
```

Optionally, these can be further broken out. Below the base/layout/component level, naming is up to the module or theme. A more complex file structure using normalize.css and a mobile-first approach might look like this:

```yaml
css:
 base: css/base/normalize.css css/base/elements.css
 layout: css/layout/layout.css css/layout/layout--medium.css css/layout/layout--wide.css
 component: css/components/button.css css/components/dropdown.css css/components/pagination.css css/components/tabs.css …
 theme: css/theme/theme--light.css css/theme/theme--dark.css
```

Note: Module styles are always loaded before theme styles and the SMACSS groups cannot be interlaced between modules and themes. For example, loading a normalize.css in a theme's "base" SMACSS group will appear in the CSS cascade order only _after_ all other stylesheets defined in core and contrib modules even when they're defined in a higher SMACSS group like "component" and "theme". Follow [#3046636: LibraryDiscoveryParser overwrites theme CSS group ordering](https://www.drupal.org/project/drupal/issues/3046636 "Status: Needs work") for more information.

## Aggregating CSS

In Drupal 7 and earlier, the “group” option and the “every page” option of `drupal_add_css()` controlled which files would be aggregated together. The groups were `CSS_SYSTEM`, `CSS_DEFAULT` (for Drupal modules), and `CSS_THEME` (for Drupal themes) and the `every_page` option was a simple boolean flag. This meant there were up to 6 aggregated files.

In Drupal 8 and later, themes now have the ability to override stylesheets without affecting the “every page” option of the original stylesheet. See [http://drupal.org/node/1876600](http://drupal.org/node/1876600). This means conditionally-loaded CSS remains conditionally loaded.

This also means it’s now simple for themes to affect conditionally-loaded styles, so all the theme-added CSS no longer needs to load after all conditionally-loaded styles. This greatly simplifies the default grouping strategy that Drupal can employ. Instead of 6 aggregated files, we can just have 2:

1. The “Every page” aggregate. Includes:

   1. Base styles from libraries (like Normalize.css and drupal.base.css) and then Drupal themes.
   2. Layout styles from Drupal modules and then Drupal themes.
   3. Component styles (and their associated states and themes) from Drupal modules and then Drupal themes.
   4. State styles that are not included with components, from Drupal modules and then Drupal themes.
   5. Theme styles that are not included with components, from Drupal modules and then Drupal themes.
2. The conditionally-loaded aggregate. Includes:

   1. Layout styles from Drupal modules and then Drupal themes.
   2. Component styles (and their associated states and theme) from Drupal modules and then Drupal themes.
   3. State styles that are not included with components, from Drupal modules and then Drupal themes.
   4. Theme styles that are not included with components, from Drupal modules and then Drupal themes.

_Note: there should never be conditionally-loaded base styles._

The order of styles within an aggregate are determined by the “weight” option of `drupal_add_css()` which will be:

```php
/**
 * The default weight for CSS rules that style HTML elements ("base" styles).
 */
const CSS_BASE = -200;

/**
 * The default weight for CSS rules that layout a page.
 */
const CSS_LAYOUT = -100;

/**
 * The default weight for CSS rules that style design components (and their associated states and themes.)
 */
const CSS_COMPONENT = 0;

/**
 * The default weight for CSS rules that style states and are not included with components.
 */
const CSS_STATE = 100;

/**
 * The default weight for CSS rules that style theme and are not included with components.
 */
const CSS_THEME = 200;
```

The aggregated file itself is determined by the "group" option and the "every page" flag of drupal_add_css(). The group option defaults to the `CSS_AGGREGATE_DEFAULT` constant. Drupal core does not provide any other aggregate constants, but contrib can define their own aggregate groups.

### SMACSS and Sass/Compass

SMACSS when combined with Sass/Compass concept can provide much control over output css. We can use Sass Globbing concept to have multiple .scss files and only one css file with the use of partials.
