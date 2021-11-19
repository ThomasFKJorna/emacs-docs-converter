

Next: [Creating Keymaps](Creating-Keymaps.html), Previous: [Keymap Basics](Keymap-Basics.html), Up: [Keymaps](Keymaps.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 22.3 Format of Keymaps

Each keymap is a list whose CAR is the symbol `keymap`. The remaining elements of the list define the key bindings of the keymap. A symbol whose function definition is a keymap is also a keymap. Use the function `keymapp` (see below) to test whether an object is a keymap.

Several kinds of elements may appear in a keymap, after the symbol `keymap` that begins it:

*   `(type . binding)`

    This specifies one binding, for events of type `type`. Each ordinary binding applies to events of a particular *event type*, which is always a character or a symbol. See [Classifying Events](Classifying-Events.html). In this kind of binding, `binding` is a command.

*   `(type item-name . binding)`

    This specifies a binding which is also a simple menu item that displays as `item-name` in the menu. See [Simple Menu Items](Simple-Menu-Items.html).

*   `(type item-name help-string . binding)`

    This is a simple menu item with help string `help-string`.

*   `(type menu-item . details)`

    This specifies a binding which is also an extended menu item. This allows use of other features. See [Extended Menu Items](Extended-Menu-Items.html).

*   `(t . binding)`

    This specifies a *default key binding*; any event not bound by other elements of the keymap is given `binding` as its binding. Default bindings allow a keymap to bind all possible event types without having to enumerate all of them. A keymap that has a default binding completely masks any lower-precedence keymap, except for events explicitly bound to `nil` (see below).

*   `char-table`

    If an element of a keymap is a char-table, it counts as holding bindings for all character events with no modifier bits (see [modifier bits](Other-Char-Bits.html#modifier-bits)): the element whose index is `c` is the binding for the character `c`. This is a compact way to record lots of bindings. A keymap with such a char-table is called a *full keymap*. Other keymaps are called *sparse keymaps*.

*   `vector`

    This kind of element is similar to a char-table: the element whose index is `c` is the binding for the character `c`. Since the range of characters that can be bound this way is limited by the vector size, and vector creation allocates space for all character codes from 0 up, this format should not be used except for creating menu keymaps (see [Menu Keymaps](Menu-Keymaps.html)), where the bindings themselves don’t matter.

*   `string`

    Aside from elements that specify bindings for keys, a keymap can also have a string as an element. This is called the *overall prompt string* and makes it possible to use the keymap as a menu. See [Defining Menus](Defining-Menus.html).

*   `(keymap …)`

    If an element of a keymap is itself a keymap, it counts as if this inner keymap were inlined in the outer keymap. This is used for multiple-inheritance, such as in `make-composed-keymap`.

When the binding is `nil`, it doesn’t constitute a definition but it does take precedence over a default binding or a binding in the parent keymap. On the other hand, a binding of `nil` does *not* override lower-precedence keymaps; thus, if the local map gives a binding of `nil`, Emacs uses the binding from the global map.

Keymaps do not directly record bindings for the meta characters. Instead, meta characters are regarded for purposes of key lookup as sequences of two characters, the first of which is `ESC` (or whatever is currently the value of `meta-prefix-char`). Thus, the key `M-a` is internally represented as `ESC a`, and its global binding is found at the slot for `a` in `esc-map` (see [Prefix Keys](Prefix-Keys.html)).

This conversion applies only to characters, not to function keys or other input events; thus, `M-end` has nothing to do with `ESC end`.

Here as an example is the local keymap for Lisp mode, a sparse keymap. It defines bindings for `DEL`, `C-c C-z`, `C-M-q`, and `C-M-x` (the actual value also contains a menu binding, which is omitted here for the sake of brevity).

```lisp
lisp-mode-map
⇒
```

```lisp
(keymap
 (3 keymap
    ;; C-c C-z
    (26 . run-lisp))
```

```lisp
 (27 keymap
     ;; C-M-x, treated as ESC C-x
     (24 . lisp-send-defun))
```

```lisp
 ;; This part is inherited from lisp-mode-shared-map.
 keymap
 ;; DEL
 (127 . backward-delete-char-untabify)
```

```lisp
 (27 keymap
     ;; C-M-q, treated as ESC C-q
     (17 . indent-sexp)))
```

*   Function: **keymapp** *object*

    This function returns `t` if `object` is a keymap, `nil` otherwise. More precisely, this function tests for a list whose CAR is `keymap`, or for a symbol whose function definition satisfies `keymapp`.

    ```lisp
    (keymapp '(keymap))
        ⇒ t
    ```

    ```lisp
    (fset 'foo '(keymap))
    (keymapp 'foo)
        ⇒ t
    ```

    ```lisp
    (keymapp (current-global-map))
        ⇒ t
    ```

Next: [Creating Keymaps](Creating-Keymaps.html), Previous: [Keymap Basics](Keymap-Basics.html), Up: [Keymaps](Keymaps.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
