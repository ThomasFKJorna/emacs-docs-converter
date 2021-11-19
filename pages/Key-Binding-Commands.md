

Next: [Scanning Keymaps](Scanning-Keymaps.html), Previous: [Translation Keymaps](Translation-Keymaps.html), Up: [Keymaps](Keymaps.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 22.15 Commands for Binding Keys

This section describes some convenient interactive interfaces for changing key bindings. They work by calling `define-key`.

People often use `global-set-key` in their init files (see [Init File](Init-File.html)) for simple customization. For example,

```lisp
(global-set-key (kbd "C-x C-\\") 'next-line)
```

or

```lisp
(global-set-key [?\C-x ?\C-\\] 'next-line)
```

or

```lisp
(global-set-key [(control ?x) (control ?\\)] 'next-line)
```

redefines `C-x C-\` to move down a line.

```lisp
(global-set-key [M-mouse-1] 'mouse-set-point)
```

redefines the first (leftmost) mouse button, entered with the Meta key, to set point where you click.

Be careful when using non-ASCII text characters in Lisp specifications of keys to bind. If these are read as multibyte text, as they usually will be in a Lisp file (see [Loading Non-ASCII](Loading-Non_002dASCII.html)), you must type the keys as multibyte too. For instance, if you use this:

```lisp
(global-set-key "ö" 'my-function) ; bind o-umlaut
```

or

```lisp
(global-set-key ?ö 'my-function) ; bind o-umlaut
```

and your language environment is multibyte Latin-1, these commands actually bind the multibyte character with code 246, not the byte code 246 (`M-v`) sent by a Latin-1 terminal. In order to use this binding, you need to teach Emacs how to decode the keyboard by using an appropriate input method (see [Input Methods](https://www.gnu.org/software/emacs/manual/html_node/emacs/Input-Methods.html#Input-Methods) in The GNU Emacs Manual).

*   Command: **global-set-key** *key binding*

    This function sets the binding of `key` in the current global map to `binding`.

    ```lisp
    (global-set-key key binding)
    ≡
    (define-key (current-global-map) key binding)
    ```

<!---->

*   Command: **global-unset-key** *key*

    This function removes the binding of `key` from the current global map.

    One use of this function is in preparation for defining a longer key that uses `key` as a prefix—which would not be allowed if `key` has a non-prefix binding. For example:

    ```lisp
    (global-unset-key "\C-l")
        ⇒ nil
    ```

    ```lisp
    (global-set-key "\C-l\C-l" 'redraw-display)
        ⇒ nil
    ```

    This function is equivalent to using `define-key` as follows:

    ```lisp
    (global-unset-key key)
    ≡
    (define-key (current-global-map) key nil)
    ```

<!---->

*   Command: **local-set-key** *key binding*

    This function sets the binding of `key` in the current local keymap to `binding`.

    ```lisp
    (local-set-key key binding)
    ≡
    (define-key (current-local-map) key binding)
    ```

<!---->

*   Command: **local-unset-key** *key*

    This function removes the binding of `key` from the current local map.

    ```lisp
    (local-unset-key key)
    ≡
    (define-key (current-local-map) key nil)
    ```

Next: [Scanning Keymaps](Scanning-Keymaps.html), Previous: [Translation Keymaps](Translation-Keymaps.html), Up: [Keymaps](Keymaps.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
