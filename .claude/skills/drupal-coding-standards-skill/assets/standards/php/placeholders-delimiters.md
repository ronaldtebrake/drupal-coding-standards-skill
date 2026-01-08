## Temporary place-holders and delimiters

When writing a content filter module, or any code that processes or modifies content, it is tempting to use an obscure character as a place-holder, especially if only your code will see it: But this cannot be guaranteed. Non-printing, invalid or undocumented characters might not be handled correctly in the unlikely event that they are seen by a browser or feed-reader. And the more unlikely they are to be seen – the less likely they are to be tested. This will mean that some code will be written to find and eradicate these insidious characters, possibly including the ones your code is using to do its work.

To avoid this happening, and extending the lifetime of your code, please use an appropriate alpha-numeric string – prefixed by the name of the module (as a name-space) and a hyphen `-` or underscore `_` – and surrounded by `[`…`]`.

If you need delimiting place-holders, the closing delimiter can incorporate a `/` after the initial `[` and may suffix the modulename.

## Finding your placeholders

A PCRE such as

*   `'@\[modulename-tag\](.+?)\[/modulename-tag\]@'`

or

*   `'@\[modulename-tag\](.+?)\[/tag-modulename\]@'` if you suffixed the modulename as mentioned above

can be used to match the string you have previously delimited.
