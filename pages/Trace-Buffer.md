

Next: [Coverage Testing](Coverage-Testing.html), Previous: [Printing in Edebug](Printing-in-Edebug.html), Up: [Edebug](Edebug.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 18.2.12 Trace Buffer

Edebug can record an execution trace, storing it in a buffer named `*edebug-trace*`. This is a log of function calls and returns, showing the function names and their arguments and values. To enable trace recording, set `edebug-trace` to a non-`nil` value.

Making a trace buffer is not the same thing as using trace execution mode (see [Edebug Execution Modes](Edebug-Execution-Modes.html)).

When trace recording is enabled, each function entry and exit adds lines to the trace buffer. A function entry record consists of ‘`::::{`’, followed by the function name and argument values. A function exit record consists of ‘`::::}`’, followed by the function name and result of the function.

The number of ‘`:`’s in an entry shows its recursion depth. You can use the braces in the trace buffer to find the matching beginning or end of function calls.

You can customize trace recording for function entry and exit by redefining the functions `edebug-print-trace-before` and `edebug-print-trace-after`.

*   Macro: **edebug-tracing** *string body…*

    This macro requests additional trace information around the execution of the `body` forms. The argument `string` specifies text to put in the trace buffer, after the ‘`{`’ or ‘`}`’. All the arguments are evaluated, and `edebug-tracing` returns the value of the last form in `body`.

<!---->

*   Function: **edebug-trace** *format-string \&rest format-args*

    This function inserts text in the trace buffer. It computes the text with `(apply 'format format-string format-args)`. It also appends a newline to separate entries.

`edebug-tracing` and `edebug-trace` insert lines in the trace buffer whenever they are called, even if Edebug is not active. Adding text to the trace buffer also scrolls its window to show the last lines inserted.

Next: [Coverage Testing](Coverage-Testing.html), Previous: [Printing in Edebug](Printing-in-Edebug.html), Up: [Edebug](Edebug.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
