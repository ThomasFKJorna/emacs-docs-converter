

### 35.4 Syntax Properties

When the syntax table is not flexible enough to specify the syntax of a language, you can override the syntax table for specific character occurrences in the buffer, by applying a `syntax-table` text property. See [Text Properties](Text-Properties.html), for how to apply text properties.

The valid values of `syntax-table` text property are:

`syntax-table`

If the property value is a syntax table, that table is used instead of the current buffer’s syntax table to determine the syntax for the underlying text character.

`(syntax-code . matching-char)`

A cons cell of this format is a raw syntax descriptor (see [Syntax Table Internals](Syntax-Table-Internals.html)), which directly specifies a syntax class for the underlying text character.

`nil`

If the property is `nil`, the character’s syntax is determined from the current syntax table in the usual way.

### Variable: **parse-sexp-lookup-properties**

If this is non-`nil`, the syntax scanning functions, like `forward-sexp`, pay attention to `syntax-table` text properties. Otherwise they use only the current syntax table.

### Variable: **syntax-propertize-function**

This variable, if non-`nil`, should store a function for applying `syntax-table` properties to a specified stretch of text. It is intended to be used by major modes to install a function which applies `syntax-table` properties in some mode-appropriate way.

The function is called by `syntax-ppss` (see [Position Parse](Position-Parse.html)), and by Font Lock mode during syntactic fontification (see [Syntactic Font Lock](Syntactic-Font-Lock.html)). It is called with two arguments, `start` and `end`, which are the starting and ending positions of the text on which it should act. It is allowed to call `syntax-ppss` on any position before `end`, but if a Lisp program calls `syntax-ppss` on some position and later modifies the buffer at some earlier position, then it is that program’s responsibility to call `syntax-ppss-flush-cache` to flush the now obsolete info from the cache.

**Caution:** When this variable is non-`nil`, Emacs removes `syntax-table` text properties arbitrarily and relies on `syntax-propertize-function` to reapply them. Thus if this facility is used at all, the function must apply **all** `syntax-table` text properties used by the major mode. In particular, Modes derived from a CC Mode mode must not use this variable, since CC Mode uses other means to apply and remove these text properties.

### Variable: **syntax-propertize-extend-region-functions**

This abnormal hook is run by the syntax parsing code prior to calling `syntax-propertize-function`. Its role is to help locate safe starting and ending buffer positions for passing to `syntax-propertize-function`. For example, a major mode can add a function to this hook to identify multi-line syntactic constructs, and ensure that the boundaries do not fall in the middle of one.

Each function in this hook should accept two arguments, `start` and `end`. It should return either a cons cell of two adjusted buffer positions, `(new-start . new-end)`, or `nil` if no adjustment is necessary. The hook functions are run in turn, repeatedly, until they all return `nil`.
