

Next: [Translation Keymaps](Translation-Keymaps.html), Previous: [Changing Key Bindings](Changing-Key-Bindings.html), Up: [Keymaps](Keymaps.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 22.13 Remapping Commands

A special kind of key binding can be used to *remap* one command to another, without having to refer to the key sequence(s) bound to the original command. To use this feature, make a key binding for a key sequence that starts with the dummy event `remap`, followed by the command name you want to remap; for the binding, specify the new definition (usually a command name, but possibly any other valid definition for a key binding).

For example, suppose My mode provides a special command `my-kill-line`, which should be invoked instead of `kill-line`. To establish this, its mode keymap should contain the following remapping:

```lisp
(define-key my-mode-map [remap kill-line] 'my-kill-line)
```

Then, whenever `my-mode-map` is active, if the user types `C-k` (the default global key sequence for `kill-line`) Emacs will instead run `my-kill-line`.

Note that remapping only takes place through active keymaps; for example, putting a remapping in a prefix keymap like `ctl-x-map` typically has no effect, as such keymaps are not themselves active. In addition, remapping only works through a single level; in the following example,

```lisp
(define-key my-mode-map [remap kill-line] 'my-kill-line)
(define-key my-mode-map [remap my-kill-line] 'my-other-kill-line)
```

`kill-line` is *not* remapped to `my-other-kill-line`. Instead, if an ordinary key binding specifies `kill-line`, it is remapped to `my-kill-line`; if an ordinary binding specifies `my-kill-line`, it is remapped to `my-other-kill-line`.

To undo the remapping of a command, remap it to `nil`; e.g.,

```lisp
(define-key my-mode-map [remap kill-line] nil)
```

*   Function: **command-remapping** *command \&optional position keymaps*

    This function returns the remapping for `command` (a symbol), given the current active keymaps. If `command` is not remapped (which is the usual situation), or not a symbol, the function returns `nil`. `position` can optionally specify a buffer position or an event position to determine the keymaps to use, as in `key-binding`.

    If the optional argument `keymaps` is non-`nil`, it specifies a list of keymaps to search in. This argument is ignored if `position` is non-`nil`.

Next: [Translation Keymaps](Translation-Keymaps.html), Previous: [Changing Key Bindings](Changing-Key-Bindings.html), Up: [Keymaps](Keymaps.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
