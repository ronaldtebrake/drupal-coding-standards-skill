# Avoid "SELECT * FROM ..."

Using `SELECT * FROM {node}` queries in versions prior to Drupal 7 can introduce a potential security issue by causing Drupal's Node Access system to be bypassed. In such cases, private content may be shown to unprivileged users. Therefore, queries that generate lists of nodes should avoid SELECT * syntax in all cases. Instead use `SELECT nid, ...`.

It is recommended to avoid SELECT * queries in general. They are less self-documenting than explicitly listing the fields to be retrieved and also very slightly slower. Generally, SELECT * should be used in only two cases:

1. The fields in the table being selected from are dynamic and not known definitively at development time. (This is extremely rare and generally bad practice anyway.)
2. The list of fields to select is prohibitively long.

* [development] Is "SELECT * FROM ..." ok?](http://lists.drupal.org/pipermail/development/2009-February/thread.html#31953) : discussion on the development list.
* [SELECT * IS EVIL](//www.parseerror.com/sql/select*isevil.html): one developer's perspective.
