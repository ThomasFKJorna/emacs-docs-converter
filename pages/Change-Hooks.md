

Previous: [Atomic Changes](Atomic-Changes.html), Up: [Text](Text.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 32.32 Change Hooks

These hook variables let you arrange to take notice of changes in buffers (or in a particular buffer, if you make them buffer-local). See also [Special Properties](Special-Properties.html), for how to detect changes to specific parts of the text.

The functions you use in these hooks should save and restore the match data if they do anything that uses regular expressions; otherwise, they will interfere in bizarre ways with the editing operations that call them.

*   Variable: **before-change-functions**

    This variable holds a list of functions to call when Emacs is about to modify a buffer. Each function gets two arguments, the beginning and end of the region that is about to change, represented as integers. The buffer that is about to change is always the current buffer when the function is called.

<!---->

*   Variable: **after-change-functions**

    This variable holds a list of functions to call after Emacs modifies a buffer. Each function receives three arguments: the beginning and end of the region just changed, and the length of the text that existed before the change. All three arguments are integers. The buffer that has been changed is always the current buffer when the function is called.

    The length of the old text is the difference between the buffer positions before and after that text as it was before the change. As for the changed text, its length is simply the difference between the first two arguments.

Output of messages into the `*Messages*` buffer does not call these functions, and neither do certain internal buffer changes, such as changes in buffers created by Emacs internally for certain jobs, that should not be visible to Lisp programs.

The vast majority of buffer changing primitives will call `before-change-functions` and `after-change-functions` in balanced pairs, once for each change, where the arguments to these hooks exactly delimit the change being made. Yet, hook functions should not rely on this always being the case, because some complex primitives call `before-change-functions` once before making changes, and then call `after-change-functions` zero or more times, depending on how many individual changes the primitive is making. When that happens, the arguments to `before-change-functions` will enclose a region in which the individual changes are made, but won’t necessarily be the minimal such region, and the arguments to each successive call of `after-change-functions` will then delimit the part of text being changed exactly. In general, we advise using either the before- or the after-change hook, but not both.

*   Macro: **combine-after-change-calls** *body…*

    The macro executes `body` normally, but arranges to call the after-change functions just once for a series of several changes—if that seems safe.

    If a program makes several text changes in the same area of the buffer, using the macro `combine-after-change-calls` around that part of the program can make it run considerably faster when after-change hooks are in use. When the after-change hooks are ultimately called, the arguments specify a portion of the buffer including all of the changes made within the `combine-after-change-calls` body.

    **Warning:** You must not alter the values of `after-change-functions` within the body of a `combine-after-change-calls` form.

    **Warning:** If the changes you combine occur in widely scattered parts of the buffer, this will still work, but it is not advisable, because it may lead to inefficient behavior for some change hook functions.

<!---->

*   Macro: **combine-change-calls** *beg end body…*

    This executes `body` normally, except any buffer changes it makes do not trigger the calls to `before-change-functions` and `after-change-functions`. Instead there is a single call of each of these hooks for the region enclosed by `beg` and `end`, the parameters supplied to `after-change-functions` reflecting the changes made to the size of the region by `body`.

    The result of this macro is the result returned by `body`.

    This macro is useful when a function makes a possibly large number of repetitive changes to the buffer, and the change hooks would otherwise take a long time to run, were they to be run for each individual buffer modification. Emacs itself uses this macro, for example, in the commands `comment-region` and `uncomment-region`.

    **Warning:** You must not alter the values of `before-change-functions` or `after-change-function` within `body`.

    **Warning:** You must not make any buffer changes outside of the region specified by `beg` and `end`.

<!---->

*   Variable: **first-change-hook**

    This variable is a normal hook that is run whenever a buffer is changed that was previously in the unmodified state.

<!---->

*   Variable: **inhibit-modification-hooks**

    If this variable is non-`nil`, all of the change hooks are disabled; none of them run. This affects all the hook variables described above in this section, as well as the hooks attached to certain special text properties (see [Special Properties](Special-Properties.html)) and overlay properties (see [Overlay Properties](Overlay-Properties.html)).

    Also, this variable is bound to non-`nil` while running those same hook variables, so that by default modifying the buffer from a modification hook does not cause other modification hooks to be run. If you do want modification hooks to be run in a particular piece of code that is itself run from a modification hook, then rebind locally `inhibit-modification-hooks` to `nil`. However, doing this may cause recursive calls to the modification hooks, so be sure to prepare for that (for example, by binding some variable which tells your hook to do nothing).

    We recommend that you only bind this variable for modifications that do not result in lasting changes to buffer text contents (for example face changes or temporary modifications). If you need to delay change hooks during a series of changes (typically for performance reasons), use `combine-change-calls` or `combine-after-change-calls` instead.

Previous: [Atomic Changes](Atomic-Changes.html), Up: [Text](Text.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
