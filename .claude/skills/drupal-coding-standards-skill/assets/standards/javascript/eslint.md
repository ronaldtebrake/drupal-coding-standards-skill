As of Drupal 8, we use [ESLint](http://eslint.org/) to make sure our JavaScript code is consistent and free from syntax error and leaking variables and that it can be properly minified.

> ESLint is a tool to detect errors and potential problems in JavaScript code.

ESLint is a Node.js module and is integrated into a number of IDEs. (See [installation and usage](http://eslint.org/docs/user-guide/getting-started#installation-and-usage) instructions.) One of the fastest ways to install all necessary ESLint dependencies is using [eslint-config-drupal](https://www.npmjs.com/package/eslint-config-drupal).

The following set of configuration options has been agreed upon. (See [ESLint change notice](https://www.drupal.org/node/2274223).) Configuration is improved when possible, always use the latest stable ESLint version.

These configuration files ship with Drupal and can be used from there.

*   [.eslintrc.json](https://git.drupalcode.org/project/drupal/-/blob/11.x/.eslintrc.json)
*   [.eslintignore](https://git.drupalcode.org/project/drupal/-/blob/11.x/.eslintignore)

In a Drupal folder these configuration files will be automatically detected and used by ESLint when it is invoked from within the code base.

## Contrib modules

Contrib modules should also conform to ESLint requirements.

In special cases, some rules need to be changed for specific modules (if they use a third party library and they need to use a new global variable), in that case the module can create it's own `.eslintrc.json` turning some rules on or off and adding global variables they require. When ESLint runs, all the configuration files present in the directory tree are merged together: see [Configuration Cascading and Hierarchy](https://eslint.org/docs/latest/use/configure/configuration-files#cascading-and-hierarchy) for more details. For example, the Google Analytics module uses third-party code that defines the global ”ga”, which can be allowed by adding the following code to `.eslintrc.json` in the module directory:

```php
{
  "extends": [
    "drupal"
  ],
  "globals": {
    "ga": true
  }
}
```

## Checking custom JavaScript with ESLint

ESLint can be installed in order to check that custom JavaScript follows standards. Here is how:

```php
# Before running these commands, install node.js, npm, and npx
npm install eslint eslint-config-airbnb eslint-plugin-yml --save-dev
npm i eslint-config-drupal
npx eslint modules/custom/
npx eslint themes/custom/
```
