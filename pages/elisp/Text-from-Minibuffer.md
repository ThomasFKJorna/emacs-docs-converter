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

Next: [Object from Minibuffer](Object-from-Minibuffer.html), Previous: [Intro to Minibuffers](Intro-to-Minibuffers.html), Up: [Minibuffers](Minibuffers.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 20.2 Reading Text Strings with the Minibuffer

The most basic primitive for minibuffer input is `read-from-minibuffer`, which can be used to read either a string or a Lisp object in textual form. The function `read-regexp` is used for reading regular expressions (see [Regular Expressions](Regular-Expressions.html)), which are a special kind of string. There are also specialized functions for reading commands, variables, file names, etc. (see [Completion](Completion.html)).

In most cases, you should not call minibuffer input functions in the middle of a Lisp function. Instead, do all minibuffer input as part of reading the arguments for a command, in the `interactive` specification. See [Defining Commands](Defining-Commands.html).

*   Function: **read-from-minibuffer** *prompt \&optional initial keymap read history default inherit-input-method*

    This function is the most general way to get input from the minibuffer. By default, it accepts arbitrary text and returns it as a string; however, if `read` is non-`nil`, then it uses `read` to convert the text into a Lisp object (see [Input Functions](Input-Functions.html)).

    The first thing this function does is to activate a minibuffer and display it with `prompt` (which must be a string) as the prompt. Then the user can edit text in the minibuffer.

    When the user types a command to exit the minibuffer, `read-from-minibuffer` constructs the return value from the text in the minibuffer. Normally it returns a string containing that text. However, if `read` is non-`nil`, `read-from-minibuffer` reads the text and returns the resulting Lisp object, unevaluated. (See [Input Functions](Input-Functions.html), for information about reading.)

    The argument `default` specifies default values to make available through the history commands. It should be a string, a list of strings, or `nil`. The string or strings become the minibuffer’s “future history”, available to the user with `M-n`.

    If `read` is non-`nil`, then `default` is also used as the input to `read`, if the user enters empty input. If `default` is a list of strings, the first string is used as the input. If `default` is `nil`, empty input results in an `end-of-file` error. However, in the usual case (where `read` is `nil`), `read-from-minibuffer` ignores `default` when the user enters empty input and returns an empty string, `""`. In this respect, it differs from all the other minibuffer input functions in this chapter.

    If `keymap` is non-`nil`, that keymap is the local keymap to use in the minibuffer. If `keymap` is omitted or `nil`, the value of `minibuffer-local-map` is used as the keymap. Specifying a keymap is the most important way to customize the minibuffer for various applications such as completion.

    The argument `history` specifies a history list variable to use for saving the input and for history commands used in the minibuffer. It defaults to `minibuffer-history`. You can optionally specify a starting position in the history list as well. See [Minibuffer History](Minibuffer-History.html).

    If the variable `minibuffer-allow-text-properties` is non-`nil`, then the string that is returned includes whatever text properties were present in the minibuffer. Otherwise all the text properties are stripped when the value is returned.

    The text properties in `minibuffer-prompt-properties` are applied to the prompt. By default, this property list defines a face to use for the prompt. This face, if present, is applied to the end of the face list and merged before display.

    If the user wants to completely control the look of the prompt, the most convenient way to do that is to specify the `default` face at the end of all face lists. For instance:

        (read-from-minibuffer
         (concat
          (propertize "Bold" 'face '(bold default))
          (propertize " and normal: " 'face '(default))))

    If the argument `inherit-input-method` is non-`nil`, then the minibuffer inherits the current input method (see [Input Methods](Input-Methods.html)) and the setting of `enable-multibyte-characters` (see [Text Representations](Text-Representations.html)) from whichever buffer was current before entering the minibuffer.

    Use of `initial` is mostly deprecated; we recommend using a non-`nil` value only in conjunction with specifying a cons cell for `history`. See [Initial Input](Initial-Input.html).

<!---->

*   Function: **read-string** *prompt \&optional initial history default inherit-input-method*

    This function reads a string from the minibuffer and returns it. The arguments `prompt`, `initial`, `history` and `inherit-input-method` are used as in `read-from-minibuffer`. The keymap used is `minibuffer-local-map`.

    The optional argument `default` is used as in `read-from-minibuffer`, except that, if non-`nil`, it also specifies a default value to return if the user enters null input. As in `read-from-minibuffer` it should be a string, a list of strings, or `nil`, which is equivalent to an empty string. When `default` is a string, that string is the default value. When it is a list of strings, the first string is the default value. (All these strings are available to the user in the “future minibuffer history”.)

    This function works by calling the `read-from-minibuffer` function:

        (read-string prompt initial history default inherit)
        ≡
        (let ((value
               (read-from-minibuffer prompt initial nil nil
                                     history default inherit)))
          (if (and (equal value "") default)
              (if (consp default) (car default) default)
            value))

<!---->

*   Function: **read-regexp** *prompt \&optional defaults history*

    This function reads a regular expression as a string from the minibuffer and returns it. If the minibuffer prompt string `prompt` does not end in ‘`:`’ (followed by optional whitespace), the function adds ‘`: `’ to the end, preceded by the default return value (see below), if that is non-empty.

    The optional argument `defaults` controls the default value to return if the user enters null input, and should be one of: a string; `nil`, which is equivalent to an empty string; a list of strings; or a symbol.

    If `defaults` is a symbol, `read-regexp` consults the value of the variable `read-regexp-defaults-function` (see below), and if that is non-`nil` uses it in preference to `defaults`. The value in this case should be either:

    *   \- `regexp-history-last`, which means to use the first element of the appropriate minibuffer history list (see below).
    *   \- A function of no arguments, whose return value (which should be `nil`, a string, or a list of strings) becomes the value of `defaults`.

    `read-regexp` now ensures that the result of processing `defaults` is a list (i.e., if the value is `nil` or a string, it converts it to a list of one element). To this list, `read-regexp` then appends a few potentially useful candidates for input. These are:

    *   \- The word or symbol at point.
    *   \- The last regexp used in an incremental search.
    *   \- The last string used in an incremental search.
    *   \- The last string or pattern used in query-replace commands.

    The function now has a list of regular expressions that it passes to `read-from-minibuffer` to obtain the user’s input. The first element of the list is the default result in case of empty input. All elements of the list are available to the user as the “future minibuffer history” list (see [future list](https://www.gnu.org/software/emacs/manual/html_node/emacs/Minibuffer-History.html#Minibuffer-History) in The GNU Emacs Manual).

    The optional argument `history`, if non-`nil`, is a symbol specifying a minibuffer history list to use (see [Minibuffer History](Minibuffer-History.html)). If it is omitted or `nil`, the history list defaults to `regexp-history`.

<!---->

*   User Option: **read-regexp-defaults-function**

    The function `read-regexp` may use the value of this variable to determine its list of default regular expressions. If non-`nil`, the value of this variable should be either:

    *   \- The symbol `regexp-history-last`.
    *   \- A function of no arguments that returns either `nil`, a string, or a list of strings.

    See `read-regexp` above for details of how these values are used.

<!---->

*   Variable: **minibuffer-allow-text-properties**

    If this variable is `nil`, then `read-from-minibuffer` and `read-string` strip all text properties from the minibuffer input before returning it. However, `read-no-blanks-input` (see below), as well as `read-minibuffer` and related functions (see [Reading Lisp Objects With the Minibuffer](Object-from-Minibuffer.html)), and all functions that do minibuffer input with completion, discard text properties unconditionally, regardless of the value of this variable.

<!---->

*   Variable: **minibuffer-local-map**

    This is the default local keymap for reading from the minibuffer. By default, it makes the following bindings:

    *   `C-j`

        `exit-minibuffer`

    *   `RET`

        `exit-minibuffer`

    *   `M-<`

        `minibuffer-beginning-of-buffer`

    *   `C-g`

        `abort-recursive-edit`

    *   *   `M-n`
        *   `DOWN`

        `next-history-element`

    *   *   `M-p`
        *   `UP`

        `previous-history-element`

    *   `M-s`

        `next-matching-history-element`

    *   `M-r`

        `previous-matching-history-element`

<!---->

*   Function: **read-no-blanks-input** *prompt \&optional initial inherit-input-method*

    This function reads a string from the minibuffer, but does not allow whitespace characters as part of the input: instead, those characters terminate the input. The arguments `prompt`, `initial`, and `inherit-input-method` are used as in `read-from-minibuffer`.

    This is a simplified interface to the `read-from-minibuffer` function, and passes the value of the `minibuffer-local-ns-map` keymap as the `keymap` argument for that function. Since the keymap `minibuffer-local-ns-map` does not rebind `C-q`, it *is* possible to put a space into the string, by quoting it.

    This function discards text properties, regardless of the value of `minibuffer-allow-text-properties`.

        (read-no-blanks-input prompt initial)
        ≡
        (let (minibuffer-allow-text-properties)
          (read-from-minibuffer prompt initial minibuffer-local-ns-map))

<!---->

*   Variable: **minibuffer-local-ns-map**

    This built-in variable is the keymap used as the minibuffer local keymap in the function `read-no-blanks-input`. By default, it makes the following bindings, in addition to those of `minibuffer-local-map`:

    *   `SPC`

        `exit-minibuffer`

    *   `TAB`

        `exit-minibuffer`

    *   `?`

        `self-insert-and-exit`

Next: [Object from Minibuffer](Object-from-Minibuffer.html), Previous: [Intro to Minibuffers](Intro-to-Minibuffers.html), Up: [Minibuffers](Minibuffers.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
