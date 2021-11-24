

### 4.1 String and Character Basics

A character is a Lisp object which represents a single character of text. In Emacs Lisp, characters are simply integers; whether an integer is a character or not is determined only by how it is used. See [Character Codes](Character-Codes.html), for details about character representation in Emacs.

A string is a fixed sequence of characters. It is a type of sequence called a *array*, meaning that its length is fixed and cannot be altered once it is created (see [Sequences Arrays Vectors](Sequences-Arrays-Vectors.html)). Unlike in C, Emacs Lisp strings are *not* terminated by a distinguished character code.

Since strings are arrays, and therefore sequences as well, you can operate on them with the general array and sequence functions documented in [Sequences Arrays Vectors](Sequences-Arrays-Vectors.html). For example, you can access individual characters in a string using the function `aref` (see [Array Functions](Array-Functions.html)).

There are two text representations for non-ASCII characters in Emacs strings (and in buffers): unibyte and multibyte. For most Lisp programming, you don’t need to be concerned with these two representations. See [Text Representations](Text-Representations.html), for details.

Sometimes key sequences are represented as unibyte strings. When a unibyte string is a key sequence, string elements in the range 128 to 255 represent meta characters (which are large integers) rather than character codes in the range 128 to 255. Strings cannot hold characters that have the hyper, super or alt modifiers; they can hold ASCII control characters, but no other control characters. They do not distinguish case in ASCII control characters. If you want to store such characters in a sequence, such as a key sequence, you must use a vector instead of a string. See [Character Type](Character-Type.html), for more information about keyboard input characters.

Strings are useful for holding regular expressions. You can also match regular expressions against strings with `string-match` (see [Regexp Search](Regexp-Search.html)). The functions `match-string` (see [Simple Match Data](Simple-Match-Data.html)) and `replace-match` (see [Replacing Match](Replacing-Match.html)) are useful for decomposing and modifying strings after matching regular expressions against them.

Like a buffer, a string can contain text properties for the characters in it, as well as the characters themselves. See [Text Properties](Text-Properties.html). All the Lisp primitives that copy text from strings to buffers or other strings also copy the properties of the characters being copied.

See [Text](Text.html), for information about functions that display strings or copy them into buffers. See [Character Type](Character-Type.html), and [String Type](String-Type.html), for information about the syntax of characters and strings. See [Non-ASCII Characters](Non_002dASCII-Characters.html), for functions to convert between text representations and to encode and decode character codes. Also, note that `length` should *not* be used for computing the width of a string on display; use `string-width` (see [Size of Displayed Text](Size-of-Displayed-Text.html)) instead.
