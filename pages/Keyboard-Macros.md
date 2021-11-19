

Previous: [Command History](Command-History.html), Up: [Command Loop](Command-Loop.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 21.16 Keyboard Macros

A *keyboard macro* is a canned sequence of input events that can be considered a command and made the definition of a key. The Lisp representation of a keyboard macro is a string or vector containing the events. Don’t confuse keyboard macros with Lisp macros (see [Macros](Macros.html)).

*   Function: **execute-kbd-macro** *kbdmacro \&optional count loopfunc*

    This function executes `kbdmacro` as a sequence of events. If `kbdmacro` is a string or vector, then the events in it are executed exactly as if they had been input by the user. The sequence is *not* expected to be a single key sequence; normally a keyboard macro definition consists of several key sequences concatenated.

    If `kbdmacro` is a symbol, then its function definition is used in place of `kbdmacro`. If that is another symbol, this process repeats. Eventually the result should be a string or vector. If the result is not a symbol, string, or vector, an error is signaled.

    The argument `count` is a repeat count; `kbdmacro` is executed that many times. If `count` is omitted or `nil`, `kbdmacro` is executed once. If it is 0, `kbdmacro` is executed over and over until it encounters an error or a failing search.

    If `loopfunc` is non-`nil`, it is a function that is called, without arguments, prior to each iteration of the macro. If `loopfunc` returns `nil`, then this stops execution of the macro.

    See [Reading One Event](Reading-One-Event.html), for an example of using `execute-kbd-macro`.

<!---->

*   Variable: **executing-kbd-macro**

    This variable contains the string or vector that defines the keyboard macro that is currently executing. It is `nil` if no macro is currently executing. A command can test this variable so as to behave differently when run from an executing macro. Do not set this variable yourself.

<!---->

*   Variable: **defining-kbd-macro**

    This variable is non-`nil` if and only if a keyboard macro is being defined. A command can test this variable so as to behave differently while a macro is being defined. The value is `append` while appending to the definition of an existing macro. The commands `start-kbd-macro`, `kmacro-start-macro` and `end-kbd-macro` set this variable—do not set it yourself.

    The variable is always local to the current terminal and cannot be buffer-local. See [Multiple Terminals](Multiple-Terminals.html).

<!---->

*   Variable: **last-kbd-macro**

    This variable is the definition of the most recently defined keyboard macro. Its value is a string or vector, or `nil`.

    The variable is always local to the current terminal and cannot be buffer-local. See [Multiple Terminals](Multiple-Terminals.html).

<!---->

*   Variable: **kbd-macro-termination-hook**

    This normal hook is run when a keyboard macro terminates, regardless of what caused it to terminate (reaching the macro end or an error which ended the macro prematurely).

Previous: [Command History](Command-History.html), Up: [Command Loop](Command-Loop.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
