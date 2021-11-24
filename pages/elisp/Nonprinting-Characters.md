

#### 2.4.8.3 Nonprinting Characters in Strings

You can use the same backslash escape-sequences in a string constant as in character literals (but do not use the question mark that begins a character constant). For example, you can write a string containing the nonprinting characters tab and `C-a`, with commas and spaces between them, like this: `"\t, \C-a"`. See [Character Type](Character-Type.html), for a description of the read syntax for characters.

However, not all of the characters you can write with backslash escape-sequences are valid in strings. The only control characters that a string can hold are the ASCII control characters. Strings do not distinguish case in ASCII control characters.

Properly speaking, strings cannot hold meta characters; but when a string is to be used as a key sequence, there is a special convention that provides a way to represent meta versions of ASCII characters in a string. If you use the ‘`\M-`’ syntax to indicate a meta character in a string constant, this sets the 2\*\*7 bit of the character in the string. If the string is used in `define-key` or `lookup-key`, this numeric code is translated into the equivalent meta character. See [Character Type](Character-Type.html).

Strings cannot hold characters that have the hyper, super, or alt modifiers.
