

Next: [Maintaining Undo](Maintaining-Undo.html), Previous: [The Kill Ring](The-Kill-Ring.html), Up: [Text](Text.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 32.9 Undo

Most buffers have an *undo list*, which records all changes made to the buffer’s text so that they can be undone. (The buffers that don’t have one are usually special-purpose buffers for which Emacs assumes that undoing is not useful. In particular, any buffer whose name begins with a space has its undo recording off by default; see [Buffer Names](Buffer-Names.html).) All the primitives that modify the text in the buffer automatically add elements to the front of the undo list, which is in the variable `buffer-undo-list`.

*   Variable: **buffer-undo-list**

    This buffer-local variable’s value is the undo list of the current buffer. A value of `t` disables the recording of undo information.

Here are the kinds of elements an undo list can have:

*   `position`

    This kind of element records a previous value of point; undoing this element moves point to `position`. Ordinary cursor motion does not make any sort of undo record, but deletion operations use these entries to record where point was before the command.

*   `(beg . end)`

    This kind of element indicates how to delete text that was inserted. Upon insertion, the text occupied the range `beg`–`end` in the buffer.

*   `(text . position)`

    This kind of element indicates how to reinsert text that was deleted. The deleted text itself is the string `text`. The place to reinsert it is `(abs position)`. If `position` is positive, point was at the beginning of the deleted text, otherwise it was at the end. Zero or more (`marker` . `adjustment`) elements follow immediately after this element.

*   `(t . time-flag)`

    This kind of element indicates that an unmodified buffer became modified. A `time-flag` that is a non-integer Lisp timestamp represents the visited file’s modification time as of when it was previously visited or saved, using the same format as `current-time`; see [Time of Day](Time-of-Day.html). A `time-flag` of 0 means the buffer does not correspond to any file; -1 means the visited file previously did not exist. `primitive-undo` uses these values to determine whether to mark the buffer as unmodified once again; it does so only if the file’s status matches that of `time-flag`.

*   `(nil property value beg . end)`

    This kind of element records a change in a text property. Here’s how you might undo the change:

    ```lisp
    (put-text-property beg end property value)
    ```

*   `(marker . adjustment)`

    This kind of element records the fact that the marker `marker` was relocated due to deletion of surrounding text, and that it moved `adjustment` character positions. If the marker’s location is consistent with the (`text` . `position`) element preceding it in the undo list, then undoing this element moves `marker` - `adjustment` characters.

*   `(apply funname . args)`

    This is an extensible undo item, which is undone by calling `funname` with arguments `args`.

*   `(apply delta beg end funname . args)`

    This is an extensible undo item, which records a change limited to the range `beg` to `end`, which increased the size of the buffer by `delta` characters. It is undone by calling `funname` with arguments `args`.

    This kind of element enables undo limited to a region to determine whether the element pertains to that region.

*   `nil`

    This element is a boundary. The elements between two boundaries are called a *change group*; normally, each change group corresponds to one keyboard command, and undo commands normally undo an entire group as a unit.

<!---->

*   Function: **undo-boundary**

    This function places a boundary element in the undo list. The undo command stops at such a boundary, and successive undo commands undo to earlier and earlier boundaries. This function returns `nil`.

    Calling this function explicitly is useful for splitting the effects of a command into more than one unit. For example, `query-replace` calls `undo-boundary` after each replacement, so that the user can undo individual replacements one by one.

    Mostly, however, this function is called automatically at an appropriate time.

<!---->

*   Function: **undo-auto-amalgamate**

    The editor command loop automatically calls `undo-boundary` just before executing each key sequence, so that each undo normally undoes the effects of one command. A few exceptional commands are *amalgamating*: these commands generally cause small changes to buffers, so with these a boundary is inserted only every 20th command, allowing the changes to be undone as a group. By default, the commands `self-insert-command`, which produces self-inserting input characters (see [Commands for Insertion](Commands-for-Insertion.html)), and `delete-char`, which deletes characters (see [Deletion](Deletion.html)), are amalgamating. Where a command affects the contents of several buffers, as may happen, for example, when a function on the `post-command-hook` affects a buffer other than the `current-buffer`, then `undo-boundary` will be called in each of the affected buffers.

    This function can be called before an amalgamating command. It removes the previous `undo-boundary` if a series of such calls have been made.

    The maximum number of changes that can be amalgamated is controlled by the `amalgamating-undo-limit` variable. If this variable is 1, no changes are amalgamated.

A Lisp program can amalgamate a series of changes into a single change group by calling `undo-amalgamate-change-group` (see [Atomic Changes](Atomic-Changes.html)). Note that `amalgamating-undo-limit` has no effect on the groups produced by that function.

*   Variable: **undo-auto-current-boundary-timer**

    Some buffers, such as process buffers, can change even when no commands are executing. In these cases, `undo-boundary` is normally called periodically by the timer in this variable. Setting this variable to non-`nil` prevents this behavior.

<!---->

*   Variable: **undo-in-progress**

    This variable is normally `nil`, but the undo commands bind it to `t`. This is so that various kinds of change hooks can tell when they’re being called for the sake of undoing.

<!---->

*   Function: **primitive-undo** *count list*

    This is the basic function for undoing elements of an undo list. It undoes the first `count` elements of `list`, returning the rest of `list`.

    `primitive-undo` adds elements to the buffer’s undo list when it changes the buffer. Undo commands avoid confusion by saving the undo list value at the beginning of a sequence of undo operations. Then the undo operations use and update the saved value. The new elements added by undoing are not part of this saved value, so they don’t interfere with continuing to undo.

    This function does not bind `undo-in-progress`.

Some commands leave the region active after execution in such a way that it interferes with selective undo of that command. To make `undo` ignore the active region when invoked immediately after such a command, set the property `undo-inhibit-region` of the command’s function symbol to a non-nil value. See [Standard Properties](Standard-Properties.html).

Next: [Maintaining Undo](Maintaining-Undo.html), Previous: [The Kill Ring](The-Kill-Ring.html), Up: [Text](Text.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
