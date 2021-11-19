

Next: [Event Mod](Event-Mod.html), Previous: [Key Sequence Input](Key-Sequence-Input.html), Up: [Reading Input](Reading-Input.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 21.8.2 Reading One Event

The lowest level functions for command input are `read-event`, `read-char`, and `read-char-exclusive`.

If you need a function to read a character using the minibuffer, use `read-char-from-minibuffer` (see [Multiple Queries](Multiple-Queries.html)).

*   Function: **read-event** *\&optional prompt inherit-input-method seconds*

    This function reads and returns the next event of command input, waiting if necessary until an event is available.

    The returned event may come directly from the user, or from a keyboard macro. It is not decoded by the keyboard’s input coding system (see [Terminal I/O Encoding](Terminal-I_002fO-Encoding.html)).

    If the optional argument `prompt` is non-`nil`, it should be a string to display in the echo area as a prompt. If `prompt` is `nil` or the string ‘`""`’, `read-event` does not display any message to indicate it is waiting for input; instead, it prompts by echoing: it displays descriptions of the events that led to or were read by the current command. See [The Echo Area](The-Echo-Area.html).

    If `inherit-input-method` is non-`nil`, then the current input method (if any) is employed to make it possible to enter a non-ASCII character. Otherwise, input method handling is disabled for reading this event.

    If `cursor-in-echo-area` is non-`nil`, then `read-event` moves the cursor temporarily to the echo area, to the end of any message displayed there. Otherwise `read-event` does not move the cursor.

    If `seconds` is non-`nil`, it should be a number specifying the maximum time to wait for input, in seconds. If no input arrives within that time, `read-event` stops waiting and returns `nil`. A floating point `seconds` means to wait for a fractional number of seconds. Some systems support only a whole number of seconds; on these systems, `seconds` is rounded down. If `seconds` is `nil`, `read-event` waits as long as necessary for input to arrive.

    If `seconds` is `nil`, Emacs is considered idle while waiting for user input to arrive. Idle timers—those created with `run-with-idle-timer` (see [Idle Timers](Idle-Timers.html))—can run during this period. However, if `seconds` is non-`nil`, the state of idleness remains unchanged. If Emacs is non-idle when `read-event` is called, it remains non-idle throughout the operation of `read-event`; if Emacs is idle (which can happen if the call happens inside an idle timer), it remains idle.

    If `read-event` gets an event that is defined as a help character, then in some cases `read-event` processes the event directly without returning. See [Help Functions](Help-Functions.html). Certain other events, called *special events*, are also processed directly within `read-event` (see [Special Events](Special-Events.html)).

    Here is what happens if you call `read-event` and then press the right-arrow function key:

    ```lisp
    (read-event)
         ⇒ right
    ```

<!---->

*   Function: **read-char** *\&optional prompt inherit-input-method seconds*

    This function reads and returns a character input event. If the user generates an event which is not a character (i.e., a mouse click or function key event), `read-char` signals an error. The arguments work as in `read-event`.

    If the event has modifiers, Emacs attempts to resolve them and return the code of the corresponding character. For example, if the user types `C-a`, the function returns 1, which is the ASCII code of the ‘`C-a`’ character. If some of the modifiers cannot be reflected in the character code, `read-char` leaves the unresolved modifier bits set in the returned event. For example, if the user types `C-M-a`, the function returns 134217729, 8000001 in hex, i.e. ‘`C-a`’ with the Meta modifier bit set. This value is not a valid character code: it fails the `characterp` test (see [Character Codes](Character-Codes.html)). Use `event-basic-type` (see [Classifying Events](Classifying-Events.html)) to recover the character code with the modifier bits removed; use `event-modifiers` to test for modifiers in the character event returned by `read-char`.

    In the first example below, the user types the character `1` (ASCII code 49). The second example shows a keyboard macro definition that calls `read-char` from the minibuffer using `eval-expression`. `read-char` reads the keyboard macro’s very next character, which is `1`. Then `eval-expression` displays its return value in the echo area.

    ```lisp
    (read-char)
         ⇒ 49
    ```

    ```lisp
    ```

    ```lisp
    ;; We assume here you use M-: to evaluate this.
    (symbol-function 'foo)
         ⇒ "^[:(read-char)^M1"
    ```

    ```lisp
    (execute-kbd-macro 'foo)
         -| 49
         ⇒ nil
    ```

<!---->

*   Function: **read-char-exclusive** *\&optional prompt inherit-input-method seconds*

    This function reads and returns a character input event. If the user generates an event which is not a character event, `read-char-exclusive` ignores it and reads another event, until it gets a character. The arguments work as in `read-event`. The returned value may include modifier bits, as with `read-char`.

None of the above functions suppress quitting.

*   Variable: **num-nonmacro-input-events**

    This variable holds the total number of input events received so far from the terminal—not counting those generated by keyboard macros.

We emphasize that, unlike `read-key-sequence`, the functions `read-event`, `read-char`, and `read-char-exclusive` do not perform the translations described in [Translation Keymaps](Translation-Keymaps.html). If you wish to read a single key taking these translations into account, use the function `read-key`:

*   Function: **read-key** *\&optional prompt*

    This function reads a single key. It is intermediate between `read-key-sequence` and `read-event`. Unlike the former, it reads a single key, not a key sequence. Unlike the latter, it does not return a raw event, but decodes and translates the user input according to `input-decode-map`, `local-function-key-map`, and `key-translation-map` (see [Translation Keymaps](Translation-Keymaps.html)).

    The argument `prompt` is either a string to be displayed in the echo area as a prompt, or `nil`, meaning not to display a prompt.

<!---->

*   Function: **read-char-choice** *prompt chars \&optional inhibit-quit*

    This function uses `read-key` to read and return a single character. It ignores any input that is not a member of `chars`, a list of accepted characters. Optionally, it will also ignore keyboard-quit events while it is waiting for valid input. If you bind `help-form` (see [Help Functions](Help-Functions.html)) to a non-`nil` value while calling `read-char-choice`, then pressing `help-char` causes it to evaluate `help-form` and display the result. It then continues to wait for a valid input character, or keyboard-quit.

<!---->

*   Function: **read-multiple-choice** *prompt choices*

    Ask user a multiple choice question. `prompt` should be a string that will be displayed as the prompt.

    `choices` is an alist where the first element in each entry is a character to be entered, the second element is a short name for the entry to be displayed while prompting (if there’s room, it might be shortened), and the third, optional entry is a longer explanation that will be displayed in a help buffer if the user requests more help.

    The return value is the matching value from `choices`.

    ```lisp
    (read-multiple-choice
     "Continue connecting?"
     '((?a "always" "Accept certificate for this and future sessions.")
       (?s "session only" "Accept certificate this session only.")
       (?n "no" "Refuse to use certificate, close connection.")))
    ```

    The `read-multiple-choice-face` face is used to highlight the matching characters in the name string on graphical terminals.

Next: [Event Mod](Event-Mod.html), Previous: [Key Sequence Input](Key-Sequence-Input.html), Up: [Reading Input](Reading-Input.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
