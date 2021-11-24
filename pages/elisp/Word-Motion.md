

#### 30.2.2 Motion by Words

The functions for parsing words described below use the syntax table and `char-script-table` to decide whether a given character is part of a word. See [Syntax Tables](Syntax-Tables.html), and see [Character Properties](Character-Properties.html).

### Command: **forward-word** *\&optional count*

This function moves point forward `count` words (or backward if `count` is negative). If `count` is omitted or `nil`, it defaults to 1. In an interactive call, `count` is specified by the numeric prefix argument.

“Moving one word” means moving until point crosses a word-constituent character, which indicates the beginning of a word, and then continue moving until the word ends. By default, characters that begin and end words, known as *word boundaries*, are defined by the current buffer’s syntax table (see [Syntax Class Table](Syntax-Class-Table.html)), but modes can override that by setting up a suitable `find-word-boundary-function-table`, described below. Characters that belong to different scripts (as defined by `char-script-table`), also define a word boundary (see [Character Properties](Character-Properties.html)). In any case, this function cannot move point past the boundary of the accessible portion of the buffer, or across a field boundary (see [Fields](Fields.html)). The most common case of a field boundary is the end of the prompt in the minibuffer.

If it is possible to move `count` words, without being stopped prematurely by the buffer boundary or a field boundary, the value is `t`. Otherwise, the return value is `nil` and point stops at the buffer boundary or field boundary.

If `inhibit-field-text-motion` is non-`nil`, this function ignores field boundaries.

### Command: **backward-word** *\&optional count*

This function is just like `forward-word`, except that it moves backward until encountering the front of a word, rather than forward.

### User Option: **words-include-escapes**

This variable affects the behavior of `forward-word` and `backward-word`, and everything that uses them. If it is non-`nil`, then characters in the escape and character-quote syntax classes count as part of words. Otherwise, they do not.

### Variable: **inhibit-field-text-motion**

If this variable is non-`nil`, certain motion functions including `forward-word`, `forward-sentence`, and `forward-paragraph` ignore field boundaries.

### Variable: **find-word-boundary-function-table**

This variable affects the behavior of `forward-word` and `backward-word`, and everything that uses them. Its value is a char-table (see [Char-Tables](Char_002dTables.html)) of functions to search for word boundaries. If a character has a non-`nil` entry in this table, then when a word starts or ends with that character, the corresponding function will be called with 2 arguments: `pos` and `limit`. The function should return the position of the other word boundary. Specifically, if `pos` is smaller than `limit`, then `pos` is at the beginning of a word, and the function should return the position after the last character of the word; otherwise, `pos` is at the last character of a word, and the function should return the position of that word’s first character.

### Function: **forward-word-strictly** *\&optional count*

This function is like `forward-word`, but it is not affected by `find-word-boundary-function-table`. Lisp programs that should not change behavior when word movement is modified by modes which set that table, such as `subword-mode`, should use this function instead of `forward-word`.

### Function: **backward-word-strictly** *\&optional count*

This function is like `backward-word`, but it is not affected by `find-word-boundary-function-table`. Like with `forward-word-strictly`, use this function instead of `backward-word` when movement by words should only consider syntax tables.
