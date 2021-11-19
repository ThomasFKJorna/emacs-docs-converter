

Previous: [Argument List](Argument-List.html), Up: [Lambda Expressions](Lambda-Expressions.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 13.2.4 Documentation Strings of Functions

A lambda expression may optionally have a *documentation string* just after the lambda list. This string does not affect execution of the function; it is a kind of comment, but a systematized comment which actually appears inside the Lisp world and can be used by the Emacs help facilities. See [Documentation](Documentation.html), for how the documentation string is accessed.

It is a good idea to provide documentation strings for all the functions in your program, even those that are called only from within your program. Documentation strings are like comments, except that they are easier to access.

The first line of the documentation string should stand on its own, because `apropos` displays just this first line. It should consist of one or two complete sentences that summarize the function’s purpose.

The start of the documentation string is usually indented in the source file, but since these spaces come before the starting double-quote, they are not part of the string. Some people make a practice of indenting any additional lines of the string so that the text lines up in the program source. *That is a mistake.* The indentation of the following lines is inside the string; what looks nice in the source code will look ugly when displayed by the help commands.

You may wonder how the documentation string could be optional, since there are required components of the function that follow it (the body). Since evaluation of a string returns that string, without any side effects, it has no effect if it is not the last form in the body. Thus, in practice, there is no confusion between the first form of the body and the documentation string; if the only body form is a string then it serves both as the return value and as the documentation.

The last line of the documentation string can specify calling conventions different from the actual function arguments. Write text like this:

```lisp
\(fn arglist)
```

following a blank line, at the beginning of the line, with no newline following it inside the documentation string. (The ‘`\`’ is used to avoid confusing the Emacs motion commands.) The calling convention specified in this way appears in help messages in place of the one derived from the actual arguments of the function.

This feature is particularly useful for macro definitions, since the arguments written in a macro definition often do not correspond to the way users think of the parts of the macro call.

Do not use this feature if you want to deprecate the calling convention and favor the one you advertise by the above specification. Instead, use the `advertised-calling-convention` declaration (see [Declare Form](Declare-Form.html)) or `set-advertised-calling-convention` (see [Obsolete Functions](Obsolete-Functions.html)), because these two will cause the byte compiler emit a warning message when it compiles Lisp programs which use the deprecated calling convention.

Previous: [Argument List](Argument-List.html), Up: [Lambda Expressions](Lambda-Expressions.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
