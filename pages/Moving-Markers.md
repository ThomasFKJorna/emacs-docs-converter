

Next: [The Mark](The-Mark.html), Previous: [Marker Insertion Types](Marker-Insertion-Types.html), Up: [Markers](Markers.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 31.6 Moving Marker Positions

This section describes how to change the position of an existing marker. When you do this, be sure you know whether the marker is used outside of your program, and, if so, what effects will result from moving it—otherwise, confusing things may happen in other parts of Emacs.

*   Function: **set-marker** *marker position \&optional buffer*

    This function moves `marker` to `position` in `buffer`. If `buffer` is not provided, it defaults to the current buffer.

    If `position` is `nil` or a marker that points nowhere, then `marker` is set to point nowhere.

    The value returned is `marker`.

    ```lisp
    (setq m (point-marker))
         ⇒ #<marker at 4714 in markers.texi>
    ```

    ```lisp
    (set-marker m 55)
         ⇒ #<marker at 55 in markers.texi>
    ```

    ```lisp
    (setq b (get-buffer "foo"))
         ⇒ #<buffer foo>
    ```

    ```lisp
    (set-marker m 0 b)
         ⇒ #<marker at 1 in foo>
    ```

<!---->

*   Function: **move-marker** *marker position \&optional buffer*

    This is another name for `set-marker`.
