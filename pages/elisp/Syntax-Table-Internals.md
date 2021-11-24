

### 35.7 Syntax Table Internals

Syntax tables are implemented as char-tables (see [Char-Tables](Char_002dTables.html)), but most Lisp programs don’t work directly with their elements. Syntax tables do not store syntax data as syntax descriptors (see [Syntax Descriptors](Syntax-Descriptors.html)); they use an internal format, which is documented in this section. This internal format can also be assigned as syntax properties (see [Syntax Properties](Syntax-Properties.html)).

Each entry in a syntax table is a *raw syntax descriptor*: a cons cell of the form `(syntax-code . matching-char)`. `syntax-code` is an integer which encodes the syntax class and syntax flags, according to the table below. `matching-char`, if non-`nil`, specifies a matching character (similar to the second character in a syntax descriptor).

Use `aref` (see [Array Functions](Array-Functions.html)) to get the raw syntax descriptor of a character, e.g. `(aref (syntax-table) ch)`.

Here are the syntax codes corresponding to the various syntax classes:

|        |                   |        |                  |
| ------ | ----------------- | ------ | ---------------- |
| *Code* | *Class*           | *Code* | *Class*          |
| 0      | whitespace        | 8      | paired delimiter |
| 1      | punctuation       | 9      | escape           |
| 2      | word              | 10     | character quote  |
| 3      | symbol            | 11     | comment-start    |
| 4      | open parenthesis  | 12     | comment-end      |
| 5      | close parenthesis | 13     | inherit          |
| 6      | expression prefix | 14     | generic comment  |
| 7      | string quote      | 15     | generic string   |

For example, in the standard syntax table, the entry for ‘`(`’ is `(4 . 41)`. 41 is the character code for ‘`)`’.

Syntax flags are encoded in higher order bits, starting 16 bits from the least significant bit. This table gives the power of two which corresponds to each syntax flag.

|          |              |          |              |
| -------- | ------------ | -------- | ------------ |
| *Prefix* | *Flag*       | *Prefix* | *Flag*       |
| ‘`1`’    | `(ash 1 16)` | ‘`p`’    | `(ash 1 20)` |
| ‘`2`’    | `(ash 1 17)` | ‘`b`’    | `(ash 1 21)` |
| ‘`3`’    | `(ash 1 18)` | ‘`n`’    | `(ash 1 22)` |
| ‘`4`’    | `(ash 1 19)` | ‘`c`’    | `(ash 1 23)` |

### Function: **string-to-syntax** *desc*

Given a syntax descriptor `desc` (a string), this function returns the corresponding raw syntax descriptor.

### Function: **syntax-after** *pos*

This function returns the raw syntax descriptor for the character in the buffer after position `pos`, taking account of syntax properties as well as the syntax table. If `pos` is outside the buffer’s accessible portion (see [accessible portion](Narrowing.html)), the return value is `nil`.

### Function: **syntax-class** *syntax*

This function returns the syntax code for the raw syntax descriptor `syntax`. More precisely, it takes the raw syntax descriptor’s `syntax-code` component, masks off the high 16 bits which record the syntax flags, and returns the resulting integer.

If `syntax` is `nil`, the return value is `nil`. This is so that the expression

```lisp
(syntax-class (syntax-after pos))
```

evaluates to `nil` if `pos` is outside the buffer’s accessible portion, without throwing errors or returning an incorrect code.
