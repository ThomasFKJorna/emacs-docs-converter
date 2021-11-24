

### 40.9 Processor Run time

Emacs provides several functions and primitives that return time, both elapsed and processor time, used by the Emacs process.

### Command: **emacs-uptime** *\&optional format*

This function returns a string representing the Emacs *uptime*—the elapsed wall-clock time this instance of Emacs is running. The string is formatted by `format-seconds` according to the optional argument `format`. For the available format descriptors, see [format-seconds](Time-Parsing.html). If `format` is `nil` or omitted, it defaults to `"%Y, %D, %H, %M, %z%S"`.

When called interactively, it prints the uptime in the echo area.

### Function: **get-internal-run-time**

This function returns the processor run time used by Emacs, as a Lisp timestamp (see [Time of Day](Time-of-Day.html)).

Note that the time returned by this function excludes the time Emacs was not using the processor, and if the Emacs process has several threads, the returned value is the sum of the processor times used up by all Emacs threads.

If the system doesn’t provide a way to determine the processor run time, `get-internal-run-time` returns the same time as `current-time`.

### Command: **emacs-init-time**

This function returns the duration of the Emacs initialization (see [Startup Summary](Startup-Summary.html)) in seconds, as a string. When called interactively, it prints the duration in the echo area.
