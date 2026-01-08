# Spelling

Drupal Core uses US English spelling for all source code, including comments and names.

Core code is automatically checked for spelling by [CSpell](https://cspell.org/).

## Spelling of CSpell

For in line documentation settings use all lower case for cspell. For example, `// cspell:ignore.`

## Ignore word(s) in a file

Use the `cspell:ignore` directive. The words are listed alphabetically with a single space between the words. Use multiple lines to meet the number of characters per line limit.

```php
<?php

// cspell:ignore first-word second-word
```

## Disable CSpell by line

if it's not in English or a nonsense string (for example, a random string in a test) disable CSpell for that line. Note that `cspell` is lower case.

```php
<?php

// cspell:disable-next-line
$token = 'PxOHfS_QL-T01NjBgu7Z7I04tIwMp6La5vM-mVxezbU';
use Drupal\foo\Bar;
```

## Disable CSpell for multiple lines

For multiple lines, use disable before the lines and enable after. Note that `cspell` is lower case.

```php
<?php

// cspell:disable
$lorem1 = 'Lorem ipsum dolor sit amet in libero.';
$lorem2 = 'Ut fermentum est vitae metus orci.';
// cspell:enable
```

See the [cspell development tool page](https://www.drupal.org/node/3352552) for more details.
