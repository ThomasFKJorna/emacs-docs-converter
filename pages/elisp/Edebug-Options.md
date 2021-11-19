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

Previous: [Edebug and Macros](Edebug-and-Macros.html), Up: [Edebug](Edebug.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 18.2.16 Edebug Options

These options affect the behavior of Edebug:

*   User Option: **edebug-setup-hook**

    Functions to call before Edebug is used. Each time it is set to a new value, Edebug will call those functions once and then reset `edebug-setup-hook` to `nil`. You could use this to load up Edebug specifications associated with a package you are using, but only when you also use Edebug. See [Instrumenting](Instrumenting.html).

<!---->

*   User Option: **edebug-all-defs**

    If this is non-`nil`, normal evaluation of defining forms such as `defun` and `defmacro` instruments them for Edebug. This applies to `eval-defun`, `eval-region`, `eval-buffer`, and `eval-current-buffer`.

    Use the command `M-x edebug-all-defs` to toggle the value of this option. See [Instrumenting](Instrumenting.html).

<!---->

*   User Option: **edebug-all-forms**

    If this is non-`nil`, the commands `eval-defun`, `eval-region`, `eval-buffer`, and `eval-current-buffer` instrument all forms, even those that don’t define anything. This doesn’t apply to loading or evaluations in the minibuffer.

    Use the command `M-x edebug-all-forms` to toggle the value of this option. See [Instrumenting](Instrumenting.html).

<!---->

*   User Option: **edebug-eval-macro-args**

    When this is non-`nil`, all macro arguments will be instrumented in the generated code. For any macro, an `edebug-form-spec` overrides this option. So to specify exceptions for macros that have some arguments evaluated and some not, use `def-edebug-spec` to specify an `edebug-form-spec`.

<!---->

*   User Option: **edebug-save-windows**

    If this is non-`nil`, Edebug saves and restores the window configuration. That takes some time, so if your program does not care what happens to the window configurations, it is better to set this variable to `nil`.

    If the value is a list, only the listed windows are saved and restored.

    You can use the `W` command in Edebug to change this variable interactively. See [Edebug Display Update](Edebug-Display-Update.html).

<!---->

*   User Option: **edebug-save-displayed-buffer-points**

    If this is non-`nil`, Edebug saves and restores point in all displayed buffers.

    Saving and restoring point in other buffers is necessary if you are debugging code that changes the point of a buffer that is displayed in a non-selected window. If Edebug or the user then selects the window, point in that buffer will move to the window’s value of point.

    Saving and restoring point in all buffers is expensive, since it requires selecting each window twice, so enable this only if you need it. See [Edebug Display Update](Edebug-Display-Update.html).

<!---->

*   User Option: **edebug-initial-mode**

    If this variable is non-`nil`, it specifies the initial execution mode for Edebug when it is first activated. Possible values are `step`, `next`, `go`, `Go-nonstop`, `trace`, `Trace-fast`, `continue`, and `Continue-fast`.

    The default value is `step`. This variable can be set interactively with `C-x C-a C-m` (`edebug-set-initial-mode`). See [Edebug Execution Modes](Edebug-Execution-Modes.html).

<!---->

*   User Option: **edebug-trace**

    If this is non-`nil`, trace each function entry and exit. Tracing output is displayed in a buffer named `*edebug-trace*`, one function entry or exit per line, indented by the recursion level.

    Also see `edebug-tracing`, in [Trace Buffer](Trace-Buffer.html).

<!---->

*   User Option: **edebug-test-coverage**

    If non-`nil`, Edebug tests coverage of all expressions debugged. See [Coverage Testing](Coverage-Testing.html).

<!---->

*   User Option: **edebug-continue-kbd-macro**

    If non-`nil`, continue defining or executing any keyboard macro that is executing outside of Edebug. Use this with caution since it is not debugged. See [Edebug Execution Modes](Edebug-Execution-Modes.html).

<!---->

*   User Option: **edebug-print-length**

    If non-`nil`, the default value of `print-length` for printing results in Edebug. See [Output Variables](Output-Variables.html).

<!---->

*   User Option: **edebug-print-level**

    If non-`nil`, the default value of `print-level` for printing results in Edebug. See [Output Variables](Output-Variables.html).

<!---->

*   User Option: **edebug-print-circle**

    If non-`nil`, the default value of `print-circle` for printing results in Edebug. See [Output Variables](Output-Variables.html).

<!---->

*   User Option: **edebug-unwrap-results**

    If non-`nil`, Edebug tries to remove any of its own instrumentation when showing the results of expressions. This is relevant when debugging macros where the results of expressions are themselves instrumented expressions. As a very artificial example, suppose that the example function `fac` has been instrumented, and consider a macro of the form:

        (defmacro test () "Edebug example."
          (if (symbol-function 'fac)
              …))

    If you instrument the `test` macro and step through it, then by default the result of the `symbol-function` call has numerous `edebug-after` and `edebug-before` forms, which can make it difficult to see the actual result. If `edebug-unwrap-results` is non-`nil`, Edebug tries to remove these forms from the result.

<!---->

*   User Option: **edebug-on-error**

    Edebug binds `debug-on-error` to this value, if `debug-on-error` was previously `nil`. See [Trapping Errors](Trapping-Errors.html).

<!---->

*   User Option: **edebug-on-quit**

    Edebug binds `debug-on-quit` to this value, if `debug-on-quit` was previously `nil`. See [Trapping Errors](Trapping-Errors.html).

If you change the values of `edebug-on-error` or `edebug-on-quit` while Edebug is active, their values won’t be used until the *next* time Edebug is invoked via a new command.

*   User Option: **edebug-global-break-condition**

    If non-`nil`, an expression to test for at every stop point. If the result is non-`nil`, then break. Errors are ignored. See [Global Break Condition](Global-Break-Condition.html).

<!---->

*   User Option: **edebug-sit-for-seconds**

    Number of seconds to pause when a breakpoint is reached and the execution mode is trace or continue. See [Edebug Execution Modes](Edebug-Execution-Modes.html).

<!---->

*   User Option: **edebug-sit-on-break**

    Whether or not to pause for `edebug-sit-for-seconds` on reaching a breakpoint. Set to `nil` to prevent the pause, non-`nil` to allow it.

<!---->

*   User Option: **edebug-behavior-alist**

    By default, this alist contains one entry with the key `edebug` and a list of three functions, which are the default implementations of the functions inserted in instrumented code: `edebug-enter`, `edebug-before` and `edebug-after`. To change Edebug’s behavior globally, modify the default entry.

    Edebug’s behavior may also be changed on a per-definition basis by adding an entry to this alist, with a key of your choice and three functions. Then set the `edebug-behavior` symbol property of an instrumented definition to the key of the new entry, and Edebug will call the new functions in place of its own for that definition.

<!---->

*   User Option: **edebug-new-definition-function**

    A function run by Edebug after it wraps the body of a definition or closure. After Edebug has initialized its own data, this function is called with one argument, the symbol associated with the definition, which may be the actual symbol defined or one generated by Edebug. This function may be used to set the `edebug-behavior` symbol property of each definition instrumented by Edebug.

<!---->

*   User Option: **edebug-after-instrumentation-function**

    To inspect or modify Edebug’s instrumentation before it is used, set this variable to a function which takes one argument, an instrumented top-level form, and returns either the same or a replacement form, which Edebug will then use as the final result of instrumentation.

Previous: [Edebug and Macros](Edebug-and-Macros.html), Up: [Edebug](Edebug.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
