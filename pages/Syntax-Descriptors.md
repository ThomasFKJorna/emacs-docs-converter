

Next: [Syntax Table Functions](Syntax-Table-Functions.html), Previous: [Syntax Basics](Syntax-Basics.html), Up: [Syntax Tables](Syntax-Tables.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 35.2 Syntax Descriptors

The *syntax class* of a character describes its syntactic role. Each syntax table specifies the syntax class of each character. There is no necessary relationship between the class of a character in one syntax table and its class in any other table.

Each syntax class is designated by a mnemonic character, which serves as the name of the class when you need to specify a class. Usually, this designator character is one that is often assigned that class; however, its meaning as a designator is unvarying and independent of what syntax that character currently has. Thus, ‘`\`’ as a designator character always stands for escape character syntax, regardless of whether the ‘`\`’ character actually has that syntax in the current syntax table. See [Syntax Class Table](Syntax-Class-Table.html), for a list of syntax classes and their designator characters.

A *syntax descriptor* is a Lisp string that describes the syntax class and other syntactic properties of a character. When you want to modify the syntax of a character, that is done by calling the function `modify-syntax-entry` and passing a syntax descriptor as one of its arguments (see [Syntax Table Functions](Syntax-Table-Functions.html)).

The first character in a syntax descriptor must be a syntax class designator character. The second character, if present, specifies a matching character (e.g., in Lisp, the matching character for ‘`(`’ is ‘`)`’); a space specifies that there is no matching character. Then come characters specifying additional syntax properties (see [Syntax Flags](Syntax-Flags.html)).

If no matching character or flags are needed, only one character (specifying the syntax class) is sufficient.

For example, the syntax descriptor for the character ‘`*`’ in C mode is `". 23"` (i.e., punctuation, matching character slot unused, second character of a comment-starter, first character of a comment-ender), and the entry for ‘`/`’ is ‘`. 14`’ (i.e., punctuation, matching character slot unused, first character of a comment-starter, second character of a comment-ender).

Emacs also defines *raw syntax descriptors*, which are used to describe syntax classes at a lower level. See [Syntax Table Internals](Syntax-Table-Internals.html).

|                                                 |    |                                           |
| :---------------------------------------------- | -- | :---------------------------------------- |
| • [Syntax Class Table](Syntax-Class-Table.html) |    | Table of syntax classes.                  |
| • [Syntax Flags](Syntax-Flags.html)             |    | Additional flags each character can have. |

Next: [Syntax Table Functions](Syntax-Table-Functions.html), Previous: [Syntax Basics](Syntax-Basics.html), Up: [Syntax Tables](Syntax-Tables.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
