Use [eslint](http://eslint.org/docs/user-guide/command-line-interface) for Drupal JS coding standards. See the [eslint settings information](https://www.drupal.org/node/1955232). Drupal uses 'eslint-config-airbnb' as ESLint shareable config. Therefore it's reasonable to use ['Airbnb JavaScript Style Guide'](https://github.com/airbnb/javascript/) as Drupal JS coding standard.

## Indenting

* All code MUST indent using two (2) space characters,
* All code MUST NOT indent using tab characters,
* All code MUST NOT end with trailing whitespace.

## Semicolons

JavaScript allows optional "semi-colon insertion". Drupal standards do not.

* All statements (except `for, function, if, switch, try, while`) MUST be followed by a semi-colon (`;`),
* Return values MUST start on the same line as the `return` keyword.

**EXCEPTIONS:**

* Anonymous functions assigned to a variable MUST be followed by a semi-colon.

   ```php
    Drupal.behaviors.tableSelect = function (context) {
      // Statements...
    };
    ```

* do/while control structures MUST be followed by a semi-colon

    ```php
    do {
      // Statements...
    } while (condition);
    ```

## File-closure

All JavaScript code MUST be declared inside a closure wrapping the whole file.

```php
/**
 * @file
 */
(() => {

  // All the JavaScript for this file.

})();
```

## CamelCasing

For variables that are not [constants](#constants) or [constructors](#constructors), multi-word variables and functions SHOULD be lowerCamelCased.

The first letter of each variable or function SHOULD be lowercase, and the first letter of subsequent words SHOULD be capitalized. There SHOULD NOT be underscores between the words.

In case a variable contains a jQuery object, the variable MUST start with a dollar sign (`$`):

```php
let $form = $('#search-block-form');
let $inputs = $form.find('input');
```

## Variables and Arrays

Due to enabled strict mode, an undeclared variable will halt the script, and on old browsers such variables are implicitly exported into global scope.

All variables MUST be declared with let or const before they are used and SHOULD be declared only once. All variables SHOULD be declared at the beginning of a function.

Each variable assignment SHOULD be declared on a separate line - including variables that are only declared but do not get a value assigned.

```php
let anArray = [];
let eventCallback = function () {};
let curTableDragSetting;
let curTableDragIndex;
```

### Global Variables

Drupal JavaScript MUST NOT define global variables.

### Constants

Pre-defined constants SHOULD be all-uppercase and words separated by underscores: `UPPER_UNDERSCORED`.

Variables added via PHP SHOULD be [lowerCamelCased](#camelcasing), so that they are consistent with other JavaScript variables:

```php
$element['#attached']['js'][] = array(
  'data' => array('myModule' => array('basePath' => base_path())),
  'type' => 'setting',
);
```

This variable would then be referenced:

```php
Drupal.settings.myModule.basePath;
```

### Arrays

Arrays SHOULD be formatted with one space separating each element and the assignment operator, if applicable:

```php
let someArray = ['hello', 'world'];
```

If the line is longer than 80 characters, each element SHOULD be broken into its own line, and indented one level.

Use trailing comma after last element in multi line arrays. Trailing commas simplify adding and removing items to objects and arrays, since only the lines you are modifying must be touched. Also this leads to cleaner git diffs, when an item is added or removed from an object or array.

```php
// bad
let fruits = [
  'apples',
  'banana',
  'pineapple'
];

// good
let fruits = [
  'apples',
  'banana',
  'pineapple',
];
```

### Typeof

In type comparisons, the value tested MUST NOT be wrapped in parenthesis.

```php
if (typeof myVariable === 'string') {
  // ...
}
```

## Functions

### Function and method names

Function names SHOULD begin with the name of the module or theme declaring the function, so as to avoid name collisions.

```php
Drupal.behaviors.tableDrag = function (context) {
  Object.keys(Drupal.settings.tableDrag).forEach(function (base) {
    $('#' + base).once('tabledrag', addBehavior);
  });
};
```

### Function Declarations

* The `function` keyword MUST be followed by one space.
* Named functions MUST NOT have a space between the function name and the following left parenthesis.
* Optional arguments (using default values) SHOULD be defined at the end of the function signature.
* Every function SHOULD attempt to return a meaningful value.

```php
Drupal.behaviors.tableDrag = function (context) {
  // ...
  this.clickCallback = function (e) {
    return false;
  };
  // ...
};

function funStuff(field, settings) {
  const settings = settings || Drupal.settings;
  alert("This JS file does fun message popups.");
  return field;
}

// Anonymous:
() => {}

// Named:
function closeDialog() {}
```

Note: The above examples code are lacking JSDoc and comments, only for clarity.

### Function Calls

Functions SHOULD be called with no spaces between the function name, the opening parenthesis, and the first parameter.

There SHOULD be one space between commas and each parameter, and there SHOULD NOT be a space between the last parameter, the closing parenthesis, and the semicolon.

```php
let foobar = foo(bar, baz, quux);
```

There SHOULD be one space on either side of an equals sign used to assign the return value of a function to a variable.

## Constructors

Constructors are functions that are designed to be used with the `new` prefix. The `new` prefix creates a new object based on the function's prototype, and binds that object to the function's implied this parameter. JavaScript doesn't issue compile-time warning or run-time warnings if a required `new` is omitted. If you do not use the `new` prefix, no new object will be made and operations will be bound to the global object instead.

Constructor functions MUST be given names with an initial uppercase character.

A function with an initial uppercase name MUST NOT be called without a `new` operator.

```php
function CollapsibleDetails(node) {}

let collapsibleDetail = new CollapsibleDetails(element);
```

## Comments

Inline documentation for source files MUST follow the [JavaScript API documentation and comment standards](documentation.md) (based on JSDoc).

Non-JSDoc comments are strongly RECOMMENDED.

A general rule of thumb is that if you look at a section of code and think "Wow, I don't want to try and describe that", you SHOULD comment it before you forget how it works. Comments MAY be removed by JS compression utilities later, so they don't negatively impact the file download size.

Non-JSDoc comments SHOULD use capitalized sentences with punctuation. Comments SHOULD be on a separate line, immediately before the code line or block they reference.

```php
// Unselect all other checkboxes.
doSomething();
```

If each line of a list needs a separate comment, comments MAY be placed on the same line and MAY be formatted to a uniform indent for readability.

C style comments (`/* */`) and standard C++ comments (`//`) are both allowed.

## String Concatenation

Expressions SHOULD be separated with one space before and after the `+` operator to improve readability.

```php
let string = 'Foo' + bar;
string = bar + 'foo';
string = bar() + 'foo';
string = 'foo' + 'bar';
```

The concatenating assignment operator (`+=`) SHOULD be separated with one space on each side as with the assignment operator:

```php
let string += 'Foo';
string += bar;
string += baz();
```

## Control Structures

Control statements MUST have one space between the control keyword and opening parenthesis, to distinguish them from function calls.

Control structures MUST always use curly braces, even in situations where they are optional. Having them increases readability and decreases the likelihood of logic errors being introduced when new lines are added.

These include `if, for, while, switch`.

Example `if` statement (the most complicated one):

```php
if (condition1 || condition2) {
  action1();
}
else if (condition3 && condition4) {
  action2();
}
else {
  defaultAction();
}
```

### switch

For `switch` statements:

```php
switch (condition) {
  case 1:
    action1();
    break;

  case 2:
    action2();
    break;

  default:
    defaultAction();
}
```

### try

For `try/catch` statements:

```php
try {
  // Statements...
}
catch (error) {
  // Error handling...
}
finally {
  // Statements...
}
```

### for in

The body of every `for in` statement MUST be wrapped in an `if` statement that performs the filtering. It MAY select for a particular type or range of values, or it can exclude functions, or it can exclude properties from the prototype.

```php
for (let variable in object) {
  if (filter) {
    // Statements...
  }
}
```

You MUST use the `hasOwnProperty` method to distinguish the true members of the object, which SHOULD be placed inside the loop, not on the same line:

```php
for (let variable in object) {
  if (object.hasOwnProperty(variable)) {
    // Statements...
  }
}
```

## Operators

### Comparisons

The `==` and `!=` operators do type coercion before comparing. This can lead to unexpected errors.

Strict equality MUST be used in comparisons (`===` or `!==`).

### Comma Operator

You SHOULD NOT use the comma operator, with the exception of the control part in `for` statements.

The comma operator causes the expressions on either side of it to be executed in left-to-right order, and returns the value of the expression on the right.

```php
let x = (y = 3, z = 9);
```

This sets `x` to `9`. This can be confusing for users not familiar with the syntax and makes the code more difficult to read and understand.
