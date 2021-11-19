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

Next: [Completion Variables](Completion-Variables.html), Previous: [High-Level Completion](High_002dLevel-Completion.html), Up: [Completion](Completion.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 20.6.5 Reading File Names

The high-level completion functions `read-file-name`, `read-directory-name`, and `read-shell-command` are designed to read file names, directory names, and shell commands, respectively. They provide special features, including automatic insertion of the default directory.

*   Function: **read-file-name** *prompt \&optional directory default require-match initial predicate*

    This function reads a file name, prompting with `prompt` and providing completion.

    As an exception, this function reads a file name using a graphical file dialog instead of the minibuffer, if all of the following are true:

    1.  It is invoked via a mouse command.
    2.  The selected frame is on a graphical display supporting such dialogs.
    3.  The variable `use-dialog-box` is non-`nil`. See [Dialog Boxes](https://www.gnu.org/software/emacs/manual/html_node/emacs/Dialog-Boxes.html#Dialog-Boxes) in The GNU Emacs Manual.
    4.  The `directory` argument, described below, does not specify a remote file. See [Remote Files](https://www.gnu.org/software/emacs/manual/html_node/emacs/Remote-Files.html#Remote-Files) in The GNU Emacs Manual.

    The exact behavior when using a graphical file dialog is platform-dependent. Here, we simply document the behavior when using the minibuffer.

    `read-file-name` does not automatically expand the returned file name. You can call `expand-file-name` yourself if an absolute file name is required.

    The optional argument `require-match` has the same meaning as in `completing-read`. See [Minibuffer Completion](Minibuffer-Completion.html).

    The argument `directory` specifies the directory to use for completing relative file names. It should be an absolute directory name. If the variable `insert-default-directory` is non-`nil`, `directory` is also inserted in the minibuffer as initial input. It defaults to the current buffer’s value of `default-directory`.

    If you specify `initial`, that is an initial file name to insert in the buffer (after `directory`, if that is inserted). In this case, point goes at the beginning of `initial`. The default for `initial` is `nil`—don’t insert any file name. To see what `initial` does, try the command `C-x C-v` in a buffer visiting a file. **Please note:** we recommend using `default` rather than `initial` in most cases.

    If `default` is non-`nil`, then the function returns `default` if the user exits the minibuffer with the same non-empty contents that `read-file-name` inserted initially. The initial minibuffer contents are always non-empty if `insert-default-directory` is non-`nil`, as it is by default. `default` is not checked for validity, regardless of the value of `require-match`. However, if `require-match` is non-`nil`, the initial minibuffer contents should be a valid file (or directory) name. Otherwise `read-file-name` attempts completion if the user exits without any editing, and does not return `default`. `default` is also available through the history commands.

    If `default` is `nil`, `read-file-name` tries to find a substitute default to use in its place, which it treats in exactly the same way as if it had been specified explicitly. If `default` is `nil`, but `initial` is non-`nil`, then the default is the absolute file name obtained from `directory` and `initial`. If both `default` and `initial` are `nil` and the buffer is visiting a file, `read-file-name` uses the absolute file name of that file as default. If the buffer is not visiting a file, then there is no default. In that case, if the user types `RET` without any editing, `read-file-name` simply returns the pre-inserted contents of the minibuffer.

    If the user types `RET` in an empty minibuffer, this function returns an empty string, regardless of the value of `require-match`. This is, for instance, how the user can make the current buffer visit no file using `M-x set-visited-file-name`.

    If `predicate` is non-`nil`, it specifies a function of one argument that decides which file names are acceptable completion alternatives. A file name is an acceptable value if `predicate` returns non-`nil` for it.

    Here is an example of using `read-file-name`:

        (read-file-name "The file is ")

        ;; After evaluation of the preceding expression,
        ;;   the following appears in the minibuffer:

    ```
    ```

        ---------- Buffer: Minibuffer ----------
        The file is /gp/gnu/elisp/∗
        ---------- Buffer: Minibuffer ----------

    Typing `manual TAB` results in the following:

        ---------- Buffer: Minibuffer ----------
        The file is /gp/gnu/elisp/manual.texi∗
        ---------- Buffer: Minibuffer ----------

    If the user types `RET`, `read-file-name` returns the file name as the string `"/gp/gnu/elisp/manual.texi"`.

<!---->

*   Variable: **read-file-name-function**

    If non-`nil`, this should be a function that accepts the same arguments as `read-file-name`. When `read-file-name` is called, it calls this function with the supplied arguments instead of doing its usual work.

<!---->

*   User Option: **read-file-name-completion-ignore-case**

    If this variable is non-`nil`, `read-file-name` ignores case when performing completion.

<!---->

*   Function: **read-directory-name** *prompt \&optional directory default require-match initial*

    This function is like `read-file-name` but allows only directory names as completion alternatives.

    If `default` is `nil` and `initial` is non-`nil`, `read-directory-name` constructs a substitute default by combining `directory` (or the current buffer’s default directory if `directory` is `nil`) and `initial`. If both `default` and `initial` are `nil`, this function uses `directory` as substitute default, or the current buffer’s default directory if `directory` is `nil`.

<!---->

*   User Option: **insert-default-directory**

    This variable is used by `read-file-name`, and thus, indirectly, by most commands reading file names. (This includes all commands that use the code letters ‘`f`’ or ‘`F`’ in their interactive form. See [Code Characters for interactive](Interactive-Codes.html).) Its value controls whether `read-file-name` starts by placing the name of the default directory in the minibuffer, plus the initial file name, if any. If the value of this variable is `nil`, then `read-file-name` does not place any initial input in the minibuffer (unless you specify initial input with the `initial` argument). In that case, the default directory is still used for completion of relative file names, but is not displayed.

    If this variable is `nil` and the initial minibuffer contents are empty, the user may have to explicitly fetch the next history element to access a default value. If the variable is non-`nil`, the initial minibuffer contents are always non-empty and the user can always request a default value by immediately typing `RET` in an unedited minibuffer. (See above.)

    For example:

        ;; Here the minibuffer starts out with the default directory.
        (let ((insert-default-directory t))
          (read-file-name "The file is "))

    ```
    ```

        ---------- Buffer: Minibuffer ----------
        The file is ~lewis/manual/∗
        ---------- Buffer: Minibuffer ----------

    ```
    ```

        ;; Here the minibuffer is empty and only the prompt
        ;;   appears on its line.
        (let ((insert-default-directory nil))
          (read-file-name "The file is "))

    ```
    ```

        ---------- Buffer: Minibuffer ----------
        The file is ∗
        ---------- Buffer: Minibuffer ----------

<!---->

*   Function: **read-shell-command** *prompt \&optional initial history \&rest args*

    This function reads a shell command from the minibuffer, prompting with `prompt` and providing intelligent completion. It completes the first word of the command using candidates that are appropriate for command names, and the rest of the command words as file names.

    This function uses `minibuffer-local-shell-command-map` as the keymap for minibuffer input. The `history` argument specifies the history list to use; if is omitted or `nil`, it defaults to `shell-command-history` (see [shell-command-history](Minibuffer-History.html)). The optional argument `initial` specifies the initial content of the minibuffer (see [Initial Input](Initial-Input.html)). The rest of `args`, if present, are used as the `default` and `inherit-input-method` arguments in `read-from-minibuffer` (see [Text from Minibuffer](Text-from-Minibuffer.html)).

<!---->

*   Variable: **minibuffer-local-shell-command-map**

    This keymap is used by `read-shell-command` for completing command and file names that are part of a shell command. It uses `minibuffer-local-map` as its parent keymap, and binds `TAB` to `completion-at-point`.

Next: [Completion Variables](Completion-Variables.html), Previous: [High-Level Completion](High_002dLevel-Completion.html), Up: [Completion](Completion.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
