# CSScomb settings for Drupal (CSS formatting and sort tool)

[CSScomb](https://github.com/csscomb/csscomb.js "Make your code beautiful") formats and sorts CSS properties in **css, scss, sass or less** files. An explanation of the options can be found at [https://github.com/csscomb/csscomb.js/blob/master/doc/options.md](https://github.com/csscomb/csscomb.js/blob/master/doc/options.md).

CSScomb is available as:

*   a [command-line tool](https://github.com/csscomb/csscomb.js/blob/master/doc/usage-cli.md " command line usage")
*   a [plugin](https://github.com/csscomb/sublime-csscomb "csscomb/sublime-csscomb | GitHub") for [Sublime Text](http://www.sublimetext.com/3 " The text editor you'll fall in love with")
*   a [plugin](https://atom.io/packages/csscomb "Atom Editor Plugin for CSScomb | atom.io") for [Atom](https://atom.io/ " A hackable text editor for the 21st Century by GitHub")
*   a [plugin](https://github.com/csscomb/grunt-csscomb "csscomb/grunt-csscomb | GitHub") for [Grunt](http://gruntjs.com/ " The JavaScript Task Runner") (post-processing)
*   a [plugin](https://www.npmjs.com/package/gulp-csscomb "gulp-csscomb | npm packages") for [Gulp](http://gulpjs.com/ "gulp.js - the streaming build system") (post-processing)
*   an [external tool](https://github.com/csscomb/jetbrains-csscomb "csscomb/jetbrains-csscomb | GitHub") for [PhpStorm](https://www.jetbrains.com/phpstorm/ "The Most Intelligent PHP IDE | JetBrains PhpStorm")
*   a [plugin](https://github.com/csscomb/vim-csscomb "csscomb/vim-csscomb | GitHub") for [Vim](https://www.drupal.org/node/1389006 "Vimrc - Vim Plugin for Drupal | Drupal.org").

The used values in the snippet below are in accordance with the recommendations in the [CSS formatting guidelines](format.md). The sort order of CSS properties is based on the [Yandex configuration file](https://github.com/csscomb/csscomb.js/blob/master/config/yandex.json "csscomb.js/yandex.json | GitHub") and improved for SASS/LESS by [putting variables first](https://github.com/csscomb/csscomb.js/blob/master/doc/options.md#sort-order-vs-preprocessors "csscomb.js/options.md at master · csscomb/csscomb.js · GitHub") (they have to be declared before being used).

Copy/paste the configuration below into the CSScomb package settings file under _Preferences_ in Sublime Text (after installation of the plugin). For most other use cases, put it in a file named `.csscomb.json` [in your local home folder](https://github.com/csscomb/csscomb.js/blob/master/doc/configuration.md#where-to-put-config "Where to put the config file?") (usually named _www_) to make it applicable to all your projects.

```yaml
{
  "config": {
    "exclude": [
      "core/**",
      "modules/**",
      "profiles/**",
      "sites/**",
      "themes/**",
      "tests/**",
      "config/**",
      "includes/**",
      "tmp/**",
      "vendor/**",
      "node_modules/**",
      "bower_components/**",
      "lib/**",
      "src/**",
      "img/**",
      "images/**",
      "icons/**",
      "js/**",
      "javascript/**",
      "scripts/**",
      "jquery/**",
      ".git/**"
    ],
    "always-semicolon": true,
    "remove-empty-rulesets": true,
    "color-case": "lower",
    "color-shorthand": true,
    "element-case": "lower",
    "eof-newline": true,
    "leading-zero": true,
    "quotes": "double",
    "sort-order-fallback": "abc",
    "space-after-colon": " ",
    "space-after-combinator": " ",
    "space-after-opening-brace": "\n",
    "space-after-selector-delimiter": "\n",
    "space-before-colon": "",
    "space-before-combinator": " ",
    "space-before-opening-brace": " ",
    "space-before-selector-delimiter": "",
    "space-between-declarations": "\n",
    "block-indent": 2,
    "strip-spaces": true,
    "space-before-closing-brace": "\n",
    "unitless-zero": true,
    "tab-size": 2,
    "lines-between-rulesets": false,
    "verbose": true,
    "sort-order": [
      [
        "$charset",
        "$import",
        "$namespace",
        "$extend",
        "$variable",
        "$include",

        "position",
        "z-index",
        "top",
        "right",
        "bottom",
        "left",

        "display",
        "visibility",
        "float",
        "clear",
        "overflow",
        "overflow-x",
        "overflow-y",
        "-ms-overflow-x",
        "-ms-overflow-y",
        "-webkit-overflow-scrolling",
        "clip",
        "zoom",
        "flex",
        "flex-direction",
        "flex-basis",
        "flex-flow",
        "flex-grow",
        "flex-shrink",
        "flex-wrap",
        "order",
        "flex-order",
        "flex-pack",
        "align",
        "align-self",
        "justify-content",
        "align-items",
        "align-content",
        "flex-align",

        "-webkit-box-sizing",
        "-moz-box-sizing",
        "box-sizing",
        "width",
        "min-width",
        "max-width",
        "height",
        "min-height",
        "max-height",
        "margin",
        "margin-top",
        "margin-right",
        "margin-bottom",
        "margin-left",
        "padding",
        "padding-top",
        "padding-right",
        "padding-bottom",
        "padding-left",

        "table-layout",
        "-webkit-columns",
        "-moz-columns",
        "columns",
        "-webkit-column-span",
        "-moz-column-span",
        "column-span",
        "-webkit-column-width",
        "-moz-column-width",
        "column-width",
        "-webkit-column-count",
        "-moz-column-count",
        "column-count",
        "-webkit-column-fill",
        "-moz-column-fill",
        "column-fill",
        "-webkit-column-gap",
        "-moz-column-gap",
        "column-gap",
        "-webkit-column-rule",
        "-moz-column-rule",
        "column-rule",
        "-webkit-column-rule-width",
        "-moz-column-rule-width",
        "column-rule-width",
        "-webkit-column-rule-style",
        "-moz-column-rule-style",
        "column-rule-style",
        "-webkit-column-rule-color",
        "-moz-column-rule-color",
        "column-rule-color",
        "empty-cells",
        "caption-side",
        "border-spacing",
        "border-collapse",
        "$counter-style",
        "list-style",
        "list-style-position",
        "list-style-type",
        "list-style-image",

        "content",
        "quotes",
        "counter-reset",
        "counter-increment",
        "resize",
        "cursor",
        "-webkit-user-select",
        "-moz-user-select",
        "-ms-user-select",
        "user-select",
        "nav-index",
        "nav-up",
        "nav-right",
        "nav-down",
        "nav-left",
        "-webkit-transition",
        "-moz-transition",
        "-ms-transition",
        "-o-transition",
        "transition",
        "-webkit-transition-delay",
        "-moz-transition-delay",
        "-ms-transition-delay",
        "-o-transition-delay",
        "transition-delay",
        "-webkit-transition-timing-function",
        "-moz-transition-timing-function",
        "-ms-transition-timing-function",
        "-o-transition-timing-function",
        "transition-timing-function",
        "-webkit-transition-duration",
        "-moz-transition-duration",
        "-ms-transition-duration",
        "-o-transition-duration",
        "transition-duration",
        "-webkit-transition-property",
        "-moz-transition-property",
        "-ms-transition-property",
        "-o-transition-property",
        "transition-property",
        "-webkit-transform",
        "-moz-transform",
        "-ms-transform",
        "-o-transform",
        "transform",
        "-webkit-transform-origin",
        "-moz-transform-origin",
        "-ms-transform-origin",
        "-o-transform-origin",
        "transform-origin",
        "$keyframes",
        "-webkit-animation",
        "-moz-animation",
        "-ms-animation",
        "-o-animation",
        "animation",
        "-webkit-animation-name",
        "-moz-animation-name",
        "-ms-animation-name",
        "-o-animation-name",
        "animation-name",
        "-webkit-animation-duration",
        "-moz-animation-duration",
        "-ms-animation-duration",
        "-o-animation-duration",
        "animation-duration",
        "-webkit-animation-play-state",
        "-moz-animation-play-state",
        "-ms-animation-play-state",
        "-o-animation-play-state",
        "animation-play-state",
        "-webkit-animation-timing-function",
        "-moz-animation-timing-function",
        "-ms-animation-timing-function",
        "-o-animation-timing-function",
        "animation-timing-function",
        "-webkit-animation-delay",
        "-moz-animation-delay",
        "-ms-animation-delay",
        "-o-animation-delay",
        "animation-delay",
        "-webkit-animation-iteration-count",
        "-moz-animation-iteration-count",
        "-ms-animation-iteration-count",
        "-o-animation-iteration-count",
        "animation-iteration-count",
        "-webkit-animation-direction",
        "-moz-animation-direction",
        "-ms-animation-direction",
        "-o-animation-direction",
        "animation-direction",
        "text-align",
        "-webkit-text-align-last",
        "-moz-text-align-last",
        "-ms-text-align-last",
        "text-align-last",
        "vertical-align",
        "white-space",
        "text-decoration",
        "text-emphasis",
        "text-emphasis-color",
        "text-emphasis-style",
        "text-emphasis-position",
        "text-indent",
        "-ms-text-justify",
        "text-justify",
        "text-transform",
        "letter-spacing",
        "word-spacing",
        "-ms-writing-mode",
        "text-outline",
        "text-transform",
        "text-wrap",
        "text-overflow",
        "-ms-text-overflow",
        "text-overflow-ellipsis",
        "text-overflow-mode",
        "-ms-word-wrap",
        "word-wrap",
        "word-break",
        "-ms-word-break",
        "-moz-tab-size",
        "-o-tab-size",
        "tab-size",
        "-webkit-hyphens",
        "-moz-hyphens",
        "hyphens",
        "pointer-events",
        "direction",
        "unicode-bidi",
        "orphans",
        "widows",

        "opacity",
        "filter:progid:DXImageTransform.Microsoft.Alpha(Opacity",
        "-ms-filter:\\'progid:DXImageTransform.Microsoft.Alpha",
        "-webkit-filter",
        "-ms-filter",
        "filter",
        "-ms-interpolation-mode",
        "color",
        "border",
        "border-collapse",
        "border-width",
        "border-style",
        "border-color",
        "border-top",
        "border-top-width",
        "border-top-style",
        "border-top-color",
        "border-right",
        "border-right-width",
        "border-right-style",
        "border-right-color",
        "border-bottom",
        "border-bottom-width",
        "border-bottom-style",
        "border-bottom-color",
        "border-left",
        "border-left-width",
        "border-left-style",
        "border-left-color",
        "-webkit-border-radius",
        "-moz-border-radius",
        "border-radius",
        "-webkit-border-top-left-radius",
        "-moz-border-radius-topleft",
        "border-top-left-radius",
        "-webkit-border-top-right-radius",
        "-moz-border-radius-topright",
        "border-top-right-radius",
        "-webkit-border-bottom-right-radius",
        "-moz-border-radius-bottomright",
        "border-bottom-right-radius",
        "-webkit-border-bottom-left-radius",
        "-moz-border-radius-bottomleft",
        "border-bottom-left-radius",
        "-webkit-border-image",
        "-moz-border-image",
        "-o-border-image",
        "border-image",
        "-webkit-border-image-source",
        "-moz-border-image-source",
        "-o-border-image-source",
        "border-image-source",
        "-webkit-border-image-slice",
        "-moz-border-image-slice",
        "-o-border-image-slice",
        "border-image-slice",
        "-webkit-border-image-width",
        "-moz-border-image-width",
        "-o-border-image-width",
        "border-image-width",
        "-webkit-border-image-outset",
        "-moz-border-image-outset",
        "-o-border-image-outset",
        "border-image-outset",
        "-webkit-border-image-repeat",
        "-moz-border-image-repeat",
        "-o-border-image-repeat",
        "border-image-repeat",
        "outline",
        "outline-width",
        "outline-style",
        "outline-color",
        "outline-offset",
        "background",
        "filter:progid:DXImageTransform.Microsoft.AlphaImageLoader",
        "background-color",
        "background-image",
        "background-repeat",
        "background-attachment",
        "background-position",
        "background-position-x",
        "-ms-background-position-x",
        "background-position-y",
        "-ms-background-position-y",
        "-webkit-background-clip",
        "-moz-background-clip",
        "background-clip",
        "background-origin",
        "-webkit-background-size",
        "-moz-background-size",
        "-o-background-size",
        "background-size",
        "box-decoration-break",
        "-webkit-box-shadow",
        "-moz-box-shadow",
        "box-shadow",
        "filter:progid:DXImageTransform.Microsoft.gradient",
        "-ms-filter:\\'progid:DXImageTransform.Microsoft.gradient",
        "text-shadow",

        "$font-face",
        "font",
        "font-family",
        "src",
        "$font-feature-values",
        "$swash",
        "$annotation",
        "$ornaments",
        "$stylistic",
        "$styleset",
        "$character-variant",
        "font-variant-alternates",
        "font-size",
        "font-weight",
        "font-style",
        "font-variant",
        "font-size-adjust",
        "font-stretch",
        "font-effect",
        "font-emphasize",
        "font-emphasize-position",
        "font-emphasize-style",
        "font-smooth",
        "line-height",

        "$media",
        "$supports",
        "$document",
        "$page",
        "$viewport"
      ]
    ]
  }
}
```

### Note

To have ruleset groups divided by an empty line, replace the empty lines above with:

```php
    ],
    [
```

### Alternatives

There is [csscombx](https://www.npmjs.com/package/csscombx), which is a fork of [Csscomb](https://github.com/csscomb/csscomb.js) tool specifically intended for Drupal. It has a default configuration file with exactly the same content as the example above, so you don't need to make any additional set up.

How to use.

First, you need to install [nodejs](https://nodejs.org/en/download/) on your system. It has the **npm** - a package manager for many popular javascript projects.

Then, install [csscombx](https://www.npmjs.com/package/csscombx) globally:

```php
npm install csscombx --global
```

Depending on where you've installed **npm** the command above may require **sudo** prepended to it.

After installing **cd** into your theme's root folder (or any folder containing style files to be formatted) and make a so-called dry run without changing anything:

```php
csscombx -vl ./
```

The **-v** option on the command stands for verbose and **-l** for lint. It scans the current directory recursively for all **css**, **scss**, **sass** and **less** files and reports their status relative to the settings in the [drupal.json](https://github.com/drugan/csscombx/blob/master/config/drupal.json) configuration file of the tool. If you want to change default settings just copy the content of the file and paste into **.csscombx.json** file in the root of your theme, a particular style folder, webroot folder or any other folder where you require your customized formatting settings to be applied. Note that any of a project's child folders may have its own **.csscombx.json** file with different settings.

When you are ready for actual changes, run this command passing desirable folder and/or particular file paths as arguments:

```php
csscombx -v ./my-styles public/styles.css
```

Sometimes you may want to automate the formatting task above and run it whenever the style file(s) are saved after being edited. For that case, you can use the [gulp-csscombx](https://www.npmjs.com/package/gulp-csscombx) plugin.

How to use.

Install [gulp-cli](https://www.npmjs.com/package/gulp-cli) globally:

```php
npm rm --global gulp && npm install --global gulp-cli
```

Create **package.json** file in the root of your theme, module or any other project and put this into it:

```yaml
{
  "name": "whatever",
  "version": "1.0.0",
  "dependencies": {
    "gulp": "^3.9.1",
    "gulp-csscombx": "^4.2.0"
  }
}
```

Then run this command:

```php
npm install
```

If you already have a **package.json** file then just run this command without touching the file:

```php
npm install gulp --save && npm install gulp-csscombx --save
```

Then create a **gulpfile.js** file in the root of your theme, module or any other project and put this into it:

```php
'use strict';

var gulp = require('gulp');
var csscombx = require('gulp-csscombx');

// @see https://github.com/gulpjs/gulp/blob/master/docs/API.md#gulpsrcglobs-options
var mySrcStyles = ['./**.scss', './my-src/**.less', '!./client/bad.css', './gallery/*.sass'];

gulp.task('style', function() {
  return gulp.src(mySrcStyles)
    // Process mySrcStyles according Drupal coding standards.
    .pipe(csscombx())
    // Save the tree of processed folders/files in the ./my-dest-styles folder
    // or mySrcStyles if you want formatted files saved in the same place.
    .pipe(gulp.dest('./my-dest-styles'));
});

gulp.task('watch-my-styles', function() {
  // Run the 'style' task whenever mySrcStyles are edited and saved.
  var watcher = gulp.watch(mySrcStyles, ['style']);

  // Emit a change event notification on the console if you need one.
  watcher.on('change', function(event) {
    console.log('File ' + event.path + ' was ' + event.type + ', running tasks...');
  });
});
```

Adjust **mySrcStyles** variable's value for the content you want to be formatted and run this command:

```php
gulp watch-my-styles
```

You'll see that execution of the command is halted and waits while any of the **mySrcStyles** will be changed. Do it and look in the console again. It should report that some files are automatically formatted. When you are done with developing your styles just go to the console and press **Ctrl+C** keyboard shortcut to terminate the gulp watcher's task.

If you think that some of the [drupal.json](https://github.com/drugan/csscombx/blob/master/config/drupal.json) default configuration file's settings do not comply with Drupal CSS coding standards then please do the following:

Create a [new issue](https://www.drupal.org/node/add/project-issue/drupal) for the **CSS** component of the Drupal core.

Post a [comment on this page](https://www.drupal.org/node/2399303/discuss) to notify all the participants involved.

[Create the issue](https://github.com/drugan/csscombx/issues/new) or just make a pull request on the [csscombx](https://github.com/drugan/csscombx).

Also, if you have some difficulties with the **gulp-csscombx** plugin then post the issue on [its GitHub repository](https://github.com/drugan/gulp-csscombx/issues).
