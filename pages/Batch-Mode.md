

Next: [Session Management](Session-Management.html), Previous: [X11 Keysyms](X11-Keysyms.html), Up: [System Interface](System-Interface.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 40.17 Batch Mode

The command-line option ‘`-batch`’ causes Emacs to run noninteractively. In this mode, Emacs does not read commands from the terminal, it does not alter the terminal modes, and it does not expect to be outputting to an erasable screen. The idea is that you specify Lisp programs to run; when they are finished, Emacs should exit. The way to specify the programs to run is with ‘`-l file`’, which loads the library named `file`, or ‘`-f function`’, which calls `function` with no arguments, or ‘`--eval=form`’.

Any Lisp program output that would normally go to the echo area, either using `message`, or using `prin1`, etc., with `t` as the stream (see [Output Streams](Output-Streams.html)), goes instead to Emacs’s standard descriptors when in batch mode: `message` writes to the standard error descriptor, while `prin1` and other print functions write to the standard output. Similarly, input that would normally come from the minibuffer is read from the standard input descriptor. Thus, Emacs behaves much like a noninteractive application program. (The echo area output that Emacs itself normally generates, such as command echoing, is suppressed entirely.)

Non-ASCII text written to the standard output or error descriptors is by default encoded using `locale-coding-system` (see [Locales](Locales.html)) if it is non-`nil`; this can be overridden by binding `coding-system-for-write` to a coding system of you choice (see [Explicit Encoding](Explicit-Encoding.html)).

*   Variable: **noninteractive**

    This variable is non-`nil` when Emacs is running in batch mode.

If Emacs exits due to signaling an error in batch mode, the exit status of the Emacs command is non-zero:

```lisp
$ emacs -Q --batch --eval '(error "foo")'; echo $?
foo
255
```

Next: [Session Management](Session-Management.html), Previous: [X11 Keysyms](X11-Keysyms.html), Up: [System Interface](System-Interface.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
