

Next: [Accepting Output](Accepting-Output.html), Previous: [Filter Functions](Filter-Functions.html), Up: [Output from Processes](Output-from-Processes.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 38.9.3 Decoding Process Output

When Emacs writes process output directly into a multibyte buffer, it decodes the output according to the process output coding system. If the coding system is `raw-text` or `no-conversion`, Emacs converts the unibyte output to multibyte using `string-to-multibyte`, and inserts the resulting multibyte text.

You can use `set-process-coding-system` to specify which coding system to use (see [Process Information](Process-Information.html)). Otherwise, the coding system comes from `coding-system-for-read`, if that is non-`nil`; or else from the defaulting mechanism (see [Default Coding Systems](Default-Coding-Systems.html)). If the text output by a process contains null bytes, Emacs by default uses `no-conversion` for it; see [inhibit-nul-byte-detection](Lisp-and-Coding-Systems.html), for how to control this behavior.

**Warning:** Coding systems such as `undecided`, which determine the coding system from the data, do not work entirely reliably with asynchronous subprocess output. This is because Emacs has to process asynchronous subprocess output in batches, as it arrives. Emacs must try to detect the proper coding system from one batch at a time, and this does not always work. Therefore, if at all possible, specify a coding system that determines both the character code conversion and the end of line conversion—that is, one like `latin-1-unix`, rather than `undecided` or `latin-1`.

When Emacs calls a process filter function, it provides the process output as a multibyte string or as a unibyte string according to the process’s filter coding system. Emacs decodes the output according to the process output coding system, which usually produces a multibyte string, except for coding systems such as `binary` and `raw-text`.

Next: [Accepting Output](Accepting-Output.html), Previous: [Filter Functions](Filter-Functions.html), Up: [Output from Processes](Output-from-Processes.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
