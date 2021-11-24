

### 22.4 Creating Keymaps

Here we describe the functions for creating keymaps.

### Function: **make-sparse-keymap** *\&optional prompt*

This function creates and returns a new sparse keymap with no entries. (A sparse keymap is the kind of keymap you usually want.) The new keymap does not contain a char-table, unlike `make-keymap`, and does not bind any events.

```lisp
(make-sparse-keymap)
    ⇒ (keymap)
```

If you specify `prompt`, that becomes the overall prompt string for the keymap. You should specify this only for menu keymaps (see [Defining Menus](Defining-Menus.html)). A keymap with an overall prompt string will always present a mouse menu or a keyboard menu if it is active for looking up the next input event. Don’t specify an overall prompt string for the main map of a major or minor mode, because that would cause the command loop to present a keyboard menu every time.

### Function: **make-keymap** *\&optional prompt*

This function creates and returns a new full keymap. That keymap contains a char-table (see [Char-Tables](Char_002dTables.html)) with slots for all characters without modifiers. The new keymap initially binds all these characters to `nil`, and does not bind any other kind of event. The argument `prompt` specifies a prompt string, as in `make-sparse-keymap`.

```lisp
(make-keymap)
    ⇒ (keymap #^[nil nil keymap nil nil nil …])
```

A full keymap is more efficient than a sparse keymap when it holds lots of bindings; for just a few, the sparse keymap is better.

### Function: **copy-keymap** *keymap*

This function returns a copy of `keymap`. This is almost never needed. If you want a keymap that’s like another yet with a few changes, you should use map inheritance rather than copying. I.e., something like:

```lisp
(let ((map (make-sparse-keymap)))
  (set-keymap-parent map <theirmap>)
  (define-key map ...)
  ...)
```

When performing `copy-keymap`, any keymaps that appear directly as bindings in `keymap` are also copied recursively, and so on to any number of levels. However, recursive copying does not take place when the definition of a character is a symbol whose function definition is a keymap; the same symbol appears in the new copy.

```lisp
(setq map (copy-keymap (current-local-map)))
⇒ (keymap
```

```lisp
     ;; (This implements meta characters.)
     (27 keymap
         (83 . center-paragraph)
         (115 . center-line))
     (9 . tab-to-tab-stop))
```

```lisp
```

```lisp
(eq map (current-local-map))
    ⇒ nil
```

```lisp
(equal map (current-local-map))
    ⇒ t
```
