

#### 18.1.10 Internals of the Debugger

This section describes functions and variables used internally by the debugger.

### Variable: **debugger**

The value of this variable is the function to call to invoke the debugger. Its value must be a function of any number of arguments, or, more typically, the name of a function. This function should invoke some kind of debugger. The default value of the variable is `debug`.

The first argument that Lisp hands to the function indicates why it was called. The convention for arguments is detailed in the description of `debug` (see [Invoking the Debugger](Invoking-the-Debugger.html)).

### Function: **backtrace**

This function prints a trace of Lisp function calls currently active. The trace is identical to the one that `debug` would show in the `*Backtrace*` buffer. The return value is always nil.

In the following example, a Lisp expression calls `backtrace` explicitly. This prints the backtrace to the stream `standard-output`, which, in this case, is the buffer ‘`backtrace-output`’.

Each line of the backtrace represents one function call. The line shows the function followed by a list of the values of the function’s arguments if they are all known; if they are still being computed, the line consists of a list containing the function and its unevaluated arguments. Long lists or deeply nested structures may be elided.

```lisp
(with-output-to-temp-buffer "backtrace-output"
  (let ((var 1))
    (save-excursion
      (setq var (eval '(progn
                         (1+ var)
                         (list 'testing (backtrace))))))))

     ⇒ (testing nil)
```

```lisp
```

```lisp
----------- Buffer: backtrace-output ------------
  backtrace()
  (list 'testing (backtrace))
```

```lisp
  (progn ...)
  eval((progn (1+ var) (list 'testing (backtrace))))
  (setq ...)
  (save-excursion ...)
  (let ...)
  (with-output-to-temp-buffer ...)
  eval((with-output-to-temp-buffer ...))
  eval-last-sexp-1(nil)
```

```lisp
  eval-last-sexp(nil)
  call-interactively(eval-last-sexp)
----------- Buffer: backtrace-output ------------
```

### User Option: **debugger-stack-frame-as-list**

If this variable is non-`nil`, every stack frame of the backtrace is displayed as a list. This aims at improving the backtrace readability at the cost of special forms no longer being visually different from regular function calls.

With `debugger-stack-frame-as-list` non-`nil`, the above example would look as follows:

```lisp
----------- Buffer: backtrace-output ------------
  (backtrace)
  (list 'testing (backtrace))
```

```lisp
  (progn ...)
  (eval (progn (1+ var) (list 'testing (backtrace))))
  (setq ...)
  (save-excursion ...)
  (let ...)
  (with-output-to-temp-buffer ...)
  (eval (with-output-to-temp-buffer ...))
  (eval-last-sexp-1 nil)
```

```lisp
  (eval-last-sexp nil)
  (call-interactively eval-last-sexp)
----------- Buffer: backtrace-output ------------
```

### Variable: **debug-on-next-call**

If this variable is non-`nil`, it says to call the debugger before the next `eval`, `apply` or `funcall`. Entering the debugger sets `debug-on-next-call` to `nil`.

The `d` command in the debugger works by setting this variable.

### Function: **backtrace-debug** *level flag*

This function sets the debug-on-exit flag of the stack frame `level` levels down the stack, giving it the value `flag`. If `flag` is non-`nil`, this will cause the debugger to be entered when that frame later exits. Even a nonlocal exit through that frame will enter the debugger.

This function is used only by the debugger.

### Variable: **command-debug-status**

This variable records the debugging status of the current interactive command. Each time a command is called interactively, this variable is bound to `nil`. The debugger can set this variable to leave information for future debugger invocations during the same command invocation.

The advantage of using this variable rather than an ordinary global variable is that the data will never carry over to a subsequent command invocation.

This variable is obsolete and will be removed in future versions.

### Function: **backtrace-frame** *frame-number \&optional base*

The function `backtrace-frame` is intended for use in Lisp debuggers. It returns information about what computation is happening in the stack frame `frame-number` levels down.

If that frame has not evaluated the arguments yet, or is a special form, the value is `(nil function arg-forms…)`.

If that frame has evaluated its arguments and called its function already, the return value is `(t function arg-values…)`.

In the return value, `function` is whatever was supplied as the CAR of the evaluated list, or a `lambda` expression in the case of a macro call. If the function has a `&rest` argument, that is represented as the tail of the list `arg-values`.

If `base` is specified, `frame-number` counts relative to the topmost frame whose `function` is `base`.

If `frame-number` is out of range, `backtrace-frame` returns `nil`.

### Function: **mapbacktrace** *function \&optional base*

The function `mapbacktrace` calls `function` once for each frame in the backtrace, starting at the first frame whose function is `base` (or from the top if `base` is omitted or `nil`).

`function` is called with four arguments: `evald`, `func`, `args`, and `flags`.

If a frame has not evaluated its arguments yet or is a special form, `evald` is `nil` and `args` is a list of forms.

If a frame has evaluated its arguments and called its function already, `evald` is `t` and `args` is a list of values. `flags` is a plist of properties of the current frame: currently, the only supported property is `:debug-on-exit`, which is `t` if the stack frame’s `debug-on-exit` flag is set.
