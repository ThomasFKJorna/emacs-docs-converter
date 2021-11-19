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

Next: [Auto Major Mode](Auto-Major-Mode.html), Up: [Major Modes](Major-Modes.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 23.2.1 Major Mode Conventions

The code for every major mode should follow various coding conventions, including conventions for local keymap and syntax table initialization, function and variable names, and hooks.

If you use the `define-derived-mode` macro, it will take care of many of these conventions automatically. See [Derived Modes](Derived-Modes.html). Note also that Fundamental mode is an exception to many of these conventions, because it represents the default state of Emacs.

The following list of conventions is only partial. Each major mode should aim for consistency in general with other Emacs major modes, as this makes Emacs as a whole more coherent. It is impossible to list here all the possible points where this issue might come up; if the Emacs developers point out an area where your major mode deviates from the usual conventions, please make it compatible.

*   Define a major mode command whose name ends in ‘`-mode`’. When called with no arguments, this command should switch to the new mode in the current buffer by setting up the keymap, syntax table, and buffer-local variables in an existing buffer. It should not change the buffer’s contents.

*   Write a documentation string for this command that describes the special commands available in this mode. See [Mode Help](Mode-Help.html).

    The documentation string may include the special documentation substrings, ‘`\[command]`’, ‘`\{keymap}`’, and ‘`\<keymap>`’, which allow the help display to adapt automatically to the user’s own key bindings. See [Keys in Documentation](Keys-in-Documentation.html).

*   The major mode command should start by calling `kill-all-local-variables`. This runs the normal hook `change-major-mode-hook`, then gets rid of the buffer-local variables of the major mode previously in effect. See [Creating Buffer-Local](Creating-Buffer_002dLocal.html).

*   The major mode command should set the variable `major-mode` to the major mode command symbol. This is how `describe-mode` discovers which documentation to print.

*   The major mode command should set the variable `mode-name` to the “pretty” name of the mode, usually a string (but see [Mode Line Data](Mode-Line-Data.html), for other possible forms). The name of the mode appears in the mode line.

*   Calling the major mode command twice in direct succession should not fail and should do the same thing as calling the command only once. In other words, the major mode command should be idempotent.

*   Since all global names are in the same name space, all the global variables, constants, and functions that are part of the mode should have names that start with the major mode name (or with an abbreviation of it if the name is long). See [Coding Conventions](Coding-Conventions.html).

*   In a major mode for editing some kind of structured text, such as a programming language, indentation of text according to structure is probably useful. So the mode should set `indent-line-function` to a suitable function, and probably customize other variables for indentation. See [Auto-Indentation](Auto_002dIndentation.html).

*   The major mode should usually have its own keymap, which is used as the local keymap in all buffers in that mode. The major mode command should call `use-local-map` to install this local map. See [Active Keymaps](Active-Keymaps.html), for more information.

    This keymap should be stored permanently in a global variable named `modename-mode-map`. Normally the library that defines the mode sets this variable.

    See [Tips for Defining](Tips-for-Defining.html), for advice about how to write the code to set up the mode’s keymap variable.

*   The key sequences bound in a major mode keymap should usually start with `C-c`, followed by a control character, a digit, or `{`, `}`, `<`, `>`, `:` or `;`. The other punctuation characters are reserved for minor modes, and ordinary letters are reserved for users.

    A major mode can also rebind the keys `M-n`, `M-p` and `M-s`. The bindings for `M-n` and `M-p` should normally be some kind of moving forward and backward, but this does not necessarily mean cursor motion.

    It is legitimate for a major mode to rebind a standard key sequence if it provides a command that does the same job in a way better suited to the text this mode is used for. For example, a major mode for editing a programming language might redefine `C-M-a` to move to the beginning of a function in a way that works better for that language. The recommended way of tailoring `C-M-a` to the needs of a major mode is to set `beginning-of-defun-function` (see [List Motion](List-Motion.html)) to invoke the function specific to the mode.

    It is also legitimate for a major mode to rebind a standard key sequence whose standard meaning is rarely useful in that mode. For instance, minibuffer modes rebind `M-r`, whose standard meaning is rarely of any use in the minibuffer. Major modes such as Dired or Rmail that do not allow self-insertion of text can reasonably redefine letters and other printing characters as special commands.

*   Major modes for editing text should not define `RET` to do anything other than insert a newline. However, it is ok for specialized modes for text that users don’t directly edit, such as Dired and Info modes, to redefine `RET` to do something entirely different.

*   Major modes should not alter options that are primarily a matter of user preference, such as whether Auto-Fill mode is enabled. Leave this to each user to decide. However, a major mode should customize other variables so that Auto-Fill mode will work usefully *if* the user decides to use it.

*   The mode may have its own syntax table or may share one with other related modes. If it has its own syntax table, it should store this in a variable named `modename-mode-syntax-table`. See [Syntax Tables](Syntax-Tables.html).

*   If the mode handles a language that has a syntax for comments, it should set the variables that define the comment syntax. See [Options Controlling Comments](https://www.gnu.org/software/emacs/manual/html_node/emacs/Options-for-Comments.html#Options-for-Comments) in The GNU Emacs Manual.

*   The mode may have its own abbrev table or may share one with other related modes. If it has its own abbrev table, it should store this in a variable named `modename-mode-abbrev-table`. If the major mode command defines any abbrevs itself, it should pass `t` for the `system-flag` argument to `define-abbrev`. See [Defining Abbrevs](Defining-Abbrevs.html).

*   The mode should specify how to do highlighting for Font Lock mode, by setting up a buffer-local value for the variable `font-lock-defaults` (see [Font Lock Mode](Font-Lock-Mode.html)).

*   Each face that the mode defines should, if possible, inherit from an existing Emacs face. See [Basic Faces](Basic-Faces.html), and [Faces for Font Lock](Faces-for-Font-Lock.html).

*   The mode should specify how Imenu should find the definitions or sections of a buffer, by setting up a buffer-local value for the variable `imenu-generic-expression`, for the two variables `imenu-prev-index-position-function` and `imenu-extract-index-name-function`, or for the variable `imenu-create-index-function` (see [Imenu](Imenu.html)).

*   The mode can specify a local value for `eldoc-documentation-function` to tell ElDoc mode how to handle this mode.

*   The mode can specify how to complete various keywords by adding one or more buffer-local entries to the special hook `completion-at-point-functions`. See [Completion in Buffers](Completion-in-Buffers.html).

*   To make a buffer-local binding for an Emacs customization variable, use `make-local-variable` in the major mode command, not `make-variable-buffer-local`. The latter function would make the variable local to every buffer in which it is subsequently set, which would affect buffers that do not use this mode. It is undesirable for a mode to have such global effects. See [Buffer-Local Variables](Buffer_002dLocal-Variables.html).

    With rare exceptions, the only reasonable way to use `make-variable-buffer-local` in a Lisp package is for a variable which is used only within that package. Using it on a variable used by other packages would interfere with them.

*   Each major mode should have a normal *mode hook* named `modename-mode-hook`. The very last thing the major mode command should do is to call `run-mode-hooks`. This runs the normal hook `change-major-mode-after-body-hook`, the mode hook, the function `hack-local-variables` (when the buffer is visiting a file), and then the normal hook `after-change-major-mode-hook`. See [Mode Hooks](Mode-Hooks.html).

*   The major mode command may start by calling some other major mode command (called the *parent mode*) and then alter some of its settings. A mode that does this is called a *derived mode*. The recommended way to define one is to use the `define-derived-mode` macro, but this is not required. Such a mode should call the parent mode command inside a `delay-mode-hooks` form. (Using `define-derived-mode` does this automatically.) See [Derived Modes](Derived-Modes.html), and [Mode Hooks](Mode-Hooks.html).

*   If something special should be done if the user switches a buffer from this mode to any other major mode, this mode can set up a buffer-local value for `change-major-mode-hook` (see [Creating Buffer-Local](Creating-Buffer_002dLocal.html)).

*   If this mode is appropriate only for specially-prepared text produced by the mode itself (rather than by the user typing at the keyboard or by an external file), then the major mode command symbol should have a property named `mode-class` with value `special`, put on as follows:

        (put 'funny-mode 'mode-class 'special)

    This tells Emacs that new buffers created while the current buffer is in Funny mode should not be put in Funny mode, even though the default value of `major-mode` is `nil`. By default, the value of `nil` for `major-mode` means to use the current buffer’s major mode when creating new buffers (see [Auto Major Mode](Auto-Major-Mode.html)), but with such `special` modes, Fundamental mode is used instead. Modes such as Dired, Rmail, and Buffer List use this feature.

    The function `view-buffer` does not enable View mode in buffers whose mode-class is special, because such modes usually provide their own View-like bindings.

    The `define-derived-mode` macro automatically marks the derived mode as special if the parent mode is special. Special mode is a convenient parent for such modes to inherit from; See [Basic Major Modes](Basic-Major-Modes.html).

*   If you want to make the new mode the default for files with certain recognizable names, add an element to `auto-mode-alist` to select the mode for those file names (see [Auto Major Mode](Auto-Major-Mode.html)). If you define the mode command to autoload, you should add this element in the same file that calls `autoload`. If you use an autoload cookie for the mode command, you can also use an autoload cookie for the form that adds the element (see [autoload cookie](Autoload.html#autoload-cookie)). If you do not autoload the mode command, it is sufficient to add the element in the file that contains the mode definition.

*   The top-level forms in the file defining the mode should be written so that they may be evaluated more than once without adverse consequences. For instance, use `defvar` or `defcustom` to set mode-related variables, so that they are not reinitialized if they already have a value (see [Defining Variables](Defining-Variables.html)).

Next: [Auto Major Mode](Auto-Major-Mode.html), Up: [Major Modes](Major-Modes.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
