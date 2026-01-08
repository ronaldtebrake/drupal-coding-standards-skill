## Adjusting the error reporting level

Drupal 6.x releases ignore E_NOTICE, E_STRICT, and E_DEPRECATED notices for the benefit of production sites. To view all PHP errors on development or testing sites, you may change `includes/common.inc` from:

```php
if ($errno & (E_ALL ^ E_DEPRECATED ^ E_NOTICE)) {
```

to:

```php
if ($errno & (E_ALL | E_STRICT)) {
```

Drupal 7.x releases report any error levels which are part of E_ALL, and allow PHP to be configured to report additional error levels, such as E_STRICT. To view all PHP errors on development or testing sites, you may set

```php
php_value error_reporting -1
```

in the `.htaccess` file.

## Use of isset() or !empty()

If you want to test the value of a variable, array element or object property, you may need to use `if (isset($var))` or `if (!empty($var))` rather than `if ($var)` if there is a possibility that `$var` has not been defined.

The difference between [isset()](http://php.net/isset) and [!empty()](http://php.net/empty) is that unlike `!empty()`, `isset()` will return TRUE even if the variable is set to an empty string or zero. In order to decide which one to use, consider whether `''` or `0` are valid and expected values for your variable.

The following code may trigger an E_NOTICE error:

```php
function _form_builder($form, $parents = array(), $multiple = FALSE) {
  // (...)
  if ($form['#input']) {
    // some code (...)
  }
}
```

Here, the variable `$form` is passed on to the function. If `$form['#input']` evaluates as true, `some code` is executed. However, if `$form['#input']` does not exist, the function outputs the following error message: `notice: Undefined index: #input in includes/form.inc on line 194.`

Even though the array $form is already declared and passed to the function, each array index must be explicitly declared. The previous code should read:

```php
function _form_builder($form, $parents = array(), $multiple = FALSE) {
  // (...)
  if (!empty($form['#input'])) {
    // some code (...)
  }
}
```

**Beware!**

The function [isset()](http://php.net/isset) returns `TRUE` when the variable is set to `0`, but `FALSE` if the variable is set to `NULL`. In some cases, [is_null()](http://php.net/is_null) is a better choice, especially when testing the value of a variable returned by an SQL query.
