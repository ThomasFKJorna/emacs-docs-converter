

Next: [Buffer Gap](Buffer-Gap.html), Previous: [Indirect Buffers](Indirect-Buffers.html), Up: [Buffers](Buffers.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 27.12 Swapping Text Between Two Buffers

Specialized modes sometimes need to let the user access from the same buffer several vastly different types of text. For example, you may need to display a summary of the buffer text, in addition to letting the user access the text itself.

This could be implemented with multiple buffers (kept in sync when the user edits the text), or with narrowing (see [Narrowing](Narrowing.html)). But these alternatives might sometimes become tedious or prohibitively expensive, especially if each type of text requires expensive buffer-global operations in order to provide correct display and editing commands.

Emacs provides another facility for such modes: you can quickly swap buffer text between two buffers with `buffer-swap-text`. This function is very fast because it doesn’t move any text, it only changes the internal data structures of the buffer object to point to a different chunk of text. Using it, you can pretend that a group of two or more buffers are actually a single virtual buffer that holds the contents of all the individual buffers together.

*   Function: **buffer-swap-text** *buffer*

    This function swaps the text of the current buffer and that of its argument `buffer`. It signals an error if one of the two buffers is an indirect buffer (see [Indirect Buffers](Indirect-Buffers.html)) or is a base buffer of an indirect buffer.

    All the buffer properties that are related to the buffer text are swapped as well: the positions of point and mark, all the markers, the overlays, the text properties, the undo list, the value of the `enable-multibyte-characters` flag (see [enable-multibyte-characters](Text-Representations.html)), etc.

    **Warning:** If this function is called from within a `save-excursion` form, the current buffer will be set to `buffer` upon leaving the form, since the marker used by `save-excursion` to save the position and buffer will be swapped as well.

If you use `buffer-swap-text` on a file-visiting buffer, you should set up a hook to save the buffer’s original text rather than what it was swapped with. `write-region-annotate-functions` works for this purpose. You should probably set `buffer-saved-size` to -2 in the buffer, so that changes in the text it is swapped with will not interfere with auto-saving.

Next: [Buffer Gap](Buffer-Gap.html), Previous: [Indirect Buffers](Indirect-Buffers.html), Up: [Buffers](Buffers.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
