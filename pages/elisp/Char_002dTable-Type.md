

#### 2.4.10 Char-Table Type

A *char-table* is a one-dimensional array of elements of any type, indexed by character codes. Char-tables have certain extra features to make them more useful for many jobs that involve assigning information to character codes—for example, a char-table can have a parent to inherit from, a default value, and a small number of extra slots to use for special purposes. A char-table can also specify a single value for a whole character set.

The printed representation of a char-table is like a vector except that there is an extra ‘`#^`’ at the beginning.[1](#FOOT1)

See [Char-Tables](Char_002dTables.html), for special functions to operate on char-tables. Uses of char-tables include:

Case tables (see [Case Tables](Case-Tables.html)).

Character category tables (see [Categories](Categories.html)).

Display tables (see [Display Tables](Display-Tables.html)).

Syntax tables (see [Syntax Tables](Syntax-Tables.html)).

***

#### Footnotes

##### [(1)](#DOCF1)

You may also encounter ‘`#^^`’, used for sub-char-tables.
