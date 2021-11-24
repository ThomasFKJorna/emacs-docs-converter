

### 22.11 Functions for Key Lookup

Here are the functions and variables pertaining to key lookup.

### Function: **lookup-key** *keymap key \&optional accept-defaults*

This function returns the definition of `key` in `keymap`. All the other functions described in this chapter that look up keys use `lookup-key`. Here are examples:

```lisp
(lookup-key (current-global-map) "\C-x\C-f")
    ⇒ find-file
```

```lisp
(lookup-key (current-global-map) (kbd "C-x C-f"))
    ⇒ find-file
```

```lisp
(lookup-key (current-global-map) "\C-x\C-f12345")
    ⇒ 2
```

If the string or vector `key` is not a valid key sequence according to the prefix keys specified in `keymap`, it must be too long and have extra events at the end that do not fit into a single key sequence. Then the value is a number, the number of events at the front of `key` that compose a complete key.

If `accept-defaults` is non-`nil`, then `lookup-key` considers default bindings as well as bindings for the specific events in `key`. Otherwise, `lookup-key` reports only bindings for the specific sequence `key`, ignoring default bindings except when you explicitly ask about them. (To do this, supply `t` as an element of `key`; see [Format of Keymaps](Format-of-Keymaps.html).)

If `key` contains a meta character (not a function key), that character is implicitly replaced by a two-character sequence: the value of `meta-prefix-char`, followed by the corresponding non-meta character. Thus, the first example below is handled by conversion into the second example.

```lisp
(lookup-key (current-global-map) "\M-f")
    ⇒ forward-word
```

```lisp
(lookup-key (current-global-map) "\ef")
    ⇒ forward-word
```

The `keymap` argument can also be a list of keymaps.

Unlike `read-key-sequence`, this function does not modify the specified events in ways that discard information (see [Key Sequence Input](Key-Sequence-Input.html)). In particular, it does not convert letters to lower case and it does not change drag events to clicks.

### Command: **undefined**

Used in keymaps to undefine keys. It calls `ding`, but does not cause an error.

### Function: **local-key-binding** *key \&optional accept-defaults*

This function returns the binding for `key` in the current local keymap, or `nil` if it is undefined there.

The argument `accept-defaults` controls checking for default bindings, as in `lookup-key` (above).

### Function: **global-key-binding** *key \&optional accept-defaults*

This function returns the binding for command `key` in the current global keymap, or `nil` if it is undefined there.

The argument `accept-defaults` controls checking for default bindings, as in `lookup-key` (above).

### Function: **minor-mode-key-binding** *key \&optional accept-defaults*

This function returns a list of all the active minor mode bindings of `key`. More precisely, it returns an alist of pairs `(modename . binding)`, where `modename` is the variable that enables the minor mode, and `binding` is `key`’s binding in that mode. If `key` has no minor-mode bindings, the value is `nil`.

If the first binding found is not a prefix definition (a keymap or a symbol defined as a keymap), all subsequent bindings from other minor modes are omitted, since they would be completely shadowed. Similarly, the list omits non-prefix bindings that follow prefix bindings.

The argument `accept-defaults` controls checking for default bindings, as in `lookup-key` (above).

### User Option: **meta-prefix-char**

This variable is the meta-prefix character code. It is used for translating a meta character to a two-character sequence so it can be looked up in a keymap. For useful results, the value should be a prefix event (see [Prefix Keys](Prefix-Keys.html)). The default value is 27, which is the ASCII code for `ESC`.

As long as the value of `meta-prefix-char` remains 27, key lookup translates `M-b` into `ESC b`, which is normally defined as the `backward-word` command. However, if you were to set `meta-prefix-char` to 24, the code for `C-x`, then Emacs will translate `M-b` into `C-x b`, whose standard binding is the `switch-to-buffer` command. (Don’t actually do this!) Here is an illustration of what would happen:

```lisp
meta-prefix-char                    ; The default value.
     ⇒ 27
```

```lisp
(key-binding "\M-b")
     ⇒ backward-word
```

```lisp
?\C-x                               ; The print representation
     ⇒ 24                          ;   of a character.
```

```lisp
(setq meta-prefix-char 24)
     ⇒ 24
```

```lisp
(key-binding "\M-b")
     ⇒ switch-to-buffer            ; Now, typing M-b is
                                    ;   like typing C-x b.

(setq meta-prefix-char 27)          ; Avoid confusion!
     ⇒ 27                          ; Restore the default value!
```

This translation of one event into two happens only for characters, not for other kinds of input events. Thus, `M-F1`, a function key, is not converted into `ESC F1`.
