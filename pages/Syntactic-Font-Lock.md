

Next: [Multiline Font Lock](Multiline-Font-Lock.html), Previous: [Faces for Font Lock](Faces-for-Font-Lock.html), Up: [Font Lock Mode](Font-Lock-Mode.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 23.6.8 Syntactic Font Lock

Syntactic fontification uses a syntax table (see [Syntax Tables](Syntax-Tables.html)) to find and highlight syntactically relevant text. If enabled, it runs prior to search-based fontification. The variable `font-lock-syntactic-face-function`, documented below, determines which syntactic constructs to highlight. There are several variables that affect syntactic fontification; you should set them by means of `font-lock-defaults` (see [Font Lock Basics](Font-Lock-Basics.html)).

Whenever Font Lock mode performs syntactic fontification on a stretch of text, it first calls the function specified by `syntax-propertize-function`. Major modes can use this to apply `syntax-table` text properties to override the buffer’s syntax table in special cases. See [Syntax Properties](Syntax-Properties.html).

*   Variable: **font-lock-keywords-only**

    If the value of this variable is non-`nil`, Font Lock does not do syntactic fontification, only search-based fontification based on `font-lock-keywords`. It is normally set by Font Lock mode based on the `keywords-only` element in `font-lock-defaults`. If the value is `nil`, Font Lock will call `jit-lock-register` (see [Other Font Lock Variables](Other-Font-Lock-Variables.html)) to set up for automatic refontification of buffer text following a modified line to reflect the new syntactic context due to the change.

<!---->

*   Variable: **font-lock-syntax-table**

    This variable holds the syntax table to use for fontification of comments and strings. It is normally set by Font Lock mode based on the `syntax-alist` element in `font-lock-defaults`. If this value is `nil`, syntactic fontification uses the buffer’s syntax table (the value returned by the function `syntax-table`; see [Syntax Table Functions](Syntax-Table-Functions.html)).

<!---->

*   Variable: **font-lock-syntactic-face-function**

    If this variable is non-`nil`, it should be a function to determine which face to use for a given syntactic element (a string or a comment).

    The function is called with one argument, the parse state at point returned by `parse-partial-sexp`, and should return a face. The default value returns `font-lock-comment-face` for comments and `font-lock-string-face` for strings (see [Faces for Font Lock](Faces-for-Font-Lock.html)).

    This variable is normally set through the “other” elements in `font-lock-defaults`:

    ```lisp
    (setq-local font-lock-defaults
                `(,python-font-lock-keywords
                  nil nil nil nil
                  (font-lock-syntactic-face-function
                   . python-font-lock-syntactic-face-function)))
    ```

Next: [Multiline Font Lock](Multiline-Font-Lock.html), Previous: [Faces for Font Lock](Faces-for-Font-Lock.html), Up: [Font Lock Mode](Font-Lock-Mode.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
