# CSS architecture (for Drupal 9)

Note: This document aims to apply emerging best-practices for CSS to Drupal 8/9. As we implement these ideas in Drupal 8, this document may need to be updated.

[Skip to best practices](#best-practices)

## Goals

The goals of good CSS should not be so different from those of good software engineering. Well-architected CSS, like PHP or JavaScript, should be:

### 1. Predictable

CSS throughout Drupal core and contributed modules should be consistent and understandable. Changes should do what you would expect without side-effects.

### 2. Reusable

> CSS rules should be abstract and decoupled enough that you can build new components quickly from existing parts without having to recode patterns and problems you’ve already solved. – _Philip Walton, [CSS Architecture](http://engineering.appfolio.com/2012/11/16/css-architecture/)_

### 3. Maintainable

As new components and features are needed, it should be easy to add, modify and extend CSS without breaking (or refactoring) existing styles.

### 4. Scalable

CSS should be easy to manage for a single developer or for large, distributed teams (like Drupal’s).

## The Component

Components are the discrete, purpose-built visual elements that make up the UI of a site or app. Components consist of HTML, CSS, and often – but not always – JavaScript. They are our navbars, dialogs, buttons and carousels. Components can be simple (such as icon containers and buttons) or complex enough to be themselves composed of other components.

## Common CSS Pitfalls

To better understand the best practices provided below, it can be helpful to review some common approaches that impede our goals of predictability, maintainability, reusability and scalability.

### Pitfall: Modifying components based on context

```php
/* Modifying a component when it’s in a sidebar. */
.sidebar .component {}
```

This may seem natural, but actually makes CSS less predictable and maintainable. Sooner or later you’re going to need that component style somewhere other than a sidebar! Or, the reverse may happen: a new developer places a component in the sidebar and gets an unexpectedly different appearance.

### Pitfall: Relying on HTML structure

Mirroring a markup structure in our CSS selectors makes the resulting styles easy to break (with markup changes) and hard to reuse (because it’s tied to very specific HTML). This pitfall comes in several forms:

*   Overly complex selectors: `nav > ul > li > a`, `article p:first-child`
*   Qualified selectors: `a.button`, `ul.nav`

### Pitfall: Overly generic class names

Similar to the pitfall of styling a component based on context, it’s common to ‘scope’ a component’s parts under the parent component using a descendant selector:

```php
.widget {}
.widget .title {}
.widget .content {}
```

This CSS might seem economical, but tends to be counterproductive: `.title` and `.content` are too generic. A stand-alone `.title` component created later will affect widget titles, likely without intending to.

### Pitfall: Making a rule do too much

Applying positioning, margins, padding, colors, borders and text styles all in a single rule overloads the rule, making it difficult or impossible to reuse if some parts (say, background, borders and padding) need to be applied to a similar component later on.

### Pitfall: Needing to undo styles

Creating style rules that undo other rules, like `.component-no-padding` makes CSS over-complex, hard to understand and maintain, and bloats the stylesheet. Needing such styles usually indicates that some existing rules are doing too much.

## Best Practices

### 1. Avoid reliance on HTML structure

* CSS should define the appearance of an element anywhere and everywhere it appears.
* Use classes to assign appearance to markup. Never use id selectors in CSS.
* Keep selectors short. The best selector is a single class or element!
* Sometimes multi-part selectors are pragmatic. For example:
    ```php
    /**
      * Add a horizontal rule between adjacent list rows
      *
      * Could be part of an implementation of the Pears “slats” component:
      * http://pea.rs/content/slats-html5
      */
    .slat + .slat {
      border-top: 1px solid #cccccc;
    }
    ```

    However, extra care should be taken when using multi-part selectors:
    1. Avoid elements with no native semantics (div, span) in multi-part selectors.
    2. Avoid the descendent selector (e.g. `.my-list li`) where possible, especially for components that may wrap other components. The descendant selector has a habit of unintentionally affecting nested elements. Prefer the child selector: `.my-list > li`.
    3. Avoid more than 2 [combinators](http://reference.sitepoint.com/css/combinators) in a selector. The following rule is maxed out: `.my-list > li > a`.
    4. If in doubt, add a class and style the element directly.

###  using their own classes

To avoid relying on markup structure and overly-generic class names, define a component’s element explicitly, prefixing them with the component’s name followed by two underscores:

```php
.component {}

/* Component elements */
.component__header {}
.component__body {}
```

**Note** that there is no need to reflect DOM structure in the class name; for example, do not replace `.menu li a` with `.menu__item__link`. The class `.menu__link` should be sufficiently specific.

### 3. Extend components using modifier classes

Create component variants explicitly, adding a suffix with the variant name preceded by two dashes. In order to keep the stylesheet DRY, this modifier class should only contain the styles needed to extend the original. This means that **both** base and modifier classes **must** appear together in the markup:

CSS

```php
/* Button component */
.button {
  /* styles */
}

/* Button modifier class */
.button--primary {
  /* modifications and additions */
}
```

HTML

```php
<!-- Button variant is created by applying both component and modifier classes -->
<button class="button button--primary">Save</button>
```

### 4. Separate Concerns

Components should not be responsible for their positioning or layout within the site. Never apply widths or heights except to elements that natively have these properties (e.g. images have these properties, so it's okay to use CSS to modify their width and height). Within components, separate structural rules from stylistic rules.

Separate style from behavior by using dedicated classes for JavaScript manipulation rather than relying on classes already in use for CSS. This way, we can modify classes for style purposes without fear of breaking JS, and vice versa. To make the distinction clear, classes used for JavaScript manipulation should be prefixed with 'js-'. These JavaScript hooks must never be used for styling purposes. See the section ‘[Formatting Class Names](#6-formatting-class-names)’ for more information on naming conventions.

Avoid applying inline styles using JavaScript. If the behavior is describing a state change, apply a class name describing the state (e.g. 'is-active'), and allow CSS to provide the appearance. Only use inline styles applied via JavaScript when the value of the style attributes must be computed at runtime.

Drupal **8/9** uses the [SMACSS](http://smacss.com/book/) system to conceptually categorize CSS rules. Note that some SMACSS nomenclature has been changed to avoid confusion with existing Drupal terminology.

1.  #### [Base]
    Base rules consist of styling for HTML elements only, such as used in a CSS reset or [Normalize.css](https://github.com/necolas/normalize.css/). Base rules should never include class selectors.

    To avoid ‘undoing’ styles in components, base styles should reflect the simplest possible appearance of each element. For example, the simplest usage of the `ul` element may be completely unstyled, removing list markers and indents and relying on a component class for other applications.

2.  #### [Layout]

    Arrangement of elements on the page, including grid systems.

    > Grid systems should be thought of as shelves. They contain content but are not content in themselves. You put up your shelves then fill them with your stuff [i.e. components]. – _Harry Roberts, [CSS Guidelines](https://cssguidelin.es/)_

3.  #### [Component (SMACSS “module”)]

    Reusable, discrete UI elements; components should form the bulk of Drupal’s CSS.

4.  #### [State]

    Styles that deal with transient changes to a component’s appearance. Often, these are client-side changes that occur as the user interacts with the page, such as hovering links or opening a modal dialog. In some cases, states are static for the life of the page and are set from the server, such as the active element in main navigation. The main ways to style state are:

    * Custom classes, often but not always applied via JavaScript. These should be prefixed with `.is-`, e.g. `.is-transitioning`, `.is-open`;
    * pseudo-classes, such as `:hover` and `:checked`;
    * HTML attributes with state semantics, such as `details[open]`;
    * media queries: styles that alter appearance based on the immediate browser environment.
5.  #### [Theme]

    Purely visual styling, such as border, box-shadow, colors and backgrounds, font properties, etc. Ideally, these should be separated enough from a component’s structure to be “swappable”, and omitting these entirely should not break the component’s functionality or basic usability.

### 5. Name Components Using Design Semantics

While the HTML5 specification mentions that class names should “describe the nature of the content,” there’s no reason for this. HTML elements already impart semantics on the content and machines cannot derive content-level semantics from class names (with the narrow exception of [microformats](http://microformats.org/).)

> Class names should communicate useful information to developers. – _Nicolas Gallagher, [About HTML Semantics and Front-End Architecture](http://nicolasgallagher.com/about-html-semantics-front-end-architecture/)_

Class names used as CSS hooks should reflect _design_ semantics over content semantics. In general, they should reflect the intent and purpose of the design element they represent.

Note that this does not preclude presentational class names. Grid system classes such as `.grid-3`, utility classes such as `.leader` and `.trailer` (for adding whitespace based on a [baseline grid](http://alistapart.com/article/settingtypeontheweb)) and `.text-center` are all examples of presentational classes that represent visual semantics. They are meaningful to developers, and highly reusable.

This does not mean going back to classes like `.blue-box`. This is obviously a bad class name since it does not reflect the visual meaning, only one very surface attribute. It’s useful to ask “why is this a box, and why blue?”. Thinking about this, we might realize that this box is actually a version of the style used throughout our site for notifications, and this particular shade of blue is used for non-urgent notifications:

```php
.notification { /* general styles for all notifications */ } .notification--info { /* blue color adjustments */ }
```

#### Note for core developers

Since classes should represent design semantics and Drupal core must be design-agnostic, core default markup should be exceedingly cautious about what classes are included. This applies especially to the use of presentational class names.

#### Note for module developers

Since modules are responsible for providing the default HTML implementation, module developers should make their best effort to find an existing theme hook to use and to insert a design-derived class name, possibly one had already found in core. If the module’s content has no default design, the class name should be based on how the content is built; often this can just be the name of the module (e.g. `class="views"`.)

Module developers should ensure that themers can replace/augment any module-provided class.

### 6. Formatting Class Names

Class names should use full words rather than abbreviations. Styles for a button component should use e.g. `class="button"` rather than `class="btn"`

Class names for components should always use a dash between words. Use `class="button-group"` rather than `class="buttongroup"`

The HTML class attribute has been made to do a lot. Drupal uses a naming convention to make clear what a particular class is for and what other parts of the system it affects:

```php
/* Component Rules */
.component-name
.component-name--variant
.component-name__sub-object
.component-name__sub-object--variant  /* this configuration should be uncommon */

/* Layout Rules */
.layout-layout-method  /* e.g. '.layout-container' */
.grid-*

/**
 * State Classes
 *
 * These are always applied via JavaScript, and describe a non-default state.
 */
.is-state  /* e.g. '.is-active' */

/**
 * Functional JavaScript Hooks
 *
 * When querying or manipulating the DOM from JavaScript, prefer dedicated
 * classes not used for styling (or the id attribute).
 * If using classes, prefix them with 'js-' to mark them for JS use.
 * These 'js-' classes should not appear in stylesheets.
 */
.js-behavior-hook  /* e.g. .js-slider, .js-dropdown */
```

### Closing Note: Specificity, ids and `!important`

Avoid using the id selector in CSS. There is no general benefit to using the 'id' attribute as a CSS hook, and it has serious downsides in the form of increased selector specificity.

Although it should be avoided in CSS, there are many excellent uses for the 'id' attribute:

* A performant JavaScript hook;
* An anchor within the document for links ([http://yoururl.com#anchor](http://yoururl.com#anchor));
* An anchor for associating labels with form elements or for associating DOM elements using ARIA attributes.

The `!important` flag should be used sparingly and appropriately, and in general should be restricted to themes. Since it overrides all external stylesheet rules, it is useful for states that must supersede all others, no matter the component variant. For example, error or disabled states are appropriate places to use `!important`, since they must be consistent and always apply when present. Never use `!important` to resolve specificity problems for general CSS rules. Additionally, Drupal core and contrib modules should avoid the `!important` flag, since all module CSS should be easy to override.

## Case Study

As an example and guide to developers, take the progress bar from the [proposed Seven style guide](http://groups.drupal.org/node/283223#Progress):

![Progress bar component and small variant](files/10.progress.png)

This might be marked up and styled as follows, using the standards described above:

HTML

```php
<div class="progress">
  <!--
    .label is a separate component, used for form labels, but can be applied
    to non-label tags, and can be extended with a modifier class.
    -->
  <label class="label">Installing Node Module</label>

  <!-- progress element (sub-object) -->
  <div class="progress__track">
    <!--
      Component sub-objects don’t need more than the component name
      as a prefix; progress__track__bar is verbose and unnecessary.

      Inline styles shown here would be applied by JavaScript using
      the independent 'js-' class.
      -->
    <div class="progress__bar js-progress-percent" style="width: 63%"></div>
  </div>
  <div class="progress__description">
    <!--
      .layout-pull and .layout-push are LTR/RTL-agnostic utility classes for floating;
      these should not appear in core default markup since they are not
      design-independent.
      -->
    <div class="layout-pull">Installed 15 of 24 modules</div>
    <strong class="layout-push">63%</strong>
  </div>

  <!--
    Note that appearance is not tied to the tag; the appearance of the
    <button> element would be reset in the base layer and styled as
    needed by applying classes.

    Since this element uses an icon, it could be refactored to use a generic
    icon container component.
    -->
  <button class="progress__cancel" href="#" title="cancel">cancel</button>
</div>

<!--
  Modifier classes create component variants; modifier classes must
  be used with the original class in the markup
  -->
<div class="progress progress--small">
  <label class="label label--small">Uploading sydney-opera-house-sunset.jpg</label>
  <div class="progress__track">
    <div class="progress__bar js-progress-percent" style="width: 29%"></div>
  </div>
  <div class="progress__description">
    <div class="layout-pull">Uploaded 221 of 762KB</div>
    <strong class="layout-push">29%</strong>
  </div>
  <a class="progress__cancel" href="#" title="cancel"><span class="visually-hidden">cancel</span></a>
</div>
```

CSS (styles omitted)

```php
/**
 * Progress Bar component
 */
.progress {}
.progress__track {}
.progress__bar {}
.progress__description {}
.progress__cancel {}
.progress__cancel:focus,
.progress__cancel:hover {}

/**
 * Progress Bar small variant
 */
.progress--small .progress__track {}
.progress--small .progress__bar {}
.progress--small .progress__cancel {}
```

Note that the CSS relies almost exclusively on single class selectors; the exception is the small variant. This uses descendant selector, but is the recommended approach for styling sub-objects within variant components. Here’s why: firstly, it avoids extreme class names such as .progress--small__bar which begin to harm the understandability of the code. Second, since we have avoided overly-generic class names and since both parts of the selector are highly targeted classes, there is little risk of accidentally styling a further child element. Third, scoping in this case is unlikely to break reusability, since we should not expect a Progress track outside of the Progress element.

We are still serving of our goals well, and we greatly simplify the required markup: we only need to add the variant class to the component itself: `<div class="progress progress--small"><!-- all internals remain the same --></div>`.

## Acknowledgments

Portions of this guide are based heavily on:

* Philip Walton, [CSS Architecture](http://engineering.appfolio.com/2012/11/16/css-architecture/)
* Johnathan Snook, [SMACSS](http://smacss.com/book/)
* Nicolas Gallagher, [About HTML semantics and front-end architecture](http://nicolasgallagher.com/about-html-semantics-front-end-architecture/)

Additional Influence and Resources:

* Nicole Sullivan, [OOCSS](http://coding.smashingmagazine.com/2011/12/12/an-introduction-to-object-oriented-css-oocss/)
* Harry Roberts, [Code Smells in CSS](http://csswizardry.com/2012/11/code-smells-in-css/)
* Harry Roberts, [CSS-Guidelines](http://cssguidelin.es)
