

Next: [Keymap Basics](Keymap-Basics.html), Up: [Keymaps](Keymaps.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 22.1 Key Sequences

A *key sequence*, or *key* for short, is a sequence of one or more input events that form a unit. Input events include characters, function keys, mouse actions, or system events external to Emacs, such as `iconify-frame` (see [Input Events](Input-Events.html)). The Emacs Lisp representation for a key sequence is a string or vector. Unless otherwise stated, any Emacs Lisp function that accepts a key sequence as an argument can handle both representations.

In the string representation, alphanumeric characters ordinarily stand for themselves; for example, `"a"` represents `a` and `"2"` represents `2`. Control character events are prefixed by the substring `"\C-"`, and meta characters by `"\M-"`; for example, `"\C-x"` represents the key `C-x`. In addition, the `TAB`, `RET`, `ESC`, and `DEL` events are represented by `"\t"`, `"\r"`, `"\e"`, and `"\d"` respectively. The string representation of a complete key sequence is the concatenation of the string representations of the constituent events; thus, `"\C-xl"` represents the key sequence `C-x l`.

Key sequences containing function keys, mouse button events, system events, or non-ASCII characters such as `C-=` or `H-a` cannot be represented as strings; they have to be represented as vectors.

In the vector representation, each element of the vector represents an input event, in its Lisp form. See [Input Events](Input-Events.html). For example, the vector `[?\C-x ?l]` represents the key sequence `C-x l`.

For examples of key sequences written in string and vector representations, [Init Rebinding](https://www.gnu.org/software/emacs/manual/html_node/emacs/Init-Rebinding.html#Init-Rebinding) in The GNU Emacs Manual.

*   Function: **kbd** *keyseq-text*

    This function converts the text `keyseq-text` (a string constant) into a key sequence (a string or vector constant). The contents of `keyseq-text` should use the same syntax as in the buffer invoked by the `C-x C-k RET` (`kmacro-edit-macro`) command; in particular, you must surround function key names with ‘`<…>`’. See [Edit Keyboard Macro](https://www.gnu.org/software/emacs/manual/html_node/emacs/Edit-Keyboard-Macro.html#Edit-Keyboard-Macro) in The GNU Emacs Manual.

    ```lisp
    (kbd "C-x") ⇒ "\C-x"
    (kbd "C-x C-f") ⇒ "\C-x\C-f"
    (kbd "C-x 4 C-f") ⇒ "\C-x4\C-f"
    (kbd "X") ⇒ "X"
    (kbd "RET") ⇒ "\^M"
    (kbd "C-c SPC") ⇒ "\C-c "
    (kbd "<f1> SPC") ⇒ [f1 32]
    (kbd "C-M-<down>") ⇒ [C-M-down]
    ```

Next: [Keymap Basics](Keymap-Basics.html), Up: [Keymaps](Keymaps.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
