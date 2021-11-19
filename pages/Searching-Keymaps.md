

Next: [Controlling Active Maps](Controlling-Active-Maps.html), Previous: [Active Keymaps](Active-Keymaps.html), Up: [Keymaps](Keymaps.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 22.8 Searching the Active Keymaps

Here is a pseudo-Lisp summary of how Emacs searches the active keymaps:

```lisp
(or (if overriding-terminal-local-map
        (find-in overriding-terminal-local-map))
    (if overriding-local-map
        (find-in overriding-local-map)
      (or (find-in (get-char-property (point) 'keymap))
          (find-in-any emulation-mode-map-alists)
          (find-in-any minor-mode-overriding-map-alist)
          (find-in-any minor-mode-map-alist)
          (if (get-text-property (point) 'local-map)
              (find-in (get-char-property (point) 'local-map))
            (find-in (current-local-map)))))
    (find-in (current-global-map)))
```

Here, `find-in` and `find-in-any` are pseudo functions that search in one keymap and in an alist of keymaps, respectively. Note that the `set-transient-map` function works by setting `overriding-terminal-local-map` (see [Controlling Active Maps](Controlling-Active-Maps.html)).

In the above pseudo-code, if a key sequence starts with a mouse event (see [Mouse Events](Mouse-Events.html)), that event’s position is used instead of point, and the event’s buffer is used instead of the current buffer. In particular, this affects how the `keymap` and `local-map` properties are looked up. If a mouse event occurs on a string embedded with a `display`, `before-string`, or `after-string` property (see [Special Properties](Special-Properties.html)), and the string has a non-`nil` `keymap` or `local-map` property, that overrides the corresponding property in the underlying buffer text (i.e., the property specified by the underlying text is ignored).

When a key binding is found in one of the active keymaps, and that binding is a command, the search is over—the command is executed. However, if the binding is a symbol with a value or a string, Emacs replaces the input key sequences with the variable’s value or the string, and restarts the search of the active keymaps. See [Key Lookup](Key-Lookup.html).

The command which is finally found might also be remapped. See [Remapping Commands](Remapping-Commands.html).

Next: [Controlling Active Maps](Controlling-Active-Maps.html), Previous: [Active Keymaps](Active-Keymaps.html), Up: [Keymaps](Keymaps.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
