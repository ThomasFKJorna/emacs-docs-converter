

### 28.8 Recombining Windows

When deleting the last sibling of a window `W`, its parent window is deleted too, with `W` replacing it in the window tree. This means that `W` must be recombined with its parent’s siblings to form a new window combination (see [Windows and Frames](Windows-and-Frames.html)). In some occasions, deleting a live window may even entail the deletion of two internal windows.

```lisp
     ______________________________________
    | ______  ____________________________ |
    ||      || __________________________ ||
    ||      ||| ___________  ___________ |||
    ||      ||||           ||           ||||
    ||      ||||____W6_____||_____W7____||||
    ||      |||____________W4____________|||
    ||      || __________________________ ||
    ||      |||                          |||
    ||      |||                          |||
    ||      |||____________W5____________|||
    ||__W2__||_____________W3_____________ |
    |__________________W1__________________|
```

Deleting `W5` in this configuration normally causes the deletion of `W3` and `W4`. The remaining live windows `W2`, `W6` and `W7` are recombined to form a new horizontal combination with parent `W1`.

Sometimes, however, it makes sense to not delete a parent window like `W4`. In particular, a parent window should not be removed when it was used to preserve a combination embedded in a combination of the same type. Such embeddings make sense to assure that when you split a window and subsequently delete the new window, Emacs reestablishes the layout of the associated frame as it existed before the splitting.

Consider a scenario starting with two live windows `W2` and `W3` and their parent `W1`.

```lisp
     ______________________________________
    | ____________________________________ |
    ||                                    ||
    ||                                    ||
    ||                                    ||
    ||                                    ||
    ||                                    ||
    ||                                    ||
    ||_________________W2_________________||
    | ____________________________________ |
    ||                                    ||
    ||                                    ||
    ||_________________W3_________________||
    |__________________W1__________________|
```

Split `W2` to make a new window `W4` as follows.

```lisp
     ______________________________________
    | ____________________________________ |
    ||                                    ||
    ||                                    ||
    ||_________________W2_________________||
    | ____________________________________ |
    ||                                    ||
    ||                                    ||
    ||_________________W4_________________||
    | ____________________________________ |
    ||                                    ||
    ||                                    ||
    ||_________________W3_________________||
    |__________________W1__________________|
```

Now, when enlarging a window vertically, Emacs tries to obtain the corresponding space from its lower sibling, provided such a window exists. In our scenario, enlarging `W4` will steal space from `W3`.

```lisp
     ______________________________________
    | ____________________________________ |
    ||                                    ||
    ||                                    ||
    ||_________________W2_________________||
    | ____________________________________ |
    ||                                    ||
    ||                                    ||
    ||                                    ||
    ||                                    ||
    ||_________________W4_________________||
    | ____________________________________ |
    ||_________________W3_________________||
    |__________________W1__________________|
```

Deleting `W4` will now give its entire space to `W2`, including the space earlier stolen from `W3`.

```lisp
     ______________________________________
    | ____________________________________ |
    ||                                    ||
    ||                                    ||
    ||                                    ||
    ||                                    ||
    ||                                    ||
    ||                                    ||
    ||                                    ||
    ||                                    ||
    ||_________________W2_________________||
    | ____________________________________ |
    ||_________________W3_________________||
    |__________________W1__________________|
```

This can be counterintuitive, in particular if `W4` were used for displaying a buffer only temporarily (see [Temporary Displays](Temporary-Displays.html)), and you want to continue working with the initial layout.

The behavior can be fixed by making a new parent window when splitting `W2`. The variable described next allows that to be done.

### User Option: **window-combination-limit**

This variable controls whether splitting a window shall make a new parent window. The following values are recognized:

*   `nil`

    This means that the new live window is allowed to share the existing parent window, if one exists, provided the split occurs in the same direction as the existing window combination (otherwise, a new internal window is created anyway).

*   `window-size`

    This means that `display-buffer` makes a new parent window when it splits a window and is passed a `window-height` or `window-width` entry in the `alist` argument (see [Buffer Display Action Functions](Buffer-Display-Action-Functions.html)). Otherwise, window splitting behaves as for a value of `nil`.

*   `temp-buffer-resize`

    In this case `with-temp-buffer-window` makes a new parent window when it splits a window and `temp-buffer-resize-mode` is enabled (see [Temporary Displays](Temporary-Displays.html)). Otherwise, window splitting behaves as for `nil`.

*   `temp-buffer`

    In this case `with-temp-buffer-window` always makes a new parent window when it splits an existing window (see [Temporary Displays](Temporary-Displays.html)). Otherwise, window splitting behaves as for `nil`.

*   `display-buffer`

    This means that when `display-buffer` (see [Choosing Window](Choosing-Window.html)) splits a window it always makes a new parent window. Otherwise, window splitting behaves as for `nil`.

*   `t`

    This means that splitting a window always creates a new parent window. Thus, if the value of this variable is at all times `t`, then at all times every window tree is a binary tree (a tree where each window except the root window has exactly one sibling).

The default is `window-size`. Other values are reserved for future use.

If, as a consequence of this variable’s setting, `split-window` makes a new parent window, it also calls `set-window-combination-limit` (see below) on the newly-created internal window. This affects how the window tree is rearranged when the child windows are deleted (see below).

If `window-combination-limit` is `t`, splitting `W2` in the initial configuration of our scenario would have produced this:

```lisp
     ______________________________________
    | ____________________________________ |
    || __________________________________ ||
    |||                                  |||
    |||________________W2________________|||
    || __________________________________ ||
    |||                                  |||
    |||________________W4________________|||
    ||_________________W5_________________||
    | ____________________________________ |
    ||                                    ||
    ||                                    ||
    ||_________________W3_________________||
    |__________________W1__________________|
```

A new internal window `W5` has been created; its children are `W2` and the new live window `W4`. Now, `W2` is the only sibling of `W4`, so enlarging `W4` will try to shrink `W2`, leaving `W3` unaffected. Observe that `W5` represents a vertical combination of two windows embedded in the vertical combination `W1`.

### Function: **set-window-combination-limit** *window limit*

This function sets the *combination limit* of the window `window` to `limit`. This value can be retrieved via the function `window-combination-limit`. See below for its effects; note that it is only meaningful for internal windows. The `split-window` function automatically calls this function, passing it `t` as `limit`, provided the value of the variable `window-combination-limit` is `t` when it is called.

### Function: **window-combination-limit** *window*

This function returns the combination limit for `window`.

The combination limit is meaningful only for an internal window. If it is `nil`, then Emacs is allowed to automatically delete `window`, in response to a window deletion, in order to group the child windows of `window` with its sibling windows to form a new window combination. If the combination limit is `t`, the child windows of `window` are never automatically recombined with its siblings.

If, in the configuration shown at the beginning of this section, the combination limit of `W4` (the parent window of `W6` and `W7`) is `t`, deleting `W5` will not implicitly delete `W4` too.

Alternatively, the problems sketched above can be avoided by always resizing all windows in the same combination whenever one of its windows is split or deleted. This also permits splitting windows that would be otherwise too small for such an operation.

### User Option: **window-combination-resize**

If this variable is `nil`, `split-window` can only split a window (denoted by `window`) if `window`’s screen area is large enough to accommodate both itself and the new window.

If this variable is `t`, `split-window` tries to resize all windows that are part of the same combination as `window`, in order to accommodate the new window. In particular, this may allow `split-window` to succeed even if `window` is a fixed-size window or too small to ordinarily split. Furthermore, subsequently resizing or deleting `window` may resize all other windows in its combination.

The default is `nil`. Other values are reserved for future use. A specific split operation may ignore the value of this variable if it is affected by a non-`nil` value of `window-combination-limit`.

To illustrate the effect of `window-combination-resize`, consider the following frame layout.

```lisp
     ______________________________________
    | ____________________________________ |
    ||                                    ||
    ||                                    ||
    ||                                    ||
    ||                                    ||
    ||_________________W2_________________||
    | ____________________________________ |
    ||                                    ||
    ||                                    ||
    ||                                    ||
    ||                                    ||
    ||_________________W3_________________||
    |__________________W1__________________|
```

If `window-combination-resize` is `nil`, splitting window `W3` leaves the size of `W2` unchanged:

```lisp
     ______________________________________
    | ____________________________________ |
    ||                                    ||
    ||                                    ||
    ||                                    ||
    ||                                    ||
    ||_________________W2_________________||
    | ____________________________________ |
    ||                                    ||
    ||_________________W3_________________||
    | ____________________________________ |
    ||                                    ||
    ||_________________W4_________________||
    |__________________W1__________________|
```

If `window-combination-resize` is `t`, splitting `W3` instead leaves all three live windows with approximately the same height:

```lisp
     ______________________________________
    | ____________________________________ |
    ||                                    ||
    ||                                    ||
    ||_________________W2_________________||
    | ____________________________________ |
    ||                                    ||
    ||                                    ||
    ||_________________W3_________________||
    | ____________________________________ |
    ||                                    ||
    ||                                    ||
    ||_________________W4_________________||
    |__________________W1__________________|
```

Deleting any of the live windows `W2`, `W3` or `W4` will distribute its space proportionally among the two remaining live windows.
