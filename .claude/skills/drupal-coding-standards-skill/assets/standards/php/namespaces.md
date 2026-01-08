PHP 5.3 introduces [namespaces](http://www.php.net/namespaces) to the language. This page document shows how namespaces should be referenced within Drupal and it assumes that you are familiar with the concept of namespaces. (If not, you can have a look at this [article introducing namespaces](http://www.sitepoint.com/php-53-namespaces-basics/).)

Not all files in Drupal declare a namespace. As of Drupal 8 an increasing number of files do, but not all. Prior to Drupal 8 virtually no code used namespaces, in order to remain compatible with PHP 5.2. Therefore there are two slightly different standards.

## "use"-ing classes

*   Classes and interfaces with a backslash `\` inside their fully-qualified name (for example: `Drupal\simpletest\WebTestBase`) must not use their fully-qualified name inside the code. If the namespace differs from the namespace of the current file, put a `use` statement on the top of the file. For example:

    ```php
    namespace Drupal\my_module\Tests\Foo;

    use Drupal\simpletest\WebTestBase;

    /**
     * Tests that the foo bars.
     */
    class BarTest extends WebTestBase {
    ```

*   Classes and interfaces without a backslash `\` inside their fully-qualified name (for example, the built-in PHP Exception class) must be fully qualified when used in a namespaced file. For example: `new \Exception();`. Do not `use` global classes.
*   In a file that does not declare a namespace (and is therefore in the global namespace), classes in any namespace other than global must be specified with a "use" statement at the top of the file.
*   When importing a class with "use", do not include a leading `\`. (The [PHP documentation](http://www.php.net/manual/en/language.namespaces.importing.php) makes the same recommendation.)
*   When specifying a class name in a string, use its full name including namespace, without leading `\`.

    Escape the namespace separator in double-quoted strings: `"Drupal\\Context\\ContextInterface"`

    Do not escape it in single-quoted strings: `'Drupal\Context\ContextInterface'`

    As stated elsewhere, single-quoted strings are generally preferred.

*   Specify a single class per use statement. Do not specify multiple classes in a single use statement.
*   If there are multiple use declarations in a file, we do not currently have a standard about what order they should be in. However, consider code readability and do something sensible, especially if there are many use declarations.
*   API documentation (in .api.php files) should use full class names. Note that if a class is used more than once in multiple hook signatures, it must still be "use"ed, and then only the short names of the class should be used in the function.

Example:

```php
namespace Drupal\Subsystem;

// This imports just the Cat class from the Drupal\OtherSystem namespace.
use Drupal\OtherSystem\Cat;

// Bar is a class in the Drupal\Subsystem namespace in another file.
// It is already available without any importing.

/**
 * Defines a Foo.
 */
class Foo {

  /**
   * Constructs a new Foo object.
   */
  public function __construct(Bar $b, Cat $c) {
    // Global classes must be prefixed with a \ character.
    $d = new \DateTime();
  }

}
```

```php
/**
 * @file
 * The Example module.
 *
 * This file is not part of any namespace, so all global namespaced classes
 *  are automatically available.
 */

use Drupal\Subsystem\Foo;

/**
 * Does stuff with Foo stuff.
 *
 * @param \Drupal\Subsystem\Foo $f
 *   A Foo object used to bar the baz.
 */
function do_stuff(Foo $f) {
  // The DateTime class does not need to be imported as it is already global
  $d = new \DateTime();
}
```

## Class aliasing

PHP allows classes to be aliased when they are imported into a namespace. In general that should only be done to avoid a name collision. If a collision happens, alias both colliding classes by prefixing the next higher portion of the namespace.

Example:

```php
use Foo\Bar\Baz as BarBaz;
use Stuff\Thing\Baz as ThingBaz;

/**
 * Tests stuff for the whichever.
 */
function test() {
  $a = new BarBaz(); // This will be Foo\Bar\Baz
  $b = new ThingBaz(); // This will be Stuff\Thing\Baz
}
```

That helps keep clear which one is which, and where it comes from. Aliasing should only be done to avoid name collisions.

### Order of import

If there are more than one classes to 'use', there is no specific rule to order them.

```php
namespace Drupal\block;

use Drupal\Core\Entity\EntityForm;
use Drupal\Core\Entity\EntityManagerInterface;
use Drupal\Core\Form\FormState;
use Drupal\Core\Form\FormStateInterface;
use Symfony\Component\DependencyInjection\ContainerInterface;
```

## Modules

Modules creating classes should place their code inside a custom namespace.The convention for those namespaces begins:

Drupal\module_name\\...

[Drupal 8 supports PSR-4](psr4.md), so to permit class autodiscovery, a class in the folder:

'module folder'/src/SubFolder1/SubFolder2

should declare the namespace:

Drupal\module_name\SubFolder1\SubFolder2

Note that the /src/ subfolder is omitted from the namespace.

### Examples

* Class _Drupal\example_module\Foo_ in namespace _Drupal\example_module_ should be in a file named _example_module/src/Foo.php_
* Class _Drupal\example_module\Foo\Bar_ in namespace _Drupal\example_module\Foo_ should be in a file named _example_module/src/Foo/Bar.php_
