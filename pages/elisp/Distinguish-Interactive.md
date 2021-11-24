

### 21.4 Distinguish Interactive Calls

Sometimes a command should display additional visual feedback (such as an informative message in the echo area) for interactive calls only. There are three ways to do this. The recommended way to test whether the function was called using `call-interactively` is to give it an optional argument `print-message` and use the `interactive` spec to make it non-`nil` in interactive calls. Here’s an example:

```lisp
(defun foo (&optional print-message)
  (interactive "p")
  (when print-message
    (message "foo")))
```

We use `"p"` because the numeric prefix argument is never `nil`. Defined in this way, the function does display the message when called from a keyboard macro.

The above method with the additional argument is usually best, because it allows callers to say “treat this call as interactive”. But you can also do the job by testing `called-interactively-p`.

### Function: **called-interactively-p** *kind*

This function returns `t` when the calling function was called using `call-interactively`.

The argument `kind` should be either the symbol `interactive` or the symbol `any`. If it is `interactive`, then `called-interactively-p` returns `t` only if the call was made directly by the user—e.g., if the user typed a key sequence bound to the calling function, but *not* if the user ran a keyboard macro that called the function (see [Keyboard Macros](Keyboard-Macros.html)). If `kind` is `any`, `called-interactively-p` returns `t` for any kind of interactive call, including keyboard macros.

If in doubt, use `any`; the only known proper use of `interactive` is if you need to decide whether to display a helpful message while a function is running.

A function is never considered to be called interactively if it was called via Lisp evaluation (or with `apply` or `funcall`).

Here is an example of using `called-interactively-p`:

```lisp
(defun foo ()
  (interactive)
  (when (called-interactively-p 'any)
    (message "Interactive!")
    'foo-called-interactively))
```

```lisp
```

```lisp
;; Type M-x foo.
     -| Interactive!
```

```lisp
```

```lisp
(foo)
     ⇒ nil
```

Here is another example that contrasts direct and indirect calls to `called-interactively-p`.

```lisp
(defun bar ()
  (interactive)
  (message "%s" (list (foo) (called-interactively-p 'any))))
```

```lisp
```

```lisp
;; Type M-x bar.
     -| (nil t)
```
