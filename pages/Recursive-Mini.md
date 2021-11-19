

Next: [Minibuffer Misc](Minibuffer-Misc.html), Previous: [Minibuffer Contents](Minibuffer-Contents.html), Up: [Minibuffers](Minibuffers.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 20.13 Recursive Minibuffers

These functions and variables deal with recursive minibuffers (see [Recursive Editing](Recursive-Editing.html)):

*   Function: **minibuffer-depth**

    This function returns the current depth of activations of the minibuffer, a nonnegative integer. If no minibuffers are active, it returns zero.

<!---->

*   User Option: **enable-recursive-minibuffers**

    If this variable is non-`nil`, you can invoke commands (such as `find-file`) that use minibuffers even while the minibuffer is active. Such invocation produces a recursive editing level for a new minibuffer. The outer-level minibuffer is invisible while you are editing the inner one.

    If this variable is `nil`, you cannot invoke minibuffer commands when the minibuffer is active, not even if you switch to another window to do it.

If a command name has a property `enable-recursive-minibuffers` that is non-`nil`, then the command can use the minibuffer to read arguments even if it is invoked from the minibuffer. A command can also achieve this by binding `enable-recursive-minibuffers` to `t` in the interactive declaration (see [Using Interactive](Using-Interactive.html)). The minibuffer command `next-matching-history-element` (normally `M-s` in the minibuffer) does the latter.
