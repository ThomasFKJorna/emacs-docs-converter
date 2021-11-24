

#### 23.7.1.6 Specifying Indentation Rules

Based on the provided grammar, SMIE will be able to provide automatic indentation without any extra effort. But in practice, this default indentation style will probably not be good enough. You will want to tweak it in many different cases.

SMIE indentation is based on the idea that indentation rules should be as local as possible. To this end, it relies on the idea of *virtual* indentation, which is the indentation that a particular program point would have if it were at the beginning of a line. Of course, if that program point is indeed at the beginning of a line, its virtual indentation is its current indentation. But if not, then SMIE uses the indentation algorithm to compute the virtual indentation of that point. Now in practice, the virtual indentation of a program point does not have to be identical to the indentation it would have if we inserted a newline before it. To see how this works, the SMIE rule for indentation after a `{` in C does not care whether the `{` is standing on a line of its own or is at the end of the preceding line. Instead, these different cases are handled in the indentation rule that decides how to indent before a `{`.

Another important concept is the notion of *parent*: The *parent* of a token, is the head token of the nearest enclosing syntactic construct. For example, the parent of an `else` is the `if` to which it belongs, and the parent of an `if`, in turn, is the lead token of the surrounding construct. The command `backward-sexp` jumps from a token to its parent, but there are some caveats: for *openers* (tokens which start a construct, like `if`), you need to start with point before the token, while for others you need to start with point after the token. `backward-sexp` stops with point before the parent token if that is the *opener* of the token of interest, and otherwise it stops with point after the parent token.

SMIE indentation rules are specified using a function that takes two arguments `method` and `arg` where the meaning of `arg` and the expected return value depend on `method`.

`method` can be:

### `:after`, in which case `arg` is a token and the function should return the `offset` to use for indentation after `arg`.

### `:before`, in which case `arg` is a token and the function should return the `offset` to use to indent `arg` itself.

### `:elem`, in which case the function should return either the offset to use to indent function arguments (if `arg` is the symbol `arg`) or the basic indentation step (if `arg` is the symbol `basic`).

### `:list-intro`, in which case `arg` is a token and the function should return non-`nil` if the token is followed by a list of expressions (not separated by any token) rather than an expression.

When `arg` is a token, the function is called with point just before that token. A return value of `nil` always means to fallback on the default behavior, so the function should return `nil` for arguments it does not expect.

`offset` can be:

`nil`: use the default indentation rule.

`(column . column)`: indent to column `column`.

`number`: offset by `number`, relative to a base token which is the current token for `:after` and its parent for `:before`.
