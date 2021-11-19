

Next: [Syntax Table Internals](Syntax-Table-Internals.html), Previous: [Motion and Syntax](Motion-and-Syntax.html), Up: [Syntax Tables](Syntax-Tables.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 35.6 Parsing Expressions

This section describes functions for parsing and scanning balanced expressions. We will refer to such expressions as *sexps*, following the terminology of Lisp, even though these functions can act on languages other than Lisp. Basically, a sexp is either a balanced parenthetical grouping, a string, or a symbol (i.e., a sequence of characters whose syntax is either word constituent or symbol constituent). However, characters in the expression prefix syntax class (see [Syntax Class Table](Syntax-Class-Table.html)) are treated as part of the sexp if they appear next to it.

The syntax table controls the interpretation of characters, so these functions can be used for Lisp expressions when in Lisp mode and for C expressions when in C mode. See [List Motion](List-Motion.html), for convenient higher-level functions for moving over balanced expressions.

A character’s syntax controls how it changes the state of the parser, rather than describing the state itself. For example, a string delimiter character toggles the parser state between in-string and in-code, but the syntax of characters does not directly say whether they are inside a string. For example (note that 15 is the syntax code for generic string delimiters),

```lisp
(put-text-property 1 9 'syntax-table '(15 . nil))
```

does not tell Emacs that the first eight chars of the current buffer are a string, but rather that they are all string delimiters. As a result, Emacs treats them as four consecutive empty string constants.

|                                                   |    |                                                |
| :------------------------------------------------ | -- | :--------------------------------------------- |
| • [Motion via Parsing](Motion-via-Parsing.html)   |    | Motion functions that work by parsing.         |
| • [Position Parse](Position-Parse.html)           |    | Determining the syntactic state of a position. |
| • [Parser State](Parser-State.html)               |    | How Emacs represents a syntactic state.        |
| • [Low-Level Parsing](Low_002dLevel-Parsing.html) |    | Parsing across a specified region.             |
| • [Control Parsing](Control-Parsing.html)         |    | Parameters that affect parsing.                |

Next: [Syntax Table Internals](Syntax-Table-Internals.html), Previous: [Motion and Syntax](Motion-and-Syntax.html), Up: [Syntax Tables](Syntax-Tables.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
