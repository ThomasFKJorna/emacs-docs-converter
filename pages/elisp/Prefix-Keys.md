

### 22.6 Prefix Keys

A *prefix key* is a key sequence whose binding is a keymap. The keymap defines what to do with key sequences that extend the prefix key. For example, `C-x` is a prefix key, and it uses a keymap that is also stored in the variable `ctl-x-map`. This keymap defines bindings for key sequences starting with `C-x`.

Some of the standard Emacs prefix keys use keymaps that are also found in Lisp variables:

`esc-map` is the global keymap for the `ESC` prefix key. Thus, the global definitions of all meta characters are actually found here. This map is also the function definition of `ESC-prefix`.

`help-map` is the global keymap for the `C-h` prefix key.

`mode-specific-map` is the global keymap for the prefix key `C-c`. This map is actually global, not mode-specific, but its name provides useful information about `C-c` in the output of `C-h b` (`display-bindings`), since the main use of this prefix key is for mode-specific bindings.

`ctl-x-map` is the global keymap used for the `C-x` prefix key. This map is found via the function cell of the symbol `Control-X-prefix`.

`mule-keymap` is the global keymap used for the `C-x RET` prefix key.

`ctl-x-4-map` is the global keymap used for the `C-x 4` prefix key.

`ctl-x-5-map` is the global keymap used for the `C-x 5` prefix key.

`2C-mode-map` is the global keymap used for the `C-x 6` prefix key.

`tab-prefix-map` is the global keymap used for the `C-x t` prefix key.

`vc-prefix-map` is the global keymap used for the `C-x v` prefix key.

`goto-map` is the global keymap used for the `M-g` prefix key.

`search-map` is the global keymap used for the `M-s` prefix key.

`facemenu-keymap` is the global keymap used for the `M-o` prefix key.

The other Emacs prefix keys are `C-x @`, `C-x a i`, `C-x ESC` and `ESC ESC`. They use keymaps that have no special names.

The keymap binding of a prefix key is used for looking up the event that follows the prefix key. (It may instead be a symbol whose function definition is a keymap. The effect is the same, but the symbol serves as a name for the prefix key.) Thus, the binding of `C-x` is the symbol `Control-X-prefix`, whose function cell holds the keymap for `C-x` commands. (The same keymap is also the value of `ctl-x-map`.)

Prefix key definitions can appear in any active keymap. The definitions of `C-c`, `C-x`, `C-h` and `ESC` as prefix keys appear in the global map, so these prefix keys are always available. Major and minor modes can redefine a key as a prefix by putting a prefix key definition for it in the local map or the minor mode’s map. See [Active Keymaps](Active-Keymaps.html).

If a key is defined as a prefix in more than one active map, then its various definitions are in effect merged: the commands defined in the minor mode keymaps come first, followed by those in the local map’s prefix definition, and then by those from the global map.

In the following example, we make `C-p` a prefix key in the local keymap, in such a way that `C-p` is identical to `C-x`. Then the binding for `C-p C-f` is the function `find-file`, just like `C-x C-f`. The key sequence `C-p 6` is not found in any active keymap.

```lisp
(use-local-map (make-sparse-keymap))
    ⇒ nil
```

```lisp
(local-set-key "\C-p" ctl-x-map)
    ⇒ nil
```

```lisp
(key-binding "\C-p\C-f")
    ⇒ find-file
```

```lisp
```

```lisp
(key-binding "\C-p6")
    ⇒ nil
```

### Function: **define-prefix-command** *symbol \&optional mapvar prompt*

This function prepares `symbol` for use as a prefix key’s binding: it creates a sparse keymap and stores it as `symbol`’s function definition. Subsequently binding a key sequence to `symbol` will make that key sequence into a prefix key. The return value is `symbol`.

This function also sets `symbol` as a variable, with the keymap as its value. But if `mapvar` is non-`nil`, it sets `mapvar` as a variable instead.

If `prompt` is non-`nil`, that becomes the overall prompt string for the keymap. The prompt string should be given for menu keymaps (see [Defining Menus](Defining-Menus.html)).
