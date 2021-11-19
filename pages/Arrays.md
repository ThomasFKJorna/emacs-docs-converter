

Next: [Array Functions](Array-Functions.html), Previous: [Sequence Functions](Sequence-Functions.html), Up: [Sequences Arrays Vectors](Sequences-Arrays-Vectors.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 6.2 Arrays

An *array* object has slots that hold a number of other Lisp objects, called the elements of the array. Any element of an array may be accessed in constant time. In contrast, the time to access an element of a list is proportional to the position of that element in the list.

Emacs defines four types of array, all one-dimensional: *strings* (see [String Type](String-Type.html)), *vectors* (see [Vector Type](Vector-Type.html)), *bool-vectors* (see [Bool-Vector Type](Bool_002dVector-Type.html)), and *char-tables* (see [Char-Table Type](Char_002dTable-Type.html)). Vectors and char-tables can hold elements of any type, but strings can only hold characters, and bool-vectors can only hold `t` and `nil`.

All four kinds of array share these characteristics:

*   The first element of an array has index zero, the second element has index 1, and so on. This is called *zero-origin* indexing. For example, an array of four elements has indices 0, 1, 2, and 3.
*   The length of the array is fixed once you create it; you cannot change the length of an existing array.
*   For purposes of evaluation, the array is a constant—i.e., it evaluates to itself.
*   The elements of an array may be referenced or changed with the functions `aref` and `aset`, respectively (see [Array Functions](Array-Functions.html)).

When you create an array, other than a char-table, you must specify its length. You cannot specify the length of a char-table, because that is determined by the range of character codes.

In principle, if you want an array of text characters, you could use either a string or a vector. In practice, we always choose strings for such applications, for four reasons:

*   They occupy one-fourth the space of a vector of the same elements.
*   Strings are printed in a way that shows the contents more clearly as text.
*   Strings can hold text properties. See [Text Properties](Text-Properties.html).
*   Many of the specialized editing and I/O facilities of Emacs accept only strings. For example, you cannot insert a vector of characters into a buffer the way you can insert a string. See [Strings and Characters](Strings-and-Characters.html).

By contrast, for an array of keyboard input characters (such as a key sequence), a vector may be necessary, because many keyboard input characters are outside the range that will fit in a string. See [Key Sequence Input](Key-Sequence-Input.html).

Next: [Array Functions](Array-Functions.html), Previous: [Sequence Functions](Sequence-Functions.html), Up: [Sequences Arrays Vectors](Sequences-Arrays-Vectors.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
