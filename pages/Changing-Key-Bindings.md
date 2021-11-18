<!-- This is the GNU Emacs Lisp Reference Manual
corresponding to Emacs version 27.2.

Copyright (C) 1990-1996, 1998-2021 Free Software Foundation,
Inc.

Permission is granted to copy, distribute and/or modify this document
under the terms of the GNU Free Documentation License, Version 1.3 or
any later version published by the Free Software Foundation; with the
Invariant Sections being "GNU General Public License," with the
Front-Cover Texts being "A GNU Manual," and with the Back-Cover
Texts as in (a) below.  A copy of the license is included in the
section entitled "GNU Free Documentation License."

(a) The FSF's Back-Cover Text is: "You have the freedom to copy and
modify this GNU manual.  Buying copies from the FSF supports it in
developing GNU and promoting software freedom." -->

<!-- Created by GNU Texinfo 6.7, http://www.gnu.org/software/texinfo/ -->

Next: [Remapping Commands](Remapping-Commands.html), Previous: [Functions for Key Lookup](Functions-for-Key-Lookup.html), Up: [Keymaps](Keymaps.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 22.12 Changing Key Bindings

The way to rebind a key is to change its entry in a keymap. If you change a binding in the global keymap, the change is effective in all buffers (though it has no direct effect in buffers that shadow the global binding with a local one). If you change the current buffer’s local map, that usually affects all buffers using the same major mode. The `global-set-key` and `local-set-key` functions are convenient interfaces for these operations (see [Key Binding Commands](Key-Binding-Commands.html)). You can also use `define-key`, a more general function; then you must explicitly specify the map to change.

When choosing the key sequences for Lisp programs to rebind, please follow the Emacs conventions for use of various keys (see [Key Binding Conventions](Key-Binding-Conventions.html)).

In writing the key sequence to rebind, it is good to use the special escape sequences for control and meta characters (see [String Type](String-Type.html)). The syntax ‘`\C-`’ means that the following character is a control character and ‘`\M-`’ means that the following character is a meta character. Thus, the string `"\M-x"` is read as containing a single `M-x`, `"\C-f"` is read as containing a single `C-f`, and `"\M-\C-x"` and `"\C-\M-x"` are both read as containing a single `C-M-x`. You can also use this escape syntax in vectors, as well as others that aren’t allowed in strings; one example is ‘`[?\C-\H-x home]`’. See [Character Type](Character-Type.html).

The key definition and lookup functions accept an alternate syntax for event types in a key sequence that is a vector: you can use a list containing modifier names plus one base event (a character or function key name). For example, `(control ?a)` is equivalent to `?\C-a` and `(hyper control left)` is equivalent to `C-H-left`. One advantage of such lists is that the precise numeric codes for the modifier bits don’t appear in compiled files.

The functions below signal an error if `keymap` is not a keymap, or if `key` is not a string or vector representing a key sequence. You can use event types (symbols) as shorthand for events that are lists. The `kbd` function (see [Key Sequences](Key-Sequences.html)) is a convenient way to specify the key sequence.

*   Function: **define-key** *keymap key binding*

    This function sets the binding for `key` in `keymap`. (If `key` is more than one event long, the change is actually made in another keymap reached from `keymap`.) The argument `binding` can be any Lisp object, but only certain types are meaningful. (For a list of meaningful types, see [Key Lookup](Key-Lookup.html).) The value returned by `define-key` is `binding`.

    If `key` is `[t]`, this sets the default binding in `keymap`. When an event has no binding of its own, the Emacs command loop uses the keymap’s default binding, if there is one.

    Every prefix of `key` must be a prefix key (i.e., bound to a keymap) or undefined; otherwise an error is signaled. If some prefix of `key` is undefined, then `define-key` defines it as a prefix key so that the rest of `key` can be defined as specified.

    If there was previously no binding for `key` in `keymap`, the new binding is added at the beginning of `keymap`. The order of bindings in a keymap makes no difference for keyboard input, but it does matter for menu keymaps (see [Menu Keymaps](Menu-Keymaps.html)).

This example creates a sparse keymap and makes a number of bindings in it:

    (setq map (make-sparse-keymap))
        ⇒ (keymap)

<!---->

    (define-key map "\C-f" 'forward-char)
        ⇒ forward-char

<!---->

    map
        ⇒ (keymap (6 . forward-char))

```
```

    ;; Build sparse submap for C-x and bind f in that.
    (define-key map (kbd "C-x f") 'forward-word)
        ⇒ forward-word

<!---->

    map
    ⇒ (keymap
        (24 keymap                ; C-x
            (102 . forward-word)) ;      f
        (6 . forward-char))       ; C-f

```
```

    ;; Bind C-p to the ctl-x-map.
    (define-key map (kbd "C-p") ctl-x-map)
    ;; ctl-x-map
    ⇒ [nil … find-file … backward-kill-sentence]

```
```

    ;; Bind C-f to foo in the ctl-x-map.
    (define-key map (kbd "C-p C-f") 'foo)
    ⇒ 'foo

<!---->

    map
    ⇒ (keymap     ; Note foo in ctl-x-map.
        (16 keymap [nil … foo … backward-kill-sentence])
        (24 keymap
            (102 . forward-word))
        (6 . forward-char))

Note that storing a new binding for `C-p C-f` actually works by changing an entry in `ctl-x-map`, and this has the effect of changing the bindings of both `C-p C-f` and `C-x C-f` in the default global map.

The function `substitute-key-definition` scans a keymap for keys that have a certain binding and rebinds them with a different binding. Another feature which is cleaner and can often produce the same results is to remap one command into another (see [Remapping Commands](Remapping-Commands.html)).

*   Function: **substitute-key-definition** *olddef newdef keymap \&optional oldmap*

    This function replaces `olddef` with `newdef` for any keys in `keymap` that were bound to `olddef`. In other words, `olddef` is replaced with `newdef` wherever it appears. The function returns `nil`.

    For example, this redefines `C-x C-f`, if you do it in an Emacs with standard bindings:

        (substitute-key-definition
         'find-file 'find-file-read-only (current-global-map))

    If `oldmap` is non-`nil`, that changes the behavior of `substitute-key-definition`: the bindings in `oldmap` determine which keys to rebind. The rebindings still happen in `keymap`, not in `oldmap`. Thus, you can change one map under the control of the bindings in another. For example,

        (substitute-key-definition
          'delete-backward-char 'my-funny-delete
          my-map global-map)

    puts the special deletion command in `my-map` for whichever keys are globally bound to the standard deletion command.

    Here is an example showing a keymap before and after substitution:

        (setq map (list 'keymap
                        (cons ?1 olddef-1)
                        (cons ?2 olddef-2)
                        (cons ?3 olddef-1)))
        ⇒ (keymap (49 . olddef-1) (50 . olddef-2) (51 . olddef-1))

    ```
    ```

        (substitute-key-definition 'olddef-1 'newdef map)
        ⇒ nil

    <!---->

        map
        ⇒ (keymap (49 . newdef) (50 . olddef-2) (51 . newdef))

<!---->

*   Function: **suppress-keymap** *keymap \&optional nodigits*

    This function changes the contents of the full keymap `keymap` by remapping `self-insert-command` to the command `undefined` (see [Remapping Commands](Remapping-Commands.html)). This has the effect of undefining all printing characters, thus making ordinary insertion of text impossible. `suppress-keymap` returns `nil`.

    If `nodigits` is `nil`, then `suppress-keymap` defines digits to run `digit-argument`, and `-` to run `negative-argument`. Otherwise it makes them undefined like the rest of the printing characters.

    The `suppress-keymap` function does not make it impossible to modify a buffer, as it does not suppress commands such as `yank` and `quoted-insert`. To prevent any modification of a buffer, make it read-only (see [Read Only Buffers](Read-Only-Buffers.html)).

    Since this function modifies `keymap`, you would normally use it on a newly created keymap. Operating on an existing keymap that is used for some other purpose is likely to cause trouble; for example, suppressing `global-map` would make it impossible to use most of Emacs.

    This function can be used to initialize the local keymap of a major mode for which insertion of text is not desirable. But usually such a mode should be derived from `special-mode` (see [Basic Major Modes](Basic-Major-Modes.html)); then its keymap will automatically inherit from `special-mode-map`, which is already suppressed. Here is how `special-mode-map` is defined:

        (defvar special-mode-map
          (let ((map (make-sparse-keymap)))
            (suppress-keymap map)
            (define-key map "q" 'quit-window)
            …
            map))

Next: [Remapping Commands](Remapping-Commands.html), Previous: [Functions for Key Lookup](Functions-for-Key-Lookup.html), Up: [Keymaps](Keymaps.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
