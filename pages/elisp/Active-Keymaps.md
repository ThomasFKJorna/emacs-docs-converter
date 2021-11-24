

### 22.7 Active Keymaps

Emacs contains many keymaps, but at any time only a few keymaps are *active*. When Emacs receives user input, it translates the input event (see [Translation Keymaps](Translation-Keymaps.html)), and looks for a key binding in the active keymaps.

Usually, the active keymaps are: (i) the keymap specified by the `keymap` property, (ii) the keymaps of enabled minor modes, (iii) the current buffer’s local keymap, and (iv) the global keymap, in that order. Emacs searches for each input key sequence in all these keymaps.

Of these usual keymaps, the highest-precedence one is specified by the `keymap` text or overlay property at point, if any. (For a mouse input event, Emacs uses the event position instead of point; see [Searching Keymaps](Searching-Keymaps.html).)

Next in precedence are keymaps specified by enabled minor modes. These keymaps, if any, are specified by the variables `emulation-mode-map-alists`, `minor-mode-overriding-map-alist`, and `minor-mode-map-alist`. See [Controlling Active Maps](Controlling-Active-Maps.html).

Next in precedence is the buffer’s *local keymap*, containing key bindings specific to the buffer. The minibuffer also has a local keymap (see [Intro to Minibuffers](Intro-to-Minibuffers.html)). If there is a `local-map` text or overlay property at point, that specifies the local keymap to use, in place of the buffer’s default local keymap.

The local keymap is normally set by the buffer’s major mode, and every buffer with the same major mode shares the same local keymap. Hence, if you call `local-set-key` (see [Key Binding Commands](Key-Binding-Commands.html)) to change the local keymap in one buffer, that also affects the local keymaps in other buffers with the same major mode.

Finally, the *global keymap* contains key bindings that are defined regardless of the current buffer, such as `C-f`. It is always active, and is bound to the variable `global-map`.

Apart from the above usual keymaps, Emacs provides special ways for programs to make other keymaps active. Firstly, the variable `overriding-local-map` specifies a keymap that replaces the usual active keymaps, except for the global keymap. Secondly, the terminal-local variable `overriding-terminal-local-map` specifies a keymap that takes precedence over *all* other keymaps (including `overriding-local-map`); this is normally used for modal/transient keybindings (the function `set-transient-map` provides a convenient interface for this). See [Controlling Active Maps](Controlling-Active-Maps.html), for details.

Making keymaps active is not the only way to use them. Keymaps are also used in other ways, such as for translating events within `read-key-sequence`. See [Translation Keymaps](Translation-Keymaps.html).

See [Standard Keymaps](Standard-Keymaps.html), for a list of some standard keymaps.

### Function: **current-active-maps** *\&optional olp position*

This returns the list of active keymaps that would be used by the command loop in the current circumstances to look up a key sequence. Normally it ignores `overriding-local-map` and `overriding-terminal-local-map`, but if `olp` is non-`nil` then it pays attention to them. `position` can optionally be either an event position as returned by `event-start` or a buffer position, and may change the keymaps as described for `key-binding`.

### Function: **key-binding** *key \&optional accept-defaults no-remap position*

This function returns the binding for `key` according to the current active keymaps. The result is `nil` if `key` is undefined in the keymaps.

The argument `accept-defaults` controls checking for default bindings, as in `lookup-key` (see [Functions for Key Lookup](Functions-for-Key-Lookup.html)).

When commands are remapped (see [Remapping Commands](Remapping-Commands.html)), `key-binding` normally processes command remappings so as to return the remapped command that will actually be executed. However, if `no-remap` is non-`nil`, `key-binding` ignores remappings and returns the binding directly specified for `key`.

If `key` starts with a mouse event (perhaps following a prefix event), the maps to be consulted are determined based on the event’s position. Otherwise, they are determined based on the value of point. However, you can override either of them by specifying `position`. If `position` is non-`nil`, it should be either a buffer position or an event position like the value of `event-start`. Then the maps consulted are determined based on `position`.

Emacs signals an error if `key` is not a string or a vector.

```lisp
(key-binding "\C-x\C-f")
    ⇒ find-file
```
