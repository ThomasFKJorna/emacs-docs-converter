

Next: [Customizing Keywords](Customizing-Keywords.html), Previous: [Font Lock Basics](Font-Lock-Basics.html), Up: [Font Lock Mode](Font-Lock-Mode.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 23.6.2 Search-based Fontification

The variable which directly controls search-based fontification is `font-lock-keywords`, which is typically specified via the `keywords` element in `font-lock-defaults`.

*   Variable: **font-lock-keywords**

    The value of this variable is a list of the keywords to highlight. Lisp programs should not set this variable directly. Normally, the value is automatically set by Font Lock mode, using the `keywords` element in `font-lock-defaults`. The value can also be altered using the functions `font-lock-add-keywords` and `font-lock-remove-keywords` (see [Customizing Keywords](Customizing-Keywords.html)).

Each element of `font-lock-keywords` specifies how to find certain cases of text, and how to highlight those cases. Font Lock mode processes the elements of `font-lock-keywords` one by one, and for each element, it finds and handles all matches. Ordinarily, once part of the text has been fontified already, this cannot be overridden by a subsequent match in the same text; but you can specify different behavior using the `override` element of a `subexp-highlighter`.

Each element of `font-lock-keywords` should have one of these forms:

*   `regexp`

    Highlight all matches for `regexp` using `font-lock-keyword-face`. For example,

    ```lisp
    ;; Highlight occurrences of the word ‘foo’
    ;; using font-lock-keyword-face.
    "\\<foo\\>"
    ```

    Be careful when composing these regular expressions; a poorly written pattern can dramatically slow things down! The function `regexp-opt` (see [Regexp Functions](Regexp-Functions.html)) is useful for calculating optimal regular expressions to match several keywords.

*   `function`

    Find text by calling `function`, and highlight the matches it finds using `font-lock-keyword-face`.

    When `function` is called, it receives one argument, the limit of the search; it should begin searching at point, and not search beyond the limit. It should return non-`nil` if it succeeds, and set the match data to describe the match that was found. Returning `nil` indicates failure of the search.

    Fontification will call `function` repeatedly with the same limit, and with point where the previous invocation left it, until `function` fails. On failure, `function` need not reset point in any particular way.

*   `(matcher . subexp)`

    In this kind of element, `matcher` is either a regular expression or a function, as described above. The CDR, `subexp`, specifies which subexpression of `matcher` should be highlighted (instead of the entire text that `matcher` matched).

    ```lisp
    ;; Highlight the ‘bar’ in each occurrence of ‘fubar’,
    ;; using font-lock-keyword-face.
    ("fu\\(bar\\)" . 1)
    ```

    If you use `regexp-opt` to produce the regular expression `matcher`, you can use `regexp-opt-depth` (see [Regexp Functions](Regexp-Functions.html)) to calculate the value for `subexp`.

*   `(matcher . facespec)`

    In this kind of element, `facespec` is an expression whose value specifies the face to use for highlighting. In the simplest case, `facespec` is a Lisp variable (a symbol) whose value is a face name.

    ```lisp
    ;; Highlight occurrences of ‘fubar’,
    ;; using the face which is the value of fubar-face.
    ("fubar" . fubar-face)
    ```

    However, `facespec` can also evaluate to a list of this form:

    ```lisp
    (face face prop1 val1 prop2 val2…)
    ```

    to specify the face `face` and various additional text properties to put on the text that matches. If you do this, be sure to add the other text property names that you set in this way to the value of `font-lock-extra-managed-props` so that the properties will also be cleared out when they are no longer appropriate. Alternatively, you can set the variable `font-lock-unfontify-region-function` to a function that clears these properties. See [Other Font Lock Variables](Other-Font-Lock-Variables.html).

*   `(matcher . subexp-highlighter)`

    In this kind of element, `subexp-highlighter` is a list which specifies how to highlight matches found by `matcher`. It has the form:

    ```lisp
    (subexp facespec [override [laxmatch]])
    ```

    The CAR, `subexp`, is an integer specifying which subexpression of the match to fontify (0 means the entire matching text). The second subelement, `facespec`, is an expression whose value specifies the face, as described above.

    The last two values in `subexp-highlighter`, `override` and `laxmatch`, are optional flags. If `override` is `t`, this element can override existing fontification made by previous elements of `font-lock-keywords`. If it is `keep`, then each character is fontified if it has not been fontified already by some other element. If it is `prepend`, the face specified by `facespec` is added to the beginning of the `font-lock-face` property. If it is `append`, the face is added to the end of the `font-lock-face` property.

    If `laxmatch` is non-`nil`, it means there should be no error if there is no subexpression numbered `subexp` in `matcher`. Obviously, fontification of the subexpression numbered `subexp` will not occur. However, fontification of other subexpressions (and other regexps) will continue. If `laxmatch` is `nil`, and the specified subexpression is missing, then an error is signaled which terminates search-based fontification.

    Here are some examples of elements of this kind, and what they do:

    ```lisp
    ;; Highlight occurrences of either ‘foo’ or ‘bar’, using
    ;; foo-bar-face, even if they have already been highlighted.
    ;; foo-bar-face should be a variable whose value is a face.
    ("foo\\|bar" 0 foo-bar-face t)

    ;; Highlight the first subexpression within each occurrence
    ;; that the function fubar-match finds,
    ;; using the face which is the value of fubar-face.
    (fubar-match 1 fubar-face)
    ```

*   `(matcher . anchored-highlighter)`

    In this kind of element, `anchored-highlighter` specifies how to highlight text that follows a match found by `matcher`. So a match found by `matcher` acts as the anchor for further searches specified by `anchored-highlighter`. `anchored-highlighter` is a list of the following form:

    ```lisp
    (anchored-matcher pre-form post-form
                            subexp-highlighters…)
    ```

    Here, `anchored-matcher`, like `matcher`, is either a regular expression or a function. After a match of `matcher` is found, point is at the end of the match. Now, Font Lock evaluates the form `pre-form`. Then it searches for matches of `anchored-matcher` and uses `subexp-highlighters` to highlight these. A `subexp-highlighter` is as described above. Finally, Font Lock evaluates `post-form`.

    The forms `pre-form` and `post-form` can be used to initialize before, and cleanup after, `anchored-matcher` is used. Typically, `pre-form` is used to move point to some position relative to the match of `matcher`, before starting with `anchored-matcher`. `post-form` might be used to move back, before resuming with `matcher`.

    After Font Lock evaluates `pre-form`, it does not search for `anchored-matcher` beyond the end of the line. However, if `pre-form` returns a buffer position that is greater than the position of point after `pre-form` is evaluated, then the position returned by `pre-form` is used as the limit of the search instead. It is generally a bad idea to return a position greater than the end of the line; in other words, the `anchored-matcher` search should not span lines.

    For example,

    ```lisp
    ;; Highlight occurrences of the word ‘item’ following
    ;; an occurrence of the word ‘anchor’ (on the same line)
    ;; in the value of item-face.
    ("\\<anchor\\>" "\\<item\\>" nil nil (0 item-face))
    ```

    Here, `pre-form` and `post-form` are `nil`. Therefore searching for ‘`item`’ starts at the end of the match of ‘`anchor`’, and searching for subsequent instances of ‘`anchor`’ resumes from where searching for ‘`item`’ concluded.

*   `(matcher highlighters…)`

    This sort of element specifies several `highlighter` lists for a single `matcher`. A `highlighter` list can be of the type `subexp-highlighter` or `anchored-highlighter` as described above.

    For example,

    ```lisp
    ;; Highlight occurrences of the word ‘anchor’ in the value
    ;; of anchor-face, and subsequent occurrences of the word
    ;; ‘item’ (on the same line) in the value of item-face.
    ("\\<anchor\\>" (0 anchor-face)
                    ("\\<item\\>" nil nil (0 item-face)))
    ```

*   `(eval . form)`

    Here `form` is an expression to be evaluated the first time this value of `font-lock-keywords` is used in a buffer. Its value should have one of the forms described in this table.

**Warning:** Do not design an element of `font-lock-keywords` to match text which spans lines; this does not work reliably. For details, see [Multiline Font Lock](Multiline-Font-Lock.html).

You can use `case-fold` in `font-lock-defaults` to specify the value of `font-lock-keywords-case-fold-search` which says whether search-based fontification should be case-insensitive.

*   Variable: **font-lock-keywords-case-fold-search**

    Non-`nil` means that regular expression matching for the sake of `font-lock-keywords` should be case-insensitive.

Next: [Customizing Keywords](Customizing-Keywords.html), Previous: [Font Lock Basics](Font-Lock-Basics.html), Up: [Font Lock Mode](Font-Lock-Mode.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
