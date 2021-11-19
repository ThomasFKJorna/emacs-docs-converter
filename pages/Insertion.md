

Next: [Commands for Insertion](Commands-for-Insertion.html), Previous: [Comparing Text](Comparing-Text.html), Up: [Text](Text.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 32.4 Inserting Text

*Insertion* means adding new text to a buffer. The inserted text goes at point—between the character before point and the character after point. Some insertion functions leave point before the inserted text, while other functions leave it after. We call the former insertion *after point* and the latter insertion *before point*.

Insertion moves markers located at positions after the insertion point, so that they stay with the surrounding text (see [Markers](Markers.html)). When a marker points at the place of insertion, insertion may or may not relocate the marker, depending on the marker’s insertion type (see [Marker Insertion Types](Marker-Insertion-Types.html)). Certain special functions such as `insert-before-markers` relocate all such markers to point after the inserted text, regardless of the markers’ insertion type.

Insertion functions signal an error if the current buffer is read-only (see [Read Only Buffers](Read-Only-Buffers.html)) or if they insert within read-only text (see [Special Properties](Special-Properties.html)).

These functions copy text characters from strings and buffers along with their properties. The inserted characters have exactly the same properties as the characters they were copied from. By contrast, characters specified as separate arguments, not part of a string or buffer, inherit their text properties from the neighboring text.

The insertion functions convert text from unibyte to multibyte in order to insert in a multibyte buffer, and vice versa—if the text comes from a string or from a buffer. However, they do not convert unibyte character codes 128 through 255 to multibyte characters, not even if the current buffer is a multibyte buffer. See [Converting Representations](Converting-Representations.html).

*   Function: **insert** *\&rest args*

    This function inserts the strings and/or characters `args` into the current buffer, at point, moving point forward. In other words, it inserts the text before point. An error is signaled unless all `args` are either strings or characters. The value is `nil`.

<!---->

*   Function: **insert-before-markers** *\&rest args*

    This function inserts the strings and/or characters `args` into the current buffer, at point, moving point forward. An error is signaled unless all `args` are either strings or characters. The value is `nil`.

    This function is unlike the other insertion functions in that it relocates markers initially pointing at the insertion point, to point after the inserted text. If an overlay begins at the insertion point, the inserted text falls outside the overlay; if a nonempty overlay ends at the insertion point, the inserted text falls inside that overlay.

<!---->

*   Command: **insert-char** *character \&optional count inherit*

    This command inserts `count` instances of `character` into the current buffer before point. The argument `count` must be an integer, and `character` must be a character.

    If called interactively, this command prompts for `character` using its Unicode name or its code point. See [Inserting Text](https://www.gnu.org/software/emacs/manual/html_node/emacs/Inserting-Text.html#Inserting-Text) in The GNU Emacs Manual.

    This function does not convert unibyte character codes 128 through 255 to multibyte characters, not even if the current buffer is a multibyte buffer. See [Converting Representations](Converting-Representations.html).

    If `inherit` is non-`nil`, the inserted characters inherit sticky text properties from the two characters before and after the insertion point. See [Sticky Properties](Sticky-Properties.html).

<!---->

*   Function: **insert-buffer-substring** *from-buffer-or-name \&optional start end*

    This function inserts a portion of buffer `from-buffer-or-name` into the current buffer before point. The text inserted is the region between `start` (inclusive) and `end` (exclusive). (These arguments default to the beginning and end of the accessible portion of that buffer.) This function returns `nil`.

    In this example, the form is executed with buffer ‘`bar`’ as the current buffer. We assume that buffer ‘`bar`’ is initially empty.

    ```lisp
    ---------- Buffer: foo ----------
    We hold these truths to be self-evident, that all
    ---------- Buffer: foo ----------
    ```

    ```lisp
    ```

    ```lisp
    (insert-buffer-substring "foo" 1 20)
         ⇒ nil

    ---------- Buffer: bar ----------
    We hold these truth∗
    ---------- Buffer: bar ----------
    ```

<!---->

*   Function: **insert-buffer-substring-no-properties** *from-buffer-or-name \&optional start end*

    This is like `insert-buffer-substring` except that it does not copy any text properties.

See [Sticky Properties](Sticky-Properties.html), for other insertion functions that inherit text properties from the nearby text in addition to inserting it. Whitespace inserted by indentation functions also inherits text properties.

Next: [Commands for Insertion](Commands-for-Insertion.html), Previous: [Comparing Text](Comparing-Text.html), Up: [Text](Text.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
