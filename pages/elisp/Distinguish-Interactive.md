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

Next: [Command Loop Info](Command-Loop-Info.html), Previous: [Interactive Call](Interactive-Call.html), Up: [Command Loop](Command-Loop.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 21.4 Distinguish Interactive Calls

Sometimes a command should display additional visual feedback (such as an informative message in the echo area) for interactive calls only. There are three ways to do this. The recommended way to test whether the function was called using `call-interactively` is to give it an optional argument `print-message` and use the `interactive` spec to make it non-`nil` in interactive calls. Here’s an example:

    (defun foo (&optional print-message)
      (interactive "p")
      (when print-message
        (message "foo")))

We use `"p"` because the numeric prefix argument is never `nil`. Defined in this way, the function does display the message when called from a keyboard macro.

The above method with the additional argument is usually best, because it allows callers to say “treat this call as interactive”. But you can also do the job by testing `called-interactively-p`.

*   Function: **called-interactively-p** *kind*

    This function returns `t` when the calling function was called using `call-interactively`.

    The argument `kind` should be either the symbol `interactive` or the symbol `any`. If it is `interactive`, then `called-interactively-p` returns `t` only if the call was made directly by the user—e.g., if the user typed a key sequence bound to the calling function, but *not* if the user ran a keyboard macro that called the function (see [Keyboard Macros](Keyboard-Macros.html)). If `kind` is `any`, `called-interactively-p` returns `t` for any kind of interactive call, including keyboard macros.

    If in doubt, use `any`; the only known proper use of `interactive` is if you need to decide whether to display a helpful message while a function is running.

    A function is never considered to be called interactively if it was called via Lisp evaluation (or with `apply` or `funcall`).

Here is an example of using `called-interactively-p`:

    (defun foo ()
      (interactive)
      (when (called-interactively-p 'any)
        (message "Interactive!")
        'foo-called-interactively))

```
```

    ;; Type M-x foo.
         -| Interactive!

```
```

    (foo)
         ⇒ nil

Here is another example that contrasts direct and indirect calls to `called-interactively-p`.

    (defun bar ()
      (interactive)
      (message "%s" (list (foo) (called-interactively-p 'any))))

```
```

    ;; Type M-x bar.
         -| (nil t)

Next: [Command Loop Info](Command-Loop-Info.html), Previous: [Interactive Call](Interactive-Call.html), Up: [Command Loop](Command-Loop.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
