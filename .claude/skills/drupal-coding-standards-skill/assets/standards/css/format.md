## Whitespace

### Indentation

Use 2 spaces for each level of indentation, the same standard as Drupal’s PHP and JavaScript code.

* Declarations (property/value pairs) should be indented one level relative to their selector.
* Rulesets within a media query should be indented one level relative to the media statement.
* Comments should maintain the indentation of their declaration or ruleset.

```php
.tabs__tab {
  display: none;
  margin: 0;
  margin-block-end: calc(-1 * var(--tabs-border-width));

  &.is-active {
    display: flex;
  }
}
```

### Blank lines

* In general, separate each ruleset by a blank line when using PostCSS.
* If a ruleset has a preceding Doxygen-style or single-line-style comment that describes it, place a blank line before the comment.
* If two rulesets have no interleaving blank line, they must be logically related. If they are not logically related to each other, add a blank line and a comment describing the second ruleset.

```php
.tabs {
  --tabs-height: var(--sp3);
  --tabs-padding-inline: var(--sp1-5);
  --tabs-active-border-size: 6px;
  --tabs-highlight-color: var(--color--primary-50); /* Minimum 3:1 contrast ratio against --tabs-background-color and --tabs-background-color-hover. */

  display: flex;
  flex-direction: column;
}

/**
 * I'm a Doxygen-style comment. Hooray!
 */
.tabs__tab {
  display: none;
  margin: 0;
  margin-block-end: calc(-1 * var(--tabs-border-width));

  /* Show tabs when JavaScript disabled. */
  @nest html:not(.js) & {
    display: flex;
  }
```

### Line endings

* There MUST NOT be any whitespace (spaces or tabs) at the end of lines.
* All text files should end with a single blank line.
* Files should be formatted with Unix line endings (a newline character, denoted as `\n` or `LF`), which is also the default in Mac OS X.

Tip: configure your editor to “show invisibles”. This will allow you to eliminate end-of-line whitespace, eliminate unintended blank-line whitespace, and avoid polluting commits.

Drupal 8 and above includes an [EditorConfig](http://editorconfig.org/) file in [its root directory](https://git.drupalcode.org/project/drupal/-/tree/11.x?ref_type=heads) to help maintain these whitespace conventions.

## Comments

Well commented code is extremely important. Take time to describe components, how they work, their limitations, and the way they are constructed. Don't leave others guessing as to the purpose of uncommon or non-obvious code.

To stay consistent with the rest of Drupal's code base, we borrow some of the CSS comment styles from the [Doxygen and comment formatting conventions](../php/documentation.md) for PHP files.

### File comments

Each file should start with a comment describing what the file does. Note that a blank line should follow a file comment. And keep line-lengths to 80 columns, when possible. For more information, see the [PHP file comment standards](../php/documentation.md).

### Single line comments describing a ruleset

Short comments describing a ruleset can be kept to one line.

### Multi-line comments describing a ruleset

When describing a ruleset or set of rulesets, any comment that requires 2 or more lines (wrapped to 80 characters) must follow the Doxygen comment style (also called a “docblock”).

### Multi-line comments inside a ruleset

Within a ruleset, multi-line comments are preceded with a `/*` and terminated by a `*/`. Text is intended to maintain the text’s left alignment.

### Single-line comments

When describing a property or ruleset, any comment that can be written inside the 80 character line length limit can use a simple CSS comment style.

Example of all comment styles:

```php
/*
 * @file
 * Example CSS file to demonstrate all variations of CSS comments. This file uses
 * modern syntax including nesting, and `:dir()` pseudo-class that PostCSS
 * transpiles into browser-readable CSS.
 */

.component {
  position: relative; /* Anchor pseudo-element. */
  z-index: 502; /* Appear above toolbar. */
  /* This is a multiline comment WITHIN a ruleset. It describes the next rule
     when there is not enough space on the same line as the rule. We try to
     make it not go very far over 80 characters. We want to make it very verbose
     to help out future developers. */
  visibility: hidden;
  transform: translateX(50%); /* LTR */
  border-radius: var(--border-radius);
  will-change: transform; /* Needed for Safari 16. */

  /*
   * This is a multiline comment that describes an entire ruleset (even if
   * nested). We try to make it not go very far over 80 characters. We want to
   * make it very verbose to help out future developers.
   */
  &[aria-expanded="true"] {
    visibility: visible;
  }

  /* Short comment outside ruleset. */
  &::marker {
    display: none;
  }

  &:dir(rtl) {
    transform: translateX(-50%);
  }
}
```

## Properties where browsers do not have or support CSS logical properties

Certain properties do not have a “logical properties” equivalent (such as `transform`). For these direction specific rules, add a `/* LTR */` comment on the same line preceded by a single space. Follow with an additional ruleset (or nested ruleset if using PostCSS) containing the inverse property/values.

```php
.my-component {
  transform: translateX(20px); /* LTR */
}

[dir="rtl"] .my-component {
  transform: translateX(-20px);
}
```

## Rulesets

*   Use one selector per line when a ruleset has a group of selectors separated by commas.
*   When possible, consider using functional pseudo-classes like `:is()`, `:not()or` `:where()` that allow combining of selectors.
*   Include one declaration per line in a declaration block.

```php
/**
 * Example of one selector per line.
 */
.my-component,
.my-other-component,
.yet-another-component {
  background-color: #cccccc;
}

/**
 * Example of the :is() pseudo-class.
 */
:is(.my-component, .my-other-component, .yet-another-component) {
  background-color: #cccccc;
}
```

## Properties

* In a declaration, the property name should be immediately followed by a colon, then a single space, and then the property’s value.
* Include a semicolon at the end of all declarations.
* For property values that require quotes, use double quotes instead of single quotes, e.g. font-family: "Arial Black", Arial, sans-serif; and content: " ";.
* Default to rem units, unless it creates an undesired effect.
* Note that if you are using PostCSS, this is automatic via the PostCSS PxToRem plugin.
* Quote attribute values in selectors, e.g. input[type="checkbox"].
* Where allowed, avoid specifying units for zero-values, e.g. use `margin: 0;` instead of `margin: 0px;`.
* Include a space after each comma in comma-separated property or function values.
* Do not use spaces around the parentheses in a function, e.g. color: rgba(0, 0, 0, 0.8);
* Use lower case function names, correct: color: rgba(0, 0, 0, 0.8);

## Declaration order

The declarations in a ruleset should be ordered so that the purpose of the declaration block is most obvious. Clarity should be the guiding principle.

1. Positioning properties include: `position`, `float`, `clear`, `inset`, `top`, `right`, `bottom`, `left`, `direction`, and `z-index` plus logical properties like `block-start`, `block-end`, `inline-start` and `inline-end`.
2. Box model properties include:
   1. display
   2. sizing (like block-size or inline-size as logical properties, and width and height and the (max|min) variation or each of them)
   3. Margins and the logical property equivalents, plus their various longhand forms (margin-block, margin-top, margin-inline-end…)
   4. Paddings and the logical property equivalents, plus their various longhand forms (padding-block, padding-top, padding-inline-end…)
   5. Borders and the logical property equivalents, plus their various longhand forms.
   6. box-sizing
3. Other declarations.

Within each of the above groups, properties can be grouped alphabetically or grouped with like properties next to each other, e.g. putting font and text properties next to each other. Drupal’s coding standards are purposefully vague here because there is no consensus on this issue (as of 2013), but we respect each other’s abilities and preferences.

If not automatically added by autoprefixer (which is part of the PostCSS build process), vendor prefixed properties should be directly before their non-prefixed version. This allows the official version of the property to override any inconsistencies in the vendor-prefixed versions once those browsers implement the official property. If browser bugs or cross-browser issues necessitate any deviation from this ordering, it should be clearly documented.

Again, the order of properties is meant to reinforce the purpose of the ruleset. As such, it is much more important to add comments to the ruleset than to worry about property ordering.

- - -

The text of this guideline was originally based on the [_Principles of writing consistent, idiomatic CSS_](https://github.com/necolas/idiomatic-css) by Nicolas Gallagher.
