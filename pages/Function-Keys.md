

Next: [Mouse Events](Mouse-Events.html), Previous: [Keyboard Events](Keyboard-Events.html), Up: [Input Events](Input-Events.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 21.7.2 Function Keys

Most keyboards also have *function keys*—keys that have names or symbols that are not characters. Function keys are represented in Emacs Lisp as symbols; the symbol’s name is the function key’s label, in lower case. For example, pressing a key labeled `F1` generates an input event represented by the symbol `f1`.

The event type of a function key event is the event symbol itself. See [Classifying Events](Classifying-Events.html).

Here are a few special cases in the symbol-naming convention for function keys:

*   `backspace`, `tab`, `newline`, `return`, `delete`

    These keys correspond to common ASCII control characters that have special keys on most keyboards.

    In ASCII, `C-i` and `TAB` are the same character. If the terminal can distinguish between them, Emacs conveys the distinction to Lisp programs by representing the former as the integer 9, and the latter as the symbol `tab`.

    Most of the time, it’s not useful to distinguish the two. So normally `local-function-key-map` (see [Translation Keymaps](Translation-Keymaps.html)) is set up to map `tab` into 9. Thus, a key binding for character code 9 (the character `C-i`) also applies to `tab`. Likewise for the other symbols in this group. The function `read-char` likewise converts these events into characters.

    In ASCII, `BS` is really `C-h`. But `backspace` converts into the character code 127 (`DEL`), not into code 8 (`BS`). This is what most users prefer.

*   `left`, `up`, `right`, `down`

    Cursor arrow keys

*   `kp-add`, `kp-decimal`, `kp-divide`, …

    Keypad keys (to the right of the regular keyboard).

*   `kp-0`, `kp-1`, …

    Keypad keys with digits.

*   `kp-f1`, `kp-f2`, `kp-f3`, `kp-f4`

    Keypad PF keys.

*   `kp-home`, `kp-left`, `kp-up`, `kp-right`, `kp-down`

    Keypad arrow keys. Emacs normally translates these into the corresponding non-keypad keys `home`, `left`, …

*   `kp-prior`, `kp-next`, `kp-end`, `kp-begin`, `kp-insert`, `kp-delete`

    Additional keypad duplicates of keys ordinarily found elsewhere. Emacs normally translates these into the like-named non-keypad keys.

You can use the modifier keys `ALT`, `CTRL`, `HYPER`, `META`, `SHIFT`, and `SUPER` with function keys. The way to represent them is with prefixes in the symbol name:

*   ‘`A-`’

    The alt modifier.

*   ‘`C-`’

    The control modifier.

*   ‘`H-`’

    The hyper modifier.

*   ‘`M-`’

    The meta modifier.

*   ‘`S-`’

    The shift modifier.

*   ‘`s-`’

    The super modifier.

Thus, the symbol for the key `F3` with `META` held down is `M-f3`. When you use more than one prefix, we recommend you write them in alphabetical order; but the order does not matter in arguments to the key-binding lookup and modification functions.

Next: [Mouse Events](Mouse-Events.html), Previous: [Keyboard Events](Keyboard-Events.html), Up: [Input Events](Input-Events.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
