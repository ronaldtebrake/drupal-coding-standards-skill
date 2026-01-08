# What to look for when reviewing CSS

There are many things to consider when reviewing CSS against our CSS standards. This is a guide of things to consider and how to present the review when looks at core CSS.

## Architecture guidelines

### Is all the code still in use?

There are times when markup in core is changed but some CSS files that rely on a particular class or ID have not been updated. Check to make sure that the CSS still applies correctly.

Example: [#2405213: Remove admin-options component from Seven theme since it is not used](https://www.drupal.org/project/drupal/issues/2405213 "Status: Closed (fixed)")

### Is some code redundant?

Some CSS is written so it can override CSS that is loaded before it, as Drupal 8 becomes more aligned to the standards, there is less overlap between CSS selectors. There may be some code in place that is no longer required to override previous values. There is sometimes code that overrides browser defaults that are identical.

Example: [Refactor 'admin-panel' CSS component](https://www.drupal.org/node/2399939#comment-9509045)

>```php
>+++ b/core/themes/seven/css/components/admin-panel.css
>@@ -1,20 +1,23 @@
>+.panel,
>+.panel__content {
>   padding: 0;
>   clear: left;
> }
>```
>I think we can delete this CSS, padding is 0 by default anyway so we don't need that. I don't know what clear left does here. Let's try deleting it and seeing what comes up

### Are the components named correctly?

Does the name of the component correctly describe the design semantics?

Example: [#2399939: Refactor 'admin-panel' CSS component](https://www.drupal.org/project/drupal/issues/2399939 "Status: Closed (fixed)")

### Should the code be abstracted out into a common reusable class?

In some situations, the custom CSS written by a module or theme could be abstracted out into a component class that can be reused across modules. The aim in Drupal 8 is to have as much CSS reusable as possible, reducing the need for custom CSS and increasing consistency.

Examples:

[#2017257: Create generic layout classes](https://www.drupal.org/project/drupal/issues/2017257 "Status: Closed (fixed)")

[#2336141: Create reusable color classes](https://www.drupal.org/project/drupal/issues/2336141 "Status: Closed (fixed)")

### Are the selectors correct?

The CSS standards recommend very short and simple selectors, replacing them with component and sub-component classes.

### Is the code in the correct file?

For themes, each CSS component should live in it's own file with it's sub-components. All the CSS that effects one component should be in the same place. There are still a few situations where the styling for one element is spread out across several files.

Some files could be broken down into even smaller files.

Example: [#2408653: Remove lists.css from Bartik, add it's current code directly to the components it references.](https://www.drupal.org/project/drupal/issues/2408653 "Status: Closed (fixed)")

## Formatting guidelines

1. Add a File comment to the top of the stylesheet - see [here](format.md#file-comments) for guidelines.
2. Check any other comments are formatted correctly - see [here](format.md#comments) for guidelines.
3. Check Whitespace is being used correctly, this includes indentations and line breaks - see [here](format.md#whitespace) for guidelines.
4. Check the formatting of rulesets, properties and media queries are correct - see [here](format.md) for guidelines.
5. As mentioned above, check existing RTL styles are formatted correctly - see [here](format.md) for guidelines.
