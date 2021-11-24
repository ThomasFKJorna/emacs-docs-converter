

#### 28.13.1 Choosing a Window for Displaying a Buffer

The command `display-buffer` flexibly chooses a window for display, and displays a specified buffer in that window. It can be called interactively, via the key binding `C-x 4 C-o`. It is also used as a subroutine by many functions and commands, including `switch-to-buffer` and `pop-to-buffer` (see [Switching Buffers](Switching-Buffers.html)).

This command performs several complex steps to find a window to display in. These steps are described by means of *display actions*, which have the form `(functions . alist)`. Here, `functions` is either a single function or a list of functions, referred to as “action functions” (see [Buffer Display Action Functions](Buffer-Display-Action-Functions.html)); and `alist` is an association list, referred to as “action alist” (see [Buffer Display Action Alists](Buffer-Display-Action-Alists.html)). See [The Zen of Buffer Display](The-Zen-of-Buffer-Display.html), for samples of display actions.

An action function accepts two arguments: the buffer to display and an action alist. It attempts to display the buffer in some window, picking or creating a window according to its own criteria. If successful, it returns the window; otherwise, it returns `nil`.

`display-buffer` works by combining display actions from several sources, and calling the action functions in turn, until one of them manages to display the buffer and returns a non-`nil` value.

### Command: **display-buffer** *buffer-or-name \&optional action frame*

This command makes `buffer-or-name` appear in some window, without selecting the window or making the buffer current. The argument `buffer-or-name` must be a buffer or the name of an existing buffer. The return value is the window chosen to display the buffer, or `nil` if no suitable window was found.

The optional argument `action`, if non-`nil`, should normally be a display action (described above). `display-buffer` builds a list of action functions and an action alist, by consolidating display actions from the following sources (in order of their precedence, from highest to lowest):

*   The variable `display-buffer-overriding-action`.
*   The user option `display-buffer-alist`.
*   The `action` argument.
*   The user option `display-buffer-base-action`.
*   The constant `display-buffer-fallback-action`.

In practice this means that `display-buffer` builds a list of all action functions specified by these display actions. The first element of this list is the first action function specified by `display-buffer-overriding-action`, if any. Its last element is `display-buffer-pop-up-frame`—the last action function specified by `display-buffer-fallback-action`. Duplicates are not removed from this list—hence one and the same action function may be called multiple times during one call of `display-buffer`.

`display-buffer` calls the action functions specified by this list in turn, passing the buffer as the first argument and the combined action alist as the second argument, until one of the functions returns non-`nil`. See [Precedence of Action Functions](Precedence-of-Action-Functions.html), for examples how display actions specified by different sources are processed by `display-buffer`.

Note that the second argument is always the list of *all* action alist entries specified by the sources named above. Hence, the first element of that list is the first action alist entry specified by `display-buffer-overriding-action`, if any. Its last element is the last alist entry of `display-buffer-base-action`, if any (the action alist of `display-buffer-fallback-action` is empty).

Note also, that the combined action alist may contain duplicate entries and entries for the same key with different values. As a rule, action functions always use the first association of a key they find. Hence, the association an action function uses is not necessarily the association provided by the display action that specified that action function,

The argument `action` can also have a non-`nil`, non-list value. This has the special meaning that the buffer should be displayed in a window other than the selected one, even if the selected window is already displaying it. If called interactively with a prefix argument, `action` is `t`. Lisp programs should always supply a list value.

The optional argument `frame`, if non-`nil`, specifies which frames to check when deciding whether the buffer is already displayed. It is equivalent to adding an element `(reusable-frames . frame)` to the action alist of `action` (see [Buffer Display Action Alists](Buffer-Display-Action-Alists.html)). The `frame` argument is provided for compatibility reasons, Lisp programs should not use it.

### Variable: **display-buffer-overriding-action**

The value of this variable should be a display action, which is treated with the highest priority by `display-buffer`. The default value is an empty display action, i.e., `(nil . nil)`.

### User Option: **display-buffer-alist**

The value of this option is an alist mapping conditions to display actions. Each condition may be either a regular expression matching a buffer name or a function that takes two arguments: a buffer name and the `action` argument passed to `display-buffer`. If either the name of the buffer passed to `display-buffer` matches a regular expression in this alist, or the function specified by a condition returns non-`nil`, then `display-buffer` uses the corresponding display action to display the buffer.

### User Option: **display-buffer-base-action**

The value of this option should be a display action. This option can be used to define a standard display action for calls to `display-buffer`.

### Constant: **display-buffer-fallback-action**

This display action specifies the fallback behavior for `display-buffer` if no other display actions are given.
