

#### 23.6.7 Faces for Font Lock

Font Lock mode can highlight using any face, but Emacs defines several faces specifically for Font Lock to use to highlight text. These *Font Lock faces* are listed below. They can also be used by major modes for syntactic highlighting outside of Font Lock mode (see [Major Mode Conventions](Major-Mode-Conventions.html)).

Each of these symbols is both a face name, and a variable whose default value is the symbol itself. Thus, the default value of `font-lock-comment-face` is `font-lock-comment-face`.

The faces are listed with descriptions of their typical usage, and in order of greater to lesser prominence. If a mode’s syntactic categories do not fit well with the usage descriptions, the faces can be assigned using the ordering as a guide.

`font-lock-warning-face`

for a construct that is peculiar (e.g., an unescaped confusable quote in an Emacs Lisp symbol like ‘`‘foo`’), or that greatly changes the meaning of other text, like ‘`;;;###autoload`’ in Emacs Lisp and ‘`#error`’ in C.

`font-lock-function-name-face`

for the name of a function being defined or declared.

`font-lock-variable-name-face`

for the name of a variable being defined or declared.

`font-lock-keyword-face`

for a keyword with special syntactic significance, like ‘`for`’ and ‘`if`’ in C.

`font-lock-comment-face`

for comments.

`font-lock-comment-delimiter-face`

for comments delimiters, like ‘`/*`’ and ‘`*/`’ in C. On most terminals, this inherits from `font-lock-comment-face`.

`font-lock-type-face`

for the names of user-defined data types.

`font-lock-constant-face`

for the names of constants, like ‘`NULL`’ in C.

`font-lock-builtin-face`

for the names of built-in functions.

`font-lock-preprocessor-face`

for preprocessor commands. This inherits, by default, from `font-lock-builtin-face`.

`font-lock-string-face`

for string constants.

`font-lock-doc-face`

for documentation strings in the code. This inherits, by default, from `font-lock-string-face`.

`font-lock-negation-char-face`

for easily-overlooked negation characters.
