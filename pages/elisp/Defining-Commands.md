

### 21.2 Defining Commands

The special form `interactive` turns a Lisp function into a command. The `interactive` form must be located at top-level in the function body, usually as the first form in the body; this applies to both lambda expressions (see [Lambda Expressions](Lambda-Expressions.html)) and `defun` forms (see [Defining Functions](Defining-Functions.html)). This form does nothing during the actual execution of the function; its presence serves as a flag, telling the Emacs command loop that the function can be called interactively. The argument of the `interactive` form specifies how the arguments for an interactive call should be read.

Alternatively, an `interactive` form may be specified in a function symbol’s `interactive-form` property. A non-`nil` value for this property takes precedence over any `interactive` form in the function body itself. This feature is seldom used.

Sometimes, a function is only intended to be called interactively, never directly from Lisp. In that case, give the function a non-`nil` `interactive-only` property, either directly or via `declare` (see [Declare Form](Declare-Form.html)). This causes the byte compiler to warn if the command is called from Lisp. The output of `describe-function` will include similar information. The value of the property can be: a string, which the byte-compiler will use directly in its warning (it should end with a period, and not start with a capital, e.g., `"use (system-name) instead."`); `t`; any other symbol, which should be an alternative function to use in Lisp code.

Generic functions (see [Generic Functions](Generic-Functions.html)) cannot be turned into commands by adding the `interactive` form to them.

|                                                     |    |                                                                  |
| :-------------------------------------------------- | -- | :--------------------------------------------------------------- |
| • [Using Interactive](Using-Interactive.html)       |    | General rules for `interactive`.                                 |
| • [Interactive Codes](Interactive-Codes.html)       |    | The standard letter-codes for reading arguments in various ways. |
| • [Interactive Examples](Interactive-Examples.html) |    | Examples of how to read interactive arguments.                   |
| • [Generic Commands](Generic-Commands.html)         |    | Select among command alternatives.                               |
