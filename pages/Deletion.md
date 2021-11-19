

Next: [User-Level Deletion](User_002dLevel-Deletion.html), Previous: [Commands for Insertion](Commands-for-Insertion.html), Up: [Text](Text.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 32.6 Deleting Text

Deletion means removing part of the text in a buffer, without saving it in the kill ring (see [The Kill Ring](The-Kill-Ring.html)). Deleted text can’t be yanked, but can be reinserted using the undo mechanism (see [Undo](Undo.html)). Some deletion functions do save text in the kill ring in some special cases.

All of the deletion functions operate on the current buffer.

*   Command: **erase-buffer**

    This function deletes the entire text of the current buffer (*not* just the accessible portion), leaving it empty. If the buffer is read-only, it signals a `buffer-read-only` error; if some of the text in it is read-only, it signals a `text-read-only` error. Otherwise, it deletes the text without asking for any confirmation. It returns `nil`.

    Normally, deleting a large amount of text from a buffer inhibits further auto-saving of that buffer because it has shrunk. However, `erase-buffer` does not do this, the idea being that the future text is not really related to the former text, and its size should not be compared with that of the former text.

<!---->

*   Command: **delete-region** *start end*

    This command deletes the text between positions `start` and `end` in the current buffer, and returns `nil`. If point was inside the deleted region, its value afterward is `start`. Otherwise, point relocates with the surrounding text, as markers do.

<!---->

*   Function: **delete-and-extract-region** *start end*

    This function deletes the text between positions `start` and `end` in the current buffer, and returns a string containing the text just deleted.

    If point was inside the deleted region, its value afterward is `start`. Otherwise, point relocates with the surrounding text, as markers do.

<!---->

*   Command: **delete-char** *count \&optional killp*

    This command deletes `count` characters directly after point, or before point if `count` is negative. If `killp` is non-`nil`, then it saves the deleted characters in the kill ring.

    In an interactive call, `count` is the numeric prefix argument, and `killp` is the unprocessed prefix argument. Therefore, if a prefix argument is supplied, the text is saved in the kill ring. If no prefix argument is supplied, then one character is deleted, but not saved in the kill ring.

    The value returned is always `nil`.

<!---->

*   Command: **delete-backward-char** *count \&optional killp*

    This command deletes `count` characters directly before point, or after point if `count` is negative. If `killp` is non-`nil`, then it saves the deleted characters in the kill ring.

    In an interactive call, `count` is the numeric prefix argument, and `killp` is the unprocessed prefix argument. Therefore, if a prefix argument is supplied, the text is saved in the kill ring. If no prefix argument is supplied, then one character is deleted, but not saved in the kill ring.

    The value returned is always `nil`.

<!---->

*   Command: **backward-delete-char-untabify** *count \&optional killp*

    This command deletes `count` characters backward, changing tabs into spaces. When the next character to be deleted is a tab, it is first replaced with the proper number of spaces to preserve alignment and then one of those spaces is deleted instead of the tab. If `killp` is non-`nil`, then the command saves the deleted characters in the kill ring.

    Conversion of tabs to spaces happens only if `count` is positive. If it is negative, exactly -`count` characters after point are deleted.

    In an interactive call, `count` is the numeric prefix argument, and `killp` is the unprocessed prefix argument. Therefore, if a prefix argument is supplied, the text is saved in the kill ring. If no prefix argument is supplied, then one character is deleted, but not saved in the kill ring.

    The value returned is always `nil`.

<!---->

*   User Option: **backward-delete-char-untabify-method**

    This option specifies how `backward-delete-char-untabify` should deal with whitespace. Possible values include `untabify`, the default, meaning convert a tab to many spaces and delete one; `hungry`, meaning delete all tabs and spaces before point with one command; `all` meaning delete all tabs, spaces and newlines before point, and `nil`, meaning do nothing special for whitespace characters.

Next: [User-Level Deletion](User_002dLevel-Deletion.html), Previous: [Commands for Insertion](Commands-for-Insertion.html), Up: [Text](Text.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
