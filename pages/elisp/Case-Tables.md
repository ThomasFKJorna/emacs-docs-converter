

### 4.10 The Case Table

You can customize case conversion by installing a special *case table*. A case table specifies the mapping between upper case and lower case letters. It affects both the case conversion functions for Lisp objects (see the previous section) and those that apply to text in the buffer (see [Case Changes](Case-Changes.html)). Each buffer has a case table; there is also a standard case table which is used to initialize the case table of new buffers.

A case table is a char-table (see [Char-Tables](Char_002dTables.html)) whose subtype is `case-table`. This char-table maps each character into the corresponding lower case character. It has three extra slots, which hold related tables:

`upcase`

The upcase table maps each character into the corresponding upper case character.

`canonicalize`

The canonicalize table maps all of a set of case-related characters into a particular member of that set.

`equivalences`

The equivalences table maps each one of a set of case-related characters into the next character in that set.

In simple cases, all you need to specify is the mapping to lower-case; the three related tables will be calculated automatically from that one.

For some languages, upper and lower case letters are not in one-to-one correspondence. There may be two different lower case letters with the same upper case equivalent. In these cases, you need to specify the maps for both lower case and upper case.

The extra table `canonicalize` maps each character to a canonical equivalent; any two characters that are related by case-conversion have the same canonical equivalent character. For example, since ‘`a`’ and ‘`A`’ are related by case-conversion, they should have the same canonical equivalent character (which should be either ‘`a`’ for both of them, or ‘`A`’ for both of them).

The extra table `equivalences` is a map that cyclically permutes each equivalence class (of characters with the same canonical equivalent). (For ordinary ASCII, this would map ‘`a`’ into ‘`A`’ and ‘`A`’ into ‘`a`’, and likewise for each set of equivalent characters.)

When constructing a case table, you can provide `nil` for `canonicalize`; then Emacs fills in this slot from the lower case and upper case mappings. You can also provide `nil` for `equivalences`; then Emacs fills in this slot from `canonicalize`. In a case table that is actually in use, those components are non-`nil`. Do not try to specify `equivalences` without also specifying `canonicalize`.

Here are the functions for working with case tables:

### Function: **case-table-p** *object*

This predicate returns non-`nil` if `object` is a valid case table.

### Function: **set-standard-case-table** *table*

This function makes `table` the standard case table, so that it will be used in any buffers created subsequently.

### Function: **standard-case-table**

This returns the standard case table.

### Function: **current-case-table**

This function returns the current buffer’s case table.

### Function: **set-case-table** *table*

This sets the current buffer’s case table to `table`.

### Macro: **with-case-table** *table body…*

The `with-case-table` macro saves the current case table, makes `table` the current case table, evaluates the `body` forms, and finally restores the case table. The return value is the value of the last form in `body`. The case table is restored even in case of an abnormal exit via `throw` or error (see [Nonlocal Exits](Nonlocal-Exits.html)).

Some language environments modify the case conversions of ASCII characters; for example, in the Turkish language environment, the ASCII capital I is downcased into a Turkish dotless i (‘`ı`’). This can interfere with code that requires ordinary ASCII case conversion, such as implementations of ASCII-based network protocols. In that case, use the `with-case-table` macro with the variable `ascii-case-table`, which stores the unmodified case table for the ASCII character set.

### Variable: **ascii-case-table**

The case table for the ASCII character set. This should not be modified by any language environment settings.

The following three functions are convenient subroutines for packages that define non-ASCII character sets. They modify the specified case table `case-table`; they also modify the standard syntax table. See [Syntax Tables](Syntax-Tables.html). Normally you would use these functions to change the standard case table.

### Function: **set-case-syntax-pair** *uc lc case-table*

This function specifies a pair of corresponding letters, one upper case and one lower case.

### Function: **set-case-syntax-delims** *l r case-table*

This function makes characters `l` and `r` a matching pair of case-invariant delimiters.

### Function: **set-case-syntax** *char syntax case-table*

This function makes `char` case-invariant, with syntax `syntax`.

### Command: **describe-buffer-case-table**

This command displays a description of the contents of the current buffer’s case table.
