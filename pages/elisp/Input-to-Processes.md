

### 38.7 Sending Input to Processes

Asynchronous subprocesses receive input when it is sent to them by Emacs, which is done with the functions in this section. You must specify the process to send input to, and the input data to send. If the subprocess runs a program, the data appears on the standard input of that program; for connections, the data is sent to the connected device or program.

Some operating systems have limited space for buffered input in a pty. On these systems, Emacs sends an EOF periodically amidst the other characters, to force them through. For most programs, these EOFs do no harm.

Subprocess input is normally encoded using a coding system before the subprocess receives it, much like text written into a file. You can use `set-process-coding-system` to specify which coding system to use (see [Process Information](Process-Information.html)). Otherwise, the coding system comes from `coding-system-for-write`, if that is non-`nil`; or else from the defaulting mechanism (see [Default Coding Systems](Default-Coding-Systems.html)).

Sometimes the system is unable to accept input for that process, because the input buffer is full. When this happens, the send functions wait a short while, accepting output from subprocesses, and then try again. This gives the subprocess a chance to read more of its pending input and make space in the buffer. It also allows filters (including the one currently running), sentinels and timers to run—so take account of that in writing your code.

In these functions, the `process` argument can be a process or the name of a process, or a buffer or buffer name (which stands for a process via `get-buffer-process`). `nil` means the current buffer’s process.

### Function: **process-send-string** *process string*

This function sends `process` the contents of `string` as standard input. It returns `nil`. For example, to make a Shell buffer list files:

```lisp
(process-send-string "shell<1>" "ls\n")
     ⇒ nil
```

### Function: **process-send-region** *process start end*

This function sends the text in the region defined by `start` and `end` as standard input to `process`.

An error is signaled unless both `start` and `end` are integers or markers that indicate positions in the current buffer. (It is unimportant which number is larger.)

### Function: **process-send-eof** *\&optional process*

This function makes `process` see an end-of-file in its input. The EOF comes after any text already sent to it. The function returns `process`.

```lisp
(process-send-eof "shell")
     ⇒ "shell"
```

### Function: **process-running-child-p** *\&optional process*

This function will tell you whether a `process`, which must not be a connection but a real subprocess, has given control of its terminal to a child process of its own. If this is true, the function returns the numeric ID of the foreground process group of `process`; it returns `nil` if Emacs can be certain that this is not so. The value is `t` if Emacs cannot tell whether this is true. This function signals an error if `process` is a network, serial, or pipe connection, or is the subprocess is not active.
