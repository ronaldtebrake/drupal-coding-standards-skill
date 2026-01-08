## Basic Exception Naming Conventions

1. As Exceptions are classes, they should follow all [coding standards for object-oriented code](index.md) like any other class.
2. All Exceptions must end with the suffix "Exception".
3. All Exceptions should include an appropriate message and should not be translated. Only messages from the install and update system are currently translated as they are user facing.
4. The Exception's message should include a hint to the values that caused the exception.
   1. Formatting messages should be done by concatenating strings or using `sprintf()`.
   2. Values should be surrounded by single quotes.
   3. **DO NOT** translate the message.
   4. **DO NOT** use `SafeMarkup::format()`.
5. Exception classes should be named for the subsystem to which they relate, and the type of error. That is, `[Subsystem][ErrorType]Exception`.

## Exception Subclasses

The use of subclassed Exceptions is preferred over reusing a single generic Exception class with different error messages as different classes may then be caught separately.

### Example:

```php
class WidgetNotFoundException extends Exception {}

function use_widget($widget_name) {
  $widget = find_widget($widget_name);

  if (!$widget) {
    throw new WidgetNotFoundException("Widget '$widget_name' not found.");
  }
}
```

See [the Drupal\Core\Entity\Exception namespace](https://api.drupal.org/api/drupal/namespace/Drupal!Core!Entity!Exception/8.2.x) for real-life examples.

## Try-catch blocks

Try-catch blocks should follow a similar line-breaking pattern to if-else statements, with each catch statement beginning a new line.

Example:

```php
try {
  $widget = 'thingies';
  $result = use_widget($widget);

  // Continue processing the $result.
  // If an exception is thrown by use_widget(), this code never gets called.
}
catch (WidgetNotFoundException $e) {
  // Error handling specific to the absence of a widget.
}
catch (Exception $e) {
  // Generic exception handling if something else gets thrown.
  \Drupal::logger('widget')->error($e->getMessage());
}
```

### Inheritance

PHP requires that all exceptions inherit off of the Exception class, either directly or indirectly.

When creating a new exception class, it should be named according to the subsystem they relate to and the error message they involve. If a given subsystem includes multiple exceptions, they should all extend from a common base exception. That allows for multiple catch blocks as necessary.

```php
class FelineException extends Exception {}

class FelineHairBallException extends FelineException {}

class FelineKittenTooCuteException extends FelineException {}

try {
  $normal = new Kitten();
  $normal->playWith($string);
}
catch (FelineHairBallException $e) {
  // Do error handling here.
}
catch (FelineKittenTooCuteException $e) {
  // Do different error handling here.
}
catch (FelineException $e) {
  // Do generic error handling here.
}
// Optionally also catch Exception so that all exceptions stop here instead of propagating up.
```
