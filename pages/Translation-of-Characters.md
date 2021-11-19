

Next: [Coding Systems](Coding-Systems.html), Previous: [Scanning Charsets](Scanning-Charsets.html), Up: [Non-ASCII Characters](Non_002dASCII-Characters.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 33.9 Translation of Characters

A *translation table* is a char-table (see [Char-Tables](Char_002dTables.html)) that specifies a mapping of characters into characters. These tables are used in encoding and decoding, and for other purposes. Some coding systems specify their own particular translation tables; there are also default translation tables which apply to all other coding systems.

A translation table has two extra slots. The first is either `nil` or a translation table that performs the reverse translation; the second is the maximum number of characters to look up for translating sequences of characters (see the description of `make-translation-table-from-alist` below).

*   Function: **make-translation-table** *\&rest translations*

    This function returns a translation table based on the argument `translations`. Each element of `translations` should be a list of elements of the form `(from . to)`; this says to translate the character `from` into `to`.

    The arguments and the forms in each argument are processed in order, and if a previous form already translates `to` to some other character, say `to-alt`, `from` is also translated to `to-alt`.

During decoding, the translation table’s translations are applied to the characters that result from ordinary decoding. If a coding system has the property `:decode-translation-table`, that specifies the translation table to use, or a list of translation tables to apply in sequence. (This is a property of the coding system, as returned by `coding-system-get`, not a property of the symbol that is the coding system’s name. See [Basic Concepts of Coding Systems](Coding-System-Basics.html).) Finally, if `standard-translation-table-for-decode` is non-`nil`, the resulting characters are translated by that table.

During encoding, the translation table’s translations are applied to the characters in the buffer, and the result of translation is actually encoded. If a coding system has property `:encode-translation-table`, that specifies the translation table to use, or a list of translation tables to apply in sequence. In addition, if the variable `standard-translation-table-for-encode` is non-`nil`, it specifies the translation table to use for translating the result.

*   Variable: **standard-translation-table-for-decode**

    This is the default translation table for decoding. If a coding systems specifies its own translation tables, the table that is the value of this variable, if non-`nil`, is applied after them.

<!---->

*   Variable: **standard-translation-table-for-encode**

    This is the default translation table for encoding. If a coding systems specifies its own translation tables, the table that is the value of this variable, if non-`nil`, is applied after them.

<!---->

*   Variable: **translation-table-for-input**

    Self-inserting characters are translated through this translation table before they are inserted. Search commands also translate their input through this table, so they can compare more reliably with what’s in the buffer.

    This variable automatically becomes buffer-local when set.

<!---->

*   Function: **make-translation-table-from-vector** *vec*

    This function returns a translation table made from `vec` that is an array of 256 elements to map bytes (values 0 through #xFF) to characters. Elements may be `nil` for untranslated bytes. The returned table has a translation table for reverse mapping in the first extra slot, and the value `1` in the second extra slot.

    This function provides an easy way to make a private coding system that maps each byte to a specific character. You can specify the returned table and the reverse translation table using the properties `:decode-translation-table` and `:encode-translation-table` respectively in the `props` argument to `define-coding-system`.

<!---->

*   Function: **make-translation-table-from-alist** *alist*

    This function is similar to `make-translation-table` but returns a complex translation table rather than a simple one-to-one mapping. Each element of `alist` is of the form `(from . to)`, where `from` and `to` are either characters or vectors specifying a sequence of characters. If `from` is a character, that character is translated to `to` (i.e., to a character or a character sequence). If `from` is a vector of characters, that sequence is translated to `to`. The returned table has a translation table for reverse mapping in the first extra slot, and the maximum length of all the `from` character sequences in the second extra slot.

Next: [Coding Systems](Coding-Systems.html), Previous: [Scanning Charsets](Scanning-Charsets.html), Up: [Non-ASCII Characters](Non_002dASCII-Characters.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
