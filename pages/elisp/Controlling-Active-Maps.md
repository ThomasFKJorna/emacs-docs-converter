

### 22.9 Controlling the Active Keymaps

### Variable: **global-map**

This variable contains the default global keymap that maps Emacs keyboard input to commands. The global keymap is normally this keymap. The default global keymap is a full keymap that binds `self-insert-command` to all of the printing characters.

It is normal practice to change the bindings in the global keymap, but you should not assign this variable any value other than the keymap it starts out with.

### Function: **current-global-map**

This function returns the current global keymap. This is the same as the value of `global-map` unless you change one or the other. The return value is a reference, not a copy; if you use `define-key` or other functions on it you will alter global bindings.

```lisp
(current-global-map)
⇒ (keymap [set-mark-command beginning-of-line …
            delete-backward-char])
```

### Function: **current-local-map**

This function returns the current buffer’s local keymap, or `nil` if it has none. In the following example, the keymap for the `*scratch*` buffer (using Lisp Interaction mode) is a sparse keymap in which the entry for `ESC`, ASCII code 27, is another sparse keymap.

```lisp
(current-local-map)
⇒ (keymap
    (10 . eval-print-last-sexp)
    (9 . lisp-indent-line)
    (127 . backward-delete-char-untabify)
```

```lisp
    (27 keymap
        (24 . eval-defun)
        (17 . indent-sexp)))
```

`current-local-map` returns a reference to the local keymap, not a copy of it; if you use `define-key` or other functions on it you will alter local bindings.

### Function: **current-minor-mode-maps**

This function returns a list of the keymaps of currently enabled minor modes.

### Function: **use-global-map** *keymap*

This function makes `keymap` the new current global keymap. It returns `nil`.

It is very unusual to change the global keymap.

### Function: **use-local-map** *keymap*

This function makes `keymap` the new local keymap of the current buffer. If `keymap` is `nil`, then the buffer has no local keymap. `use-local-map` returns `nil`. Most major mode commands use this function.

### Variable: **minor-mode-map-alist**

This variable is an alist describing keymaps that may or may not be active according to the values of certain variables. Its elements look like this:

```lisp
(variable . keymap)
```

The keymap `keymap` is active whenever `variable` has a non-`nil` value. Typically `variable` is the variable that enables or disables a minor mode. See [Keymaps and Minor Modes](Keymaps-and-Minor-Modes.html).

Note that elements of `minor-mode-map-alist` do not have the same structure as elements of `minor-mode-alist`. The map must be the CDR of the element; a list with the map as the second element will not do. The CDR can be either a keymap (a list) or a symbol whose function definition is a keymap.

When more than one minor mode keymap is active, the earlier one in `minor-mode-map-alist` takes priority. But you should design minor modes so that they don’t interfere with each other. If you do this properly, the order will not matter.

See [Keymaps and Minor Modes](Keymaps-and-Minor-Modes.html), for more information about minor modes. See also `minor-mode-key-binding` (see [Functions for Key Lookup](Functions-for-Key-Lookup.html)).

### Variable: **minor-mode-overriding-map-alist**

This variable allows major modes to override the key bindings for particular minor modes. The elements of this alist look like the elements of `minor-mode-map-alist`: `(variable . keymap)`.

If a variable appears as an element of `minor-mode-overriding-map-alist`, the map specified by that element totally replaces any map specified for the same variable in `minor-mode-map-alist`.

`minor-mode-overriding-map-alist` is automatically buffer-local in all buffers.

### Variable: **overriding-local-map**

If non-`nil`, this variable holds a keymap to use instead of the buffer’s local keymap, any text property or overlay keymaps, and any minor mode keymaps. This keymap, if specified, overrides all other maps that would have been active, except for the current global map.

### Variable: **overriding-terminal-local-map**

If non-`nil`, this variable holds a keymap to use instead of `overriding-local-map`, the buffer’s local keymap, text property or overlay keymaps, and all the minor mode keymaps.

This variable is always local to the current terminal and cannot be buffer-local. See [Multiple Terminals](Multiple-Terminals.html). It is used to implement incremental search mode.

### Variable: **overriding-local-map-menu-flag**

If this variable is non-`nil`, the value of `overriding-local-map` or `overriding-terminal-local-map` can affect the display of the menu bar. The default value is `nil`, so those map variables have no effect on the menu bar.

Note that these two map variables do affect the execution of key sequences entered using the menu bar, even if they do not affect the menu bar display. So if a menu bar key sequence comes in, you should clear the variables before looking up and executing that key sequence. Modes that use the variables would typically do this anyway; normally they respond to events that they do not handle by “unreading” them and exiting.

### Variable: **special-event-map**

This variable holds a keymap for special events. If an event type has a binding in this keymap, then it is special, and the binding for the event is run directly by `read-event`. See [Special Events](Special-Events.html).

### Variable: **emulation-mode-map-alists**

This variable holds a list of keymap alists to use for emulation modes. It is intended for modes or packages using multiple minor-mode keymaps. Each element is a keymap alist which has the same format and meaning as `minor-mode-map-alist`, or a symbol with a variable binding which is such an alist. The active keymaps in each alist are used before `minor-mode-map-alist` and `minor-mode-overriding-map-alist`.

### Function: **set-transient-map** *keymap \&optional keep-pred on-exit*

This function adds `keymap` as a *transient* keymap, which takes precedence over other keymaps for one (or more) subsequent keys.

Normally, `keymap` is used just once, to look up the very next key. If the optional argument `keep-pred` is `t`, the map stays active as long as the user types keys defined in `keymap`; when the user types a key that is not in `keymap`, the transient keymap is deactivated and normal key lookup continues for that key.

The `keep-pred` argument can also be a function. In that case, the function is called with no arguments, prior to running each command, while `keymap` is active; it should return non-`nil` if `keymap` should stay active.

The optional argument `on-exit`, if non-`nil`, specifies a function that is called, with no arguments, after `keymap` is deactivated.

This function works by adding and removing `keymap` from the variable `overriding-terminal-local-map`, which takes precedence over all other active keymaps (see [Searching Keymaps](Searching-Keymaps.html)).
