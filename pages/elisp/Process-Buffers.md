

#### 38.9.1 Process Buffers

A process can (and usually does) have an *associated buffer*, which is an ordinary Emacs buffer that is used for two purposes: storing the output from the process, and deciding when to kill the process. You can also use the buffer to identify a process to operate on, since in normal practice only one process is associated with any given buffer. Many applications of processes also use the buffer for editing input to be sent to the process, but this is not built into Emacs Lisp.

By default, process output is inserted in the associated buffer. (You can change this by defining a custom filter function, see [Filter Functions](Filter-Functions.html).) The position to insert the output is determined by the `process-mark`, which is then updated to point to the end of the text just inserted. Usually, but not always, the `process-mark` is at the end of the buffer.

Killing the associated buffer of a process also kills the process. Emacs asks for confirmation first, if the process’s `process-query-on-exit-flag` is non-`nil` (see [Query Before Exit](Query-Before-Exit.html)). This confirmation is done by the function `process-kill-buffer-query-function`, which is run from `kill-buffer-query-functions` (see [Killing Buffers](Killing-Buffers.html)).

### Function: **process-buffer** *process*

This function returns the associated buffer of the specified `process`.

```lisp
(process-buffer (get-process "shell"))
     ⇒ #<buffer *shell*>
```

### Function: **process-mark** *process*

This function returns the process marker for `process`, which is the marker that says where to insert output from the process.

If `process` does not have a buffer, `process-mark` returns a marker that points nowhere.

The default filter function uses this marker to decide where to insert process output, and updates it to point after the inserted text. That is why successive batches of output are inserted consecutively.

Custom filter functions normally should use this marker in the same fashion. For an example of a filter function that uses `process-mark`, see [Process Filter Example](Filter-Functions.html#Process-Filter-Example).

When the user is expected to enter input in the process buffer for transmission to the process, the process marker separates the new input from previous output.

### Function: **set-process-buffer** *process buffer*

This function sets the buffer associated with `process` to `buffer`. If `buffer` is `nil`, the process becomes associated with no buffer.

### Function: **get-buffer-process** *buffer-or-name*

This function returns a nondeleted process associated with the buffer specified by `buffer-or-name`. If there are several processes associated with it, this function chooses one (currently, the one most recently created, but don’t count on that). Deletion of a process (see `delete-process`) makes it ineligible for this function to return.

It is usually a bad idea to have more than one process associated with the same buffer.

```lisp
(get-buffer-process "*shell*")
     ⇒ #<process shell>
```

Killing the process’s buffer deletes the process, which kills the subprocess with a `SIGHUP` signal (see [Signals to Processes](Signals-to-Processes.html)).

If the process’s buffer is displayed in a window, your Lisp program may wish to tell the process the dimensions of that window, so that the process could adapt its output to those dimensions, much as it adapts to the screen dimensions. The following functions allow communicating this kind of information to processes; however, not all systems support the underlying functionality, so it is best to provide fallbacks, e.g., via command-line arguments or environment variables.

### Function: **set-process-window-size** *process height width*

Tell `process` that its logical window size has dimensions `width` by `height`, in character units. If this function succeeds in communicating this information to the process, it returns `t`; otherwise it returns `nil`.

When windows that display buffers associated with process change their dimensions, the affected processes should be told about these changes. By default, when the window configuration changes, Emacs will automatically call `set-process-window-size` on behalf of every process whose buffer is displayed in a window, passing it the smallest dimensions of all the windows displaying the process’s buffer. This works via `window-configuration-change-hook` (see [Window Hooks](Window-Hooks.html)), which is told to invoke the function that is the value of the variable `window-adjust-process-window-size-function` for each process whose buffer is displayed in at least one window. You can customize this behavior by setting the value of that variable.

### User Option: **window-adjust-process-window-size-function**

The value of this variable should be a function of two arguments: a process and the list of windows displaying the process’s buffer. When the function is called, the process’s buffer is the current buffer. The function should return a cons cell `(width . height)` that describes the dimensions of the logical process window to be passed via a call to `set-process-window-size`. The function can also return `nil`, in which case Emacs will not call `set-process-window-size` for this process.

Emacs supplies two predefined values for this variable: `window-adjust-process-window-size-smallest`, which returns the smallest of all the dimensions of the windows that display a process’s buffer; and `window-adjust-process-window-size-largest`, which returns the largest dimensions. For more complex strategies, write your own function.

This variable can be buffer-local.

If the process has the `adjust-window-size-function` property (see [Process Information](Process-Information.html)), its value overrides the global and buffer-local values of `window-adjust-process-window-size-function`.
