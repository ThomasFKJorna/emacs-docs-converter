

Next: [Easy Menu](Easy-Menu.html), Previous: [Tool Bar](Tool-Bar.html), Up: [Menu Keymaps](Menu-Keymaps.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 22.17.7 Modifying Menus

When you insert a new item in an existing menu, you probably want to put it in a particular place among the menu’s existing items. If you use `define-key` to add the item, it normally goes at the front of the menu. To put it elsewhere in the menu, use `define-key-after`:

*   Function: **define-key-after** *map key binding \&optional after*

    Define a binding in `map` for `key`, with value `binding`, just like `define-key`, but position the binding in `map` after the binding for the event `after`. The argument `key` should be of length one—a vector or string with just one element. But `after` should be a single event type—a symbol or a character, not a sequence. The new binding goes after the binding for `after`. If `after` is `t` or is omitted, then the new binding goes last, at the end of the keymap. However, new bindings are added before any inherited keymap.

    Here is an example:

    ```lisp
    (define-key-after my-menu [drink]
      '("Drink" . drink-command) 'eat)
    ```

    makes a binding for the fake function key `DRINK` and puts it right after the binding for `EAT`.

    Here is how to insert an item called ‘`Work`’ in the ‘`Signals`’ menu of Shell mode, after the item `break`:

    ```lisp
    (define-key-after
      (lookup-key shell-mode-map [menu-bar signals])
      [work] '("Work" . work-command) 'break)
    ```
