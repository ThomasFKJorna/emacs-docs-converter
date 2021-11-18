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

Previous: [Describing Characters](Describing-Characters.html), Up: [Documentation](Documentation.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 24.6 Help Functions

Emacs provides a variety of built-in help functions, all accessible to the user as subcommands of the prefix `C-h`. For more information about them, see [Help](https://www.gnu.org/software/emacs/manual/html_node/emacs/Help.html#Help) in The GNU Emacs Manual. Here we describe some program-level interfaces to the same information.

*   Command: **apropos** *pattern \&optional do-all*

    This function finds all meaningful symbols whose names contain a match for the apropos pattern `pattern`. An apropos pattern is either a word to match, a space-separated list of words of which at least two must match, or a regular expression (if any special regular expression characters occur). A symbol is meaningful if it has a definition as a function, variable, or face, or has properties.

    The function returns a list of elements that look like this:

        (symbol score function-doc variable-doc
         plist-doc widget-doc face-doc group-doc)

    Here, `score` is an integer measure of how important the symbol seems to be as a match. Each of the remaining elements is a documentation string, or `nil`, for `symbol` as a function, variable, etc.

    It also displays the symbols in a buffer named `*Apropos*`, each with a one-line description taken from the beginning of its documentation string.

    If `do-all` is non-`nil`, or if the user option `apropos-do-all` is non-`nil`, then `apropos` also shows key bindings for the functions that are found; it also shows *all* interned symbols, not just meaningful ones (and it lists them in the return value as well).

<!---->

*   Variable: **help-map**

    The value of this variable is a local keymap for characters following the Help key, `C-h`.

<!---->

*   Prefix Command: **help-command**

    This symbol is not a function; its function definition cell holds the keymap known as `help-map`. It is defined in `help.el` as follows:

        (define-key global-map (string help-char) 'help-command)
        (fset 'help-command help-map)

<!---->

*   User Option: **help-char**

    The value of this variable is the help character—the character that Emacs recognizes as meaning Help. By default, its value is 8, which stands for `C-h`. When Emacs reads this character, if `help-form` is a non-`nil` Lisp expression, it evaluates that expression, and displays the result in a window if it is a string.

    Usually the value of `help-form` is `nil`. Then the help character has no special meaning at the level of command input, and it becomes part of a key sequence in the normal way. The standard key binding of `C-h` is a prefix key for several general-purpose help features.

    The help character is special after prefix keys, too. If it has no binding as a subcommand of the prefix key, it runs `describe-prefix-bindings`, which displays a list of all the subcommands of the prefix key.

<!---->

*   User Option: **help-event-list**

    The value of this variable is a list of event types that serve as alternative help characters. These events are handled just like the event specified by `help-char`.

<!---->

*   Variable: **help-form**

    If this variable is non-`nil`, its value is a form to evaluate whenever the character `help-char` is read. If evaluating the form produces a string, that string is displayed.

    A command that calls `read-event`, `read-char-choice`, or `read-char` probably should bind `help-form` to a non-`nil` expression while it does input. (The time when you should not do this is when `C-h` has some other meaning.) Evaluating this expression should result in a string that explains what the input is for and how to enter it properly.

    Entry to the minibuffer binds this variable to the value of `minibuffer-help-form` (see [Definition of minibuffer-help-form](Minibuffer-Misc.html#Definition-of-minibuffer_002dhelp_002dform)).

<!---->

*   Variable: **prefix-help-command**

    This variable holds a function to print help for a prefix key. The function is called when the user types a prefix key followed by the help character, and the help character has no binding after that prefix. The variable’s default value is `describe-prefix-bindings`.

<!---->

*   Command: **describe-prefix-bindings**

    This function calls `describe-bindings` to display a list of all the subcommands of the prefix key of the most recent key sequence. The prefix described consists of all but the last event of that key sequence. (The last event is, presumably, the help character.)

The following two functions are meant for modes that want to provide help without relinquishing control, such as the electric modes. Their names begin with ‘`Helper`’ to distinguish them from the ordinary help functions.

*   Command: **Helper-describe-bindings**

    This command pops up a window displaying a help buffer containing a listing of all of the key bindings from both the local and global keymaps. It works by calling `describe-bindings`.

<!---->

*   Command: **Helper-help**

    This command provides help for the current mode. It prompts the user in the minibuffer with the message ‘`Help (Type ? for further options)`’, and then provides assistance in finding out what the key bindings are, and what the mode is intended for. It returns `nil`.

    This can be customized by changing the map `Helper-help-map`.

<!---->

*   Variable: **data-directory**

    This variable holds the name of the directory in which Emacs finds certain documentation and text files that come with Emacs.

<!---->

*   Function: **help-buffer**

    This function returns the name of the help buffer, which is normally `*Help*`; if such a buffer does not exist, it is first created.

<!---->

*   Macro: **with-help-window** *buffer-or-name body…*

    This macro evaluates `body` like `with-output-to-temp-buffer` (see [Temporary Displays](Temporary-Displays.html)), inserting any output produced by its forms into a buffer specified by `buffer-or-name`, which can be a buffer or the name of a buffer. (Frequently, `buffer-or-name` is the value returned by the function `help-buffer`.) This macro puts the specified buffer into Help mode and displays a message telling the user how to quit and scroll the help window. It selects the help window if the current value of the user option `help-window-select` has been set accordingly. It returns the last value in `body`.

<!---->

*   Function: **help-setup-xref** *item interactive-p*

    This function updates the cross reference data in the `*Help*` buffer, which is used to regenerate the help information when the user clicks on the ‘`Back`’ or ‘`Forward`’ buttons. Most commands that use the `*Help*` buffer should invoke this function before clearing the buffer. The `item` argument should have the form `(function . args)`, where `function` is a function to call, with argument list `args`, to regenerate the help buffer. The `interactive-p` argument is non-`nil` if the calling command was invoked interactively; in that case, the stack of items for the `*Help*` buffer’s ‘`Back`’ buttons is cleared.

See [describe-symbols example](Accessing-Documentation.html#describe_002dsymbols-example), for an example of using `help-buffer`, `with-help-window`, and `help-setup-xref`.

*   Macro: **make-help-screen** *fname help-line help-text help-map*

    This macro defines a help command named `fname` that acts like a prefix key that shows a list of the subcommands it offers.

    When invoked, `fname` displays `help-text` in a window, then reads and executes a key sequence according to `help-map`. The string `help-text` should describe the bindings available in `help-map`.

    The command `fname` is defined to handle a few events itself, by scrolling the display of `help-text`. When `fname` reads one of those special events, it does the scrolling and then reads another event. When it reads an event that is not one of those few, and which has a binding in `help-map`, it executes that key’s binding and then returns.

    The argument `help-line` should be a single-line summary of the alternatives in `help-map`. In the current version of Emacs, this argument is used only if you set the option `three-step-help` to `t`.

    This macro is used in the command `help-for-help` which is the binding of `C-h C-h`.

<!---->

*   User Option: **three-step-help**

    If this variable is non-`nil`, commands defined with `make-help-screen` display their `help-line` strings in the echo area at first, and display the longer `help-text` strings only if the user types the help character again.

Previous: [Describing Characters](Describing-Characters.html), Up: [Documentation](Documentation.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
