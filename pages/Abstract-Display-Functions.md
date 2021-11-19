

Next: [Abstract Display Example](Abstract-Display-Example.html), Up: [Abstract Display](Abstract-Display.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 39.20.1 Abstract Display Functions

In this subsection, `ewoc` and `node` stand for the structures described above (see [Abstract Display](Abstract-Display.html)), while `data` stands for an arbitrary Lisp object used as a data element.

*   Function: **ewoc-create** *pretty-printer \&optional header footer nosep*

    This constructs and returns a new ewoc, with no nodes (and thus no data elements). `pretty-printer` should be a function that takes one argument, a data element of the sort you plan to use in this ewoc, and inserts its textual description at point using `insert` (and never `insert-before-markers`, because that would interfere with the Ewoc package’s internal mechanisms).

    Normally, a newline is automatically inserted after the header, the footer and every node’s textual description. If `nosep` is non-`nil`, no newline is inserted. This may be useful for displaying an entire ewoc on a single line, for example, or for making nodes invisible by arranging for `pretty-printer` to do nothing for those nodes.

    An ewoc maintains its text in the buffer that is current when you create it, so switch to the intended buffer before calling `ewoc-create`.

<!---->

*   Function: **ewoc-buffer** *ewoc*

    This returns the buffer where `ewoc` maintains its text.

<!---->

*   Function: **ewoc-get-hf** *ewoc*

    This returns a cons cell `(header . footer)` made from `ewoc`’s header and footer.

<!---->

*   Function: **ewoc-set-hf** *ewoc header footer*

    This sets the header and footer of `ewoc` to the strings `header` and `footer`, respectively.

<!---->

*   *   Function: **ewoc-enter-first** *ewoc data*
    *   Function: **ewoc-enter-last** *ewoc data*

    These add a new node encapsulating `data`, putting it, respectively, at the beginning or end of `ewoc`’s chain of nodes.

<!---->

*   *   Function: **ewoc-enter-before** *ewoc node data*
    *   Function: **ewoc-enter-after** *ewoc node data*

    These add a new node encapsulating `data`, adding it to `ewoc` before or after `node`, respectively.

<!---->

*   *   Function: **ewoc-prev** *ewoc node*
    *   Function: **ewoc-next** *ewoc node*

    These return, respectively, the previous node and the next node of `node` in `ewoc`.

<!---->

*   Function: **ewoc-nth** *ewoc n*

    This returns the node in `ewoc` found at zero-based index `n`. A negative `n` means count from the end. `ewoc-nth` returns `nil` if `n` is out of range.

<!---->

*   Function: **ewoc-data** *node*

    This extracts the data encapsulated by `node` and returns it.

<!---->

*   Function: **ewoc-set-data** *node data*

    This sets the data encapsulated by `node` to `data`.

<!---->

*   Function: **ewoc-locate** *ewoc \&optional pos guess*

    This determines the node in `ewoc` which contains point (or `pos` if specified), and returns that node. If `ewoc` has no nodes, it returns `nil`. If `pos` is before the first node, it returns the first node; if `pos` is after the last node, it returns the last node. The optional third arg `guess` should be a node that is likely to be near `pos`; this doesn’t alter the result, but makes the function run faster.

<!---->

*   Function: **ewoc-location** *node*

    This returns the start position of `node`.

<!---->

*   *   Function: **ewoc-goto-prev** *ewoc arg*
    *   Function: **ewoc-goto-next** *ewoc arg*

    These move point to the previous or next, respectively, `arg`th node in `ewoc`. `ewoc-goto-prev` does not move if it is already at the first node or if `ewoc` is empty, whereas `ewoc-goto-next` moves past the last node, returning `nil`. Excepting this special case, these functions return the node moved to.

<!---->

*   Function: **ewoc-goto-node** *ewoc node*

    This moves point to the start of `node` in `ewoc`.

<!---->

*   Function: **ewoc-refresh** *ewoc*

    This function regenerates the text of `ewoc`. It works by deleting the text between the header and the footer, i.e., all the data elements’ representations, and then calling the pretty-printer function for each node, one by one, in order.

<!---->

*   Function: **ewoc-invalidate** *ewoc \&rest nodes*

    This is similar to `ewoc-refresh`, except that only `nodes` in `ewoc` are updated instead of the entire set.

<!---->

*   Function: **ewoc-delete** *ewoc \&rest nodes*

    This deletes each node in `nodes` from `ewoc`.

<!---->

*   Function: **ewoc-filter** *ewoc predicate \&rest args*

    This calls `predicate` for each data element in `ewoc` and deletes those nodes for which `predicate` returns `nil`. Any `args` are passed to `predicate`.

<!---->

*   Function: **ewoc-collect** *ewoc predicate \&rest args*

    This calls `predicate` for each data element in `ewoc` and returns a list of those elements for which `predicate` returns non-`nil`. The elements in the list are ordered as in the buffer. Any `args` are passed to `predicate`.

<!---->

*   Function: **ewoc-map** *map-function ewoc \&rest args*

    This calls `map-function` for each data element in `ewoc` and updates those nodes for which `map-function` returns non-`nil`. Any `args` are passed to `map-function`.

Next: [Abstract Display Example](Abstract-Display-Example.html), Up: [Abstract Display](Abstract-Display.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
