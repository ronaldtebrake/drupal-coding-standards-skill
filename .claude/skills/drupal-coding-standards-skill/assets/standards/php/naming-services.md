## Manipulating the Request object:

Elements added to the attributes of the Request object by any Drupal module or service should have a "_" prepended unless they come from the path.

Example:

```php
\Drupal::request()->attributes->set('_context_value', $myValue);
```

Only values that come from the path will have the "_" omitted, for example, the path pattern /node/{node}.

Drupal core and Symfony typically add some prefixed attributes that should not be overwritten by a contributed module. These include:

```php
_system_path
_title
_account
```

(Note that account is being removed in [#2073531: Use current user service instead of _account, remove _account from the request object](https://www.drupal.org/project/drupal/issues/2073531 "Status: Closed (fixed)").)

and from Symfony\Cmf\Component\Routing\RouteObjectInterface:

```php
_route
_route_object
_controller
_content
```

See the [the original change notice](https://www.drupal.org/node/2083979).
