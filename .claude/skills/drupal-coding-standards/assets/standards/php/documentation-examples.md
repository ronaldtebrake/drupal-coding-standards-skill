This page is a collection of the complete API documentation examples, which you can use as starting points to writing documentation that conforms to the Drupal project's [API documentation standards](documentation.md).

## Files

[General standards for file documentation](documentation.md#file-documenting-files)

### Module file (*.module)

```php
<?php

/**
 * @file
 * Attaches custom data fields to Drupal entities.
 */
```

### Install file (*.install)

```php
<?php

/**
 * @file
 * Install, update and uninstall functions for the System module.
 */
```

### Include file (*.inc)

```php
<?php

/**
 * @file
 * Media module integration for the Media module.
 */
```

### PHP theme template file (*.tpl.php -- base implementation)

[Special standards for tpl.php files](documentation.md#theme-template-files)

```php
<?php

/**
 * @file
 * Displays a block.
 *
 * Available variables:
 * - $block->subject: Block title.
 * (list the other variables here)
 *
 * @see template_preprocess_block()
 *
 * @ingroup themeable
 */
(HTML/PHP code for template starts here)
```

### File containing a single class

[Object-oriented coding standards](index.md).

```php
<?php

namespace Drupal\commerce\Element;

use Drupal\commerce\EntityHelper;
use Drupal\Core\Form\FormStateInterface;
use Drupal\Core\Render\Element\FormElement;

/**
 * Provides a form input element for selecting one or multiple entities.
 *
 * The element is transformed based on the number of available entities:
 *   1..#autocomplete_threshold: Checkboxes/radios element, based on #multiple.
 *   >#autocomplete_threshold: entity autocomplete element.
 * If the element is required, and there's only one available entity, a hidden
 * form element can be used instead of checkboxes/radios.
 *
 * Properties:
 * - #target_type: The entity type being selected.
 * - #multiple: Whether the user may select more than one item.
 * - #default_value: An entity ID or an array of entity IDs.
 * - #hide_single_entity: Whether to use a hidden element when there's only one
 *                        available entity and the element is required.
 * - #autocomplete_threshold: Determines when to use the autocomplete.
 * - #autocomplete_size: The size of the autocomplete element in characters.
 * - #autocomplete_placeholder: The placeholder for the autocomplete element.
 *
 * Example usage:
 * @code
 * $form['entities'] = [
 *   '#type' => 'commerce_entity_select',
 *   '#title' => t('Stores'),
 *   '#target_type' => 'commerce_store',
 *   '#multiple' => TRUE,
 * ];
 * @end
 *
 * @FormElement("commerce_entity_select")
 */
class EntitySelect extends FormElement {
```

### PHP theme template file (*.tpl.php -- theme-specific override )

[Special standards for tpl.php files](documentation.md#theme-template-files) Note that there is no @ingroup themeable in the override!

```php
<?php

/**
 * @file
 * Displays a block.
 *
 * Available variables:
 * - $block->subject: Block title.
 * (list the other variables here)
 *
 *
 * @see template_preprocess_block()
 */
(HTML/PHP code for template starts here)
```

## Functions

[Documentation standards for functions](documentation.md#functions)

### Generic functions

```php
/**
 * Returns data from the persistent cache.
 *
 * Data may be stored as either plain text or as serialized data. cache_get
 * will automatically return unserialized objects and arrays.
 *
 * @param int $cid
 *   The cache ID of the data to retrieve.
 * @param string $bin
 *   The cache bin to store the data in. Valid core values are 'cache_block',
 *   'cache_bootstrap', 'cache_field', 'cache_filter', 'cache_form',
 *   'cache_menu', 'cache_page', 'cache_path', 'cache_update' or 'cache' for
 *   the default cache.
 *
 * @return mixed
 *   The value from the cache, or FALSE on failure.
 *
 * @see cache_set()
 */
function cache_get($cid, $bin = 'cache') {
```

### Callback functions

[Standards for documenting callback functions](documentation.md#callbacks-for-built-in-php-functions) -- these are standard-format callback functions that are passed to other functions as arguments.

Callback used in only one API function:

```php
/**
 * Sorts structured arrays by weight.
 *
 * Callback for uasort() within foo_bar().
 */
function element_sort($a, $b) {
```

Callback used in a few API functions:

```php
/**
 * Sorts structured arrays by weight.
 *
 * Callback for uasort() within:
 * - foo()
 * - bar()
 */
function element_sort($a, $b) {
```

Callback used in many functions, where some explanation is needed for the otherwise standard function arguments:

[NOTE: NEEDS STANDARDS UPDATE - data types on @param/@return!]

```php
/**
 * Sorts a structured array by the 'weight' element.
 *
 * Note that the sorting is by the 'weight' array element, not by the render
 * element property '#weight'.
 *
 * Callback for uasort() used in various functions.
 *
 * @param $a
 *   First item for comparison. The compared items should be associative arrays
 *   that optionally include a 'weight' element. For items without a 'weight'
 *   element, a default value of 0 will be used.
 * @param $b
 *   Second item for comparison.
 */
function drupal_sort_weight($a, $b) {
```

### Hook definition functions

[Standards for documenting hook definitions](documentation.md#hook-definition)

[NOTE: NEEDS STANDARDS UPDATE - data types on @param/@return, and function body needs to be provided, as it's part of the documentation.]

```php
/**
 * Define the Field API schema for a field structure.
 *
 * This hook MUST be defined in .install for it to be detected during
 * installation and upgrade.
 *
 * @param $field
 *   A field structure.
 *
 * @return
 *   An associative array with the following keys:
 *   - columns: An array of Schema API column specifications, keyed by column
 *     name. This specifies what comprises a value for a given field. For
 *     example, a value for a number field is simply 'value', while a value for
 *     a formatted text field is the combination of 'value' and 'format'. It is
 *     recommended to avoid having the column definitions depend on field
 *     settings when possible. No assumptions should be made on how storage
 *     engines internally use the original column name to structure their
 *     storage.
 *   - indexes: (optional) An array of Schema API indexes definitions. Only
 *     columns that appear in the 'columns' array are allowed. Those indexes
 *     will be used as default indexes. Callers of field_create_field() can
 *     specify additional indexes, or, at their own risk, modify the default
 *     indexes specified by the field-type module. Some storage engines might
 *     not support indexes.
 *   - foreign keys: (optional) An array of Schema API foreign keys
 *     definitions.
 */
function hook_field_schema($field) {
```

### hook implementation

[Standards for documenting hook implementations](documentation.md#hook-implementation)

```php
/**
 * Implements hook_menu().
 */
function search_menu() {
```

### hook_update_N implementation (update function)

[Drupal API documentation standards for functions](documentation.md#functions)

```php
/**
 * Upgrade the node type table and fix node type 'module' attribute to avoid name-space conflicts.
 */
function node_update_7000() {
  // Rename the module column to base.
```

### form-generating function (including validate/submit)

[Form-generating functions](documentation.md#form-generating-functions)

[needs an example]

###  callbacks

[needs an example]

[Standards for documenting hook_menu() callbacks](documentation.md#hook_menu-page-router-callback-functions)

[NEEDS STANDARDS UPDATE - data type on parameter, and it needs @return as well!!]

```php
/**
 * Page callback: Displays the dashboard.
 *
 * @param $launch_customize
 *   Whether to launch in customization mode right away. TRUE or FALSE.
 */
function dashboard_admin($launch_customize = FALSE) {
```

### render API callback

[Render API callback functions](documentation.md#render-api-callback-functions)

(needs an example)

###  themeable function

[Standards for documenting themeable functions](documentation.md#themeable-functions)

```php
/**
 * Returns HTML for a form.
 *
 * @param array $variables
 *   An associative array containing:
 *   - element: An associative array containing the properties of the element.
 *     Properties used: #action, #method, #attributes, #children
 *
 * @ingroup themeable
 */
function theme_form($variables) {
```

## Classes

[Standards for documenting classes and interfaces](documentation.md#classes-and-namespaces)

### Class with a namespace

```php
<?php

namespace Drupal\Core\Plugin\Context;

use Drupal\Component\Plugin\Exception\ContextException;
use Drupal\Core\Cache\CacheableDependencyInterface;
use Drupal\Core\Plugin\ContextAwarePluginInterface;

/**
 * Provides methods to handle sets of contexts.
 */
class ContextHandler implements ContextHandlerInterface {
```

### Class without a namespace

(needs example)

### Interface

[NEEDS STANDARDS UPDATE - first line does not conform to standards]

```php
namespace Drupal\Component\Plugin;

use Drupal\Component\Plugin\Exception\PluginException;

/**
 * Interface for defining context aware plugins.
 *
 * Context aware plugins can specify an array of context definitions keyed by
 * context name at the plugin definition under the "context" key.
 */
interface ContextAwarePluginInterface extends PluginInspectionInterface {
```

### Member function

(needs example)

### Member function that overrides base class method

[NEEDS STANDARDS UPDATE: first line needs class name and probably namespace]

```php
  /**
   * Overrides prepareTimezone().
   *
   * Override basic component timezone handling to use Drupal's
   * knowledge of the preferred user timezone.
   */
  protected function prepareTimezone($timezone) {
```

### Member function that implements interface method

```php
  /**
   * Implements ArrayAccess::offsetExists().
   */
  public function offsetExists($offset) {
```

### Member constant

(needs example)

### Member variable

(needs example)

### Plugin annotation

[Standards for plugin annotation](documentation.md#annotation-plugin-etc-plugin-discovery-annotations)

(needs example)

## Miscellaneous examples

### Constant

(needs link to standards)

[NEEDS GRAMMAR UPDATE - this example is not well written. Maybe pick a different constant? Also, where did this come from? We're looking for a define() here I think, not a class constant.]

```php
/**
 * The block or element is the same for every user and page that it is visible.
 */
const DRUPAL_CACHE_GLOBAL = 0x0008;
```

### Global variable

(needs example, and you might note that these are documented in separate api.php files)

### Bullet lists

[Standards for using lists](documentation.md#lists-in-documentation)

(needs example)

### Sections, sub-sections, and in-page references

[Standards for using sections](documentation.md#section-subsection-documentation-sections)

(needs example)

### Code samples (@code)

[Standards for using @code](documentation.md#code-code-samples)

```php
 * For example, one might wish to return a list of the most recent 10 nodes
 * authored by a given user. Instead of directly issuing the SQL query
 * @code
 * SELECT n.nid, n.title, n.created FROM node n WHERE n.uid = $uid LIMIT 0, 10;
 * @endcode
```

### @link

[Standards for using @link](documentation.md#link-html-links)

```php
 * This framework creates a PHP macro language that allows the server to
 * instruct JavaScript to perform actions on the client browser. When using
 * forms, it can be used with the #ajax property.
 * The #ajax property can be used to bind events to the Ajax framework. By
 * default, #ajax uses 'system/ajax' as its path for submission and thus calls
 * ajax_form_callback() and a defined #ajax['callback'] function.
 * However, you may optionally specify a different path to request or a
 * different callback function to invoke, which can return updated HTML or can
 * also return a richer set of
 * @link ajax_commands Ajax framework commands @endlink.
```

### @see

[Standards for using @see](documentation.md#see-see-also-references)

```php
/**
 * Respond to a custom menu creation.
 *
 * This hook is used to notify modules that a custom menu has been created.
 * Contributed modules may use the information to perform actions based on the
 * information entered into the menu system.
 *
 * @param \Drupal\system\Plugin\Core\Entity\Menu $menu
 *   A menu entity.
 *
 * @see hook_menu_update()
 * @see hook_menu_delete()
 */
function hook_menu_insert($menu) {
```

### Defining a topic/group

[Standards for using @defgroup, @ingroup, and @addtogroup](documentation.md#defgroup-addtogroup-ingroup-groups-and-topics)

(needs example)

### @ingroup and @addtogroup

[Standards for using @defgroup, @ingroup, and @addtogroup](documentation.md#defgroup-addtogroup-ingroup-groups-and-topics)

```php
 * @ingroup php_wrappers
 */
function drupal_parse_url($url) {
```

(needs example for addtogroup)
