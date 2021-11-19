

Next: [Decoding Output](Decoding-Output.html), Previous: [Process Buffers](Process-Buffers.html), Up: [Output from Processes](Output-from-Processes.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 38.9.2 Process Filter Functions

A process *filter function* is a function that receives the standard output from the associated process. *All* output from that process is passed to the filter. The default filter simply outputs directly to the process buffer.

By default, the error output from the process, if any, is also passed to the filter function, unless the destination for the standard error stream of the process was separated from the standard output when the process was created. Emacs will only call the filter function during certain function calls. See [Output from Processes](Output-from-Processes.html). Note that if any of those functions are called by the filter, the filter may be called recursively.

A filter function must accept two arguments: the associated process and a string, which is output just received from it. The function is then free to do whatever it chooses with the output.

Quitting is normally inhibited within a filter function—otherwise, the effect of typing `C-g` at command level or to quit a user command would be unpredictable. If you want to permit quitting inside a filter function, bind `inhibit-quit` to `nil`. In most cases, the right way to do this is with the macro `with-local-quit`. See [Quitting](Quitting.html).

If an error happens during execution of a filter function, it is caught automatically, so that it doesn’t stop the execution of whatever program was running when the filter function was started. However, if `debug-on-error` is non-`nil`, errors are not caught. This makes it possible to use the Lisp debugger to debug filter functions. See [Debugger](Debugger.html).

Many filter functions sometimes (or always) insert the output in the process’s buffer, mimicking the actions of the default filter. Such filter functions need to make sure that they save the current buffer, select the correct buffer (if different) before inserting output, and then restore the original buffer. They should also check whether the buffer is still alive, update the process marker, and in some cases update the value of point. Here is how to do these things:

```lisp
(defun ordinary-insertion-filter (proc string)
  (when (buffer-live-p (process-buffer proc))
    (with-current-buffer (process-buffer proc)
      (let ((moving (= (point) (process-mark proc))))
```

```lisp
        (save-excursion
          ;; Insert the text, advancing the process marker.
          (goto-char (process-mark proc))
          (insert string)
          (set-marker (process-mark proc) (point)))
        (if moving (goto-char (process-mark proc)))))))
```

To make the filter force the process buffer to be visible whenever new text arrives, you could insert a line like the following just before the `with-current-buffer` construct:

```lisp
(display-buffer (process-buffer proc))
```

To force point to the end of the new output, no matter where it was previously, eliminate the variable `moving` from the example and call `goto-char` unconditionally. Note that this doesn’t necessarily move the window point. The default filter actually uses `insert-before-markers` which moves all markers, including the window point. This may move unrelated markers, so it’s generally better to move the window point explicitly, or set its insertion type to `t` (see [Window Point](Window-Point.html)).

Note that Emacs automatically saves and restores the match data while executing filter functions. See [Match Data](Match-Data.html).

The output to the filter may come in chunks of any size. A program that produces the same output twice in a row may send it as one batch of 200 characters one time, and five batches of 40 characters the next. If the filter looks for certain text strings in the subprocess output, make sure to handle the case where one of these strings is split across two or more batches of output; one way to do this is to insert the received text into a temporary buffer, which can then be searched.

*   Function: **set-process-filter** *process filter*

    This function gives `process` the filter function `filter`. If `filter` is `nil`, it gives the process the default filter, which inserts the process output into the process buffer.

<!---->

*   Function: **process-filter** *process*

    This function returns the filter function of `process`.

In case the process’s output needs to be passed to several filters, you can use `add-function` to combine an existing filter with a new one. See [Advising Functions](Advising-Functions.html).

Here is an example of the use of a filter function:

```lisp
(defun keep-output (process output)
   (setq kept (cons output kept)))
     ⇒ keep-output
```

```lisp
(setq kept nil)
     ⇒ nil
```

```lisp
(set-process-filter (get-process "shell") 'keep-output)
     ⇒ keep-output
```

```lisp
(process-send-string "shell" "ls ~/other\n")
     ⇒ nil
kept
     ⇒ ("lewis@slug:$ "
```

```lisp
"FINAL-W87-SHORT.MSS    backup.otl              kolstad.mss~
address.txt             backup.psf              kolstad.psf
backup.bib~             david.mss               resume-Dec-86.mss~
backup.err              david.psf               resume-Dec.psf
backup.mss              dland                   syllabus.mss
"
"#backups.mss#          backup.mss~             kolstad.mss
")
```

Next: [Decoding Output](Decoding-Output.html), Previous: [Process Buffers](Process-Buffers.html), Up: [Output from Processes](Output-from-Processes.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
