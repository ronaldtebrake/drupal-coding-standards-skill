## Summary

Drupal 8 implements the [PSR-4 standard](https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-4-autoloader.md) for package-based PHP namespace autoloading by the PHP Framework Interoperability Group. Upgrading [Drupal 7 modules to Drupal 8](https://www.drupal.org/update/modules/7/8) will require using [PSR-4](https://www.drupal.org/taxonomy/term/51071) standards. See [more background info on Drupal 8 development here](https://www.drupal.org/getting-started-d8-bkg-prereq). Autoloading works for both modules and themes. (For themes, though, cross-path request may have an issue. See issue [#2763861](https://www.drupal.org/project/bootstrap/issues/2763861).)

Example `vegetable.module` directory structure:

* modules/vegetable/
  * css/
  * js/
  * src/
    * Controller/
      * VegetableController.php → class Drupal\vegetable\Controller\VegetableController
    * Form/
      * VegetableForm.php → class Drupal\vegetable\Form\VegetableForm
    * Plugin/
      * Block/
        * VegetableBlock.php → class Drupal\vegetable\Plugin\Block\VegetableBlock
    * Entity/
      * Tomato.php → class Drupal\vegetable\Entity\Tomato
      * Cucumber.php → class Drupal\vegetable\Entity\Cucumber
    * Tests/
      * TomatoTest.php → class Drupal\vegetable\Tests\TomatoTest
      * CucumberTest.php → class Drupal\vegetable\Tests\CucumberTest
      * VegetableManagerTest.php → class Drupal\vegetable\Tests\VegetableManagerTest
    * fixtures/
      * weather-data.json
  * templates/
  * tests/
    * src/
      * Functional/
      * Kernel/
      * Unit/
        * TomatoTest.php → class Drupal\Tests\vegetable\Unit\TomatoTest
      * Traits/
        * VegetableTestTrait.php → trait Drupal\Tests\vegetable\Traits\VegetableTestTrait
  * vegetable.info.yml
  * vegetable.routing.yml
  * vegetable.module

Explanation:

1. Each module has a namespace that corresponds to its module name.


      Here: `Drupal\vegetable\`

2. The module's namespace is mapped to the `./src/` folder in the module directory.


      Here: `Drupal\vegetable\` → `modules/vegetable/src/`

3. Anything after the module namespace directly maps to the directory and file structure in the `./src/` folder.


      Here: `Drupal\vegetable\Entity\Tomato` → `modules/vegetable/src/Entity/Tomato.php`

The identical logic applies to PHPUnit tests contained in `./tests/src/`.

The `modules/vegetable/src/Tests` folder contains the [SimpleTest](https:/www.drupal.org/docs/7/testing/simpletest-testing-overview-drupal-7) test code and the `modules/vegetable/tests` folder contains the [PHPUnit](https:/www.drupal.org/docs/8/phpunit) test code. SimpleTest is deprecated in Drupal 8 but is still supported and PHPUnit is the recommended testing framework

## Namespace resolution

The namespace of all Drupal core components, as well as contributed modules, begins with `Drupal\`

The first parts of a namespaced class name indicate the base namespace that maps to a registered base directory, in which PHP files will be looked up:

|     | Base namespace | Base directory | Contains |
| --- | --- | --- | --- |
| Drupal core | `Drupal\Component\` | `core/lib/Drupal/Component/` | Components that are reusable outside of Drupal. |
|     | `Drupal\Core\` | `core/lib/Drupal/Core/` | Components that are specific to Drupal. |
|     | `Drupal\Tests\` | `core/tests/Drupal/Tests/` | PHPUnit tests of core components. |
| Modules | `Drupal\$modulename\` | `modules/$modulename/src/` | Main integration files. |
|     | `Drupal\$modulename\Tests\` | `modules/$modulename/src/Tests/` | Simpletest tests of the module. |
|     | `Drupal\Tests\$modulename\` | `modules/$modulename/tests/src/` | PHPUnit tests of the module. |

For modules, `$modulename` is the unique machine name of the module, which consists of lowercase characters and underscores.

The remaining part of a namespaced class name indicates the relative path within the base directory: each PHP namespace separator (`\`) is replaced with a directory separator (`/`) and the `.php` extension is appended:

| Base namespace | Relative class name | →   | Base directory | Relative file path |
| --- | --- | --- | --- | --- |
| Drupal\Component\ | Diff\Engine\DiffEngine | →   | core/lib/Drupal/Component/ | Diff/Engine/DiffEngine.php |
| Drupal\node\ | Entity\Node | →   | core/modules/node/src/ | Entity/Node.php |
| Drupal\Tests\views_ui\ | Form\Ajax\RearrangeFilterTest | →   | core/modules/views_ui/tests/src/ | Form/Ajax/RearrangeFilterTest.php |
| Drupal\devel\ | Plugin\Block\DevelSwitchUser | →   | modules/contrib/devel/src/ | Plugin/Block/DevelSwitchUser.php |

Each PHP class, interface, or trait lives in a separate PHP file.

For example, the class `Drupal\Component\Diff\Engine\DiffEngine` is defined in `core/lib/Drupal/Component/Diff/Engine/DiffEngine.php`.
