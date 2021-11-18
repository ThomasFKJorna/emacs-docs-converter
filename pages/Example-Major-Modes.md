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

Previous: [Generic Modes](Generic-Modes.html), Up: [Major Modes](Major-Modes.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 23.2.9 Major Mode Examples

Text mode is perhaps the simplest mode besides Fundamental mode. Here are excerpts from `text-mode.el` that illustrate many of the conventions listed above:

    ;; Create the syntax table for this mode.
    (defvar text-mode-syntax-table
      (let ((st (make-syntax-table)))
        (modify-syntax-entry ?\" ".   " st)
        (modify-syntax-entry ?\\ ".   " st)
        ;; Add 'p' so M-c on 'hello' leads to 'Hello', not 'hello'.
        (modify-syntax-entry ?' "w p" st)
        …
        st)
      "Syntax table used while in `text-mode'.")

```

;; Create the keymap for this mode.
```

    (defvar text-mode-map
      (let ((map (make-sparse-keymap)))
        (define-key map "\e\t" 'ispell-complete-word)
        …
        map)
      "Keymap for `text-mode'.
    Many other modes, such as `mail-mode', `outline-mode' and
    `indented-text-mode', inherit all the commands defined in this map.")

Here is how the actual mode command is defined now:

    (define-derived-mode text-mode nil "Text"
      "Major mode for editing text written for humans to read.
    In this mode, paragraphs are delimited only by blank or white lines.
    You can thus get the full benefit of adaptive filling
     (see the variable `adaptive-fill-mode').
    \\{text-mode-map}
    Turning on Text mode runs the normal hook `text-mode-hook'."

<!---->

      (setq-local text-mode-variant t)
      (setq-local require-final-newline mode-require-final-newline))

The three Lisp modes (Lisp mode, Emacs Lisp mode, and Lisp Interaction mode) have more features than Text mode and the code is correspondingly more complicated. Here are excerpts from `lisp-mode.el` that illustrate how these modes are written.

Here is how the Lisp mode syntax and abbrev tables are defined:

    ;; Create mode-specific table variables.
    (define-abbrev-table 'lisp-mode-abbrev-table ()
      "Abbrev table for Lisp mode.")

    (defvar lisp-mode-syntax-table
      (let ((table (make-syntax-table lisp--mode-syntax-table)))
        (modify-syntax-entry ?\[ "_   " table)
        (modify-syntax-entry ?\] "_   " table)
        (modify-syntax-entry ?# "' 14" table)
        (modify-syntax-entry ?| "\" 23bn" table)
        table)
      "Syntax table used in `lisp-mode'.")

The three modes for Lisp share much of their code. For instance, each calls the following function to set various variables:

    (defun lisp-mode-variables (&optional syntax keywords-case-insensitive elisp)
      (when syntax
        (set-syntax-table lisp-mode-syntax-table))
      …

Amongst other things, this function sets up the `comment-start` variable to handle Lisp comments:

      (setq-local comment-start ";")
      …

Each of the different Lisp modes has a slightly different keymap. For example, Lisp mode binds `C-c C-z` to `run-lisp`, but the other Lisp modes do not. However, all Lisp modes have some commands in common. The following code sets up the common commands:

    (defvar lisp-mode-shared-map
      (let ((map (make-sparse-keymap)))
        (set-keymap-parent map prog-mode-map)
        (define-key map "\e\C-q" 'indent-sexp)
        (define-key map "\177" 'backward-delete-char-untabify)
        map)
      "Keymap for commands shared by all sorts of Lisp modes.")

And here is the code to set up the keymap for Lisp mode:

    (defvar lisp-mode-map
      (let ((map (make-sparse-keymap))
            (menu-map (make-sparse-keymap "Lisp")))
        (set-keymap-parent map lisp-mode-shared-map)
        (define-key map "\e\C-x" 'lisp-eval-defun)
        (define-key map "\C-c\C-z" 'run-lisp)
        …
        map)
      "Keymap for ordinary Lisp mode.
    All commands in `lisp-mode-shared-map' are inherited by this map.")

Finally, here is the major mode command for Lisp mode:

    (define-derived-mode lisp-mode prog-mode "Lisp"
      "Major mode for editing Lisp code for Lisps other than GNU Emacs Lisp.
    Commands:
    Delete converts tabs to spaces as it moves back.
    Blank lines separate paragraphs.  Semicolons start comments.

    \\{lisp-mode-map}
    Note that `run-lisp' may be used either to start an inferior Lisp job
    or to switch back to an existing one."

<!---->

      (lisp-mode-variables nil t)
      (setq-local find-tag-default-function 'lisp-find-tag-default)
      (setq-local comment-start-skip
                  "\\(\\(^\\|[^\\\\\n]\\)\\(\\\\\\\\\\)*\\)\\(;+\\|#|\\) *")
      (setq imenu-case-fold-search t))

Previous: [Generic Modes](Generic-Modes.html), Up: [Major Modes](Major-Modes.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
