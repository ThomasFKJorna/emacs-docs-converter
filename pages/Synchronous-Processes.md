

Next: [Asynchronous Processes](Asynchronous-Processes.html), Previous: [Shell Arguments](Shell-Arguments.html), Up: [Processes](Processes.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 38.3 Creating a Synchronous Process

After a *synchronous process* is created, Emacs waits for the process to terminate before continuing. Starting Dired on GNU or Unix[21](#FOOT21) is an example of this: it runs `ls` in a synchronous process, then modifies the output slightly. Because the process is synchronous, the entire directory listing arrives in the buffer before Emacs tries to do anything with it.

While Emacs waits for the synchronous subprocess to terminate, the user can quit by typing `C-g`. The first `C-g` tries to kill the subprocess with a `SIGINT` signal; but it waits until the subprocess actually terminates before quitting. If during that time the user types another `C-g`, that kills the subprocess instantly with `SIGKILL` and quits immediately (except on MS-DOS, where killing other processes doesn’t work). See [Quitting](Quitting.html).

The synchronous subprocess functions return an indication of how the process terminated.

The output from a synchronous subprocess is generally decoded using a coding system, much like text read from a file. The input sent to a subprocess by `call-process-region` is encoded using a coding system, much like text written into a file. See [Coding Systems](Coding-Systems.html).

*   Function: **call-process** *program \&optional infile destination display \&rest args*

    This function calls `program` and waits for it to finish.

    The current working directory of the subprocess is set to the current buffer’s value of `default-directory` if that is local (as determined by `unhandled-file-name-directory`), or "\~" otherwise. If you want to run a process in a remote directory use `process-file`.

    The standard input for the new process comes from file `infile` if `infile` is not `nil`, and from the null device otherwise. The argument `destination` says where to put the process output. Here are the possibilities:

    *   a buffer

        Insert the output in that buffer, before point. This includes both the standard output stream and the standard error stream of the process.

    *   a buffer name (a string)

        Insert the output in a buffer with that name, before point.

    *   `t`

        Insert the output in the current buffer, before point.

    *   `nil`

        Discard the output.

    *   0

        Discard the output, and return `nil` immediately without waiting for the subprocess to finish.

        In this case, the process is not truly synchronous, since it can run in parallel with Emacs; but you can think of it as synchronous in that Emacs is essentially finished with the subprocess as soon as this function returns.

        MS-DOS doesn’t support asynchronous subprocesses, so this option doesn’t work there.

    *   `(:file file-name)`

        Send the output to the file name specified, overwriting it if it already exists.

    *   `(real-destination error-destination)`

        Keep the standard output stream separate from the standard error stream; deal with the ordinary output as specified by `real-destination`, and dispose of the error output according to `error-destination`. If `error-destination` is `nil`, that means to discard the error output, `t` means mix it with the ordinary output, and a string specifies a file name to redirect error output into.

        You can’t directly specify a buffer to put the error output in; that is too difficult to implement. But you can achieve this result by sending the error output to a temporary file and then inserting the file into a buffer when the subprocess finishes.

    If `display` is non-`nil`, then `call-process` redisplays the buffer as output is inserted. (However, if the coding system chosen for decoding output is `undecided`, meaning deduce the encoding from the actual data, then redisplay sometimes cannot continue once non-ASCII characters are encountered. There are fundamental reasons why it is hard to fix this; see [Output from Processes](Output-from-Processes.html).)

    Otherwise the function `call-process` does no redisplay, and the results become visible on the screen only when Emacs redisplays that buffer in the normal course of events.

    The remaining arguments, `args`, are strings that specify command line arguments for the program. Each string is passed to `program` as a separate argument.

    The value returned by `call-process` (unless you told it not to wait) indicates the reason for process termination. A number gives the exit status of the subprocess; 0 means success, and any other value means failure. If the process terminated with a signal, `call-process` returns a string describing the signal. If you told `call-process` not to wait, it returns `nil`.

    In the examples below, the buffer ‘`foo`’ is current.

    ```lisp
    (call-process "pwd" nil t)
         ⇒ 0

    ---------- Buffer: foo ----------
    /home/lewis/manual
    ---------- Buffer: foo ----------
    ```

    ```lisp
    ```

    ```lisp
    (call-process "grep" nil "bar" nil "lewis" "/etc/passwd")
         ⇒ 0

    ---------- Buffer: bar ----------
    lewis:x:1001:1001:Bil Lewis,,,,:/home/lewis:/bin/bash

    ---------- Buffer: bar ----------
    ```

    Here is an example of the use of `call-process`, as used to be found in the definition of the `insert-directory` function:

    ```lisp
    (call-process insert-directory-program nil t nil switches
                  (if full-directory-p
                      (concat (file-name-as-directory file) ".")
                    file))
    ```

<!---->

*   Function: **process-file** *program \&optional infile buffer display \&rest args*

    This function processes files synchronously in a separate process. It is similar to `call-process`, but may invoke a file name handler based on the value of the variable `default-directory`, which specifies the current working directory of the subprocess.

    The arguments are handled in almost the same way as for `call-process`, with the following differences:

    Some file name handlers may not support all combinations and forms of the arguments `infile`, `buffer`, and `display`. For example, some file name handlers might behave as if `display` were `nil`, regardless of the value actually passed. As another example, some file name handlers might not support separating standard output and error output by way of the `buffer` argument.

    If a file name handler is invoked, it determines the program to run based on the first argument `program`. For instance, suppose that a handler for remote files is invoked. Then the path that is used for searching for the program might be different from `exec-path`.

    The second argument `infile` may invoke a file name handler. The file name handler could be different from the handler chosen for the `process-file` function itself. (For example, `default-directory` could be on one remote host, and `infile` on a different remote host. Or `default-directory` could be non-special, whereas `infile` is on a remote host.)

    If `buffer` is a list of the form `(real-destination error-destination)`, and `error-destination` names a file, then the same remarks as for `infile` apply.

    The remaining arguments (`args`) will be passed to the process verbatim. Emacs is not involved in processing file names that are present in `args`. To avoid confusion, it may be best to avoid absolute file names in `args`, but rather to specify all file names as relative to `default-directory`. The function `file-relative-name` is useful for constructing such relative file names. Alternatively, you can use `file-local-name` (see [Magic File Names](Magic-File-Names.html)) to obtain an absolute file name as seen from the remote host’s perspective.

<!---->

*   Variable: **process-file-side-effects**

    This variable indicates whether a call of `process-file` changes remote files.

    By default, this variable is always set to `t`, meaning that a call of `process-file` could potentially change any file on a remote host. When set to `nil`, a file name handler could optimize its behavior with respect to remote file attribute caching.

    You should only ever change this variable with a let-binding; never with `setq`.

<!---->

*   Function: **call-process-region** *start end program \&optional delete destination display \&rest args*

    This function sends the text from `start` to `end` as standard input to a process running `program`. It deletes the text sent if `delete` is non-`nil`; this is useful when `destination` is `t`, to insert the output in the current buffer in place of the input.

    The arguments `destination` and `display` control what to do with the output from the subprocess, and whether to update the display as it comes in. For details, see the description of `call-process`, above. If `destination` is the integer 0, `call-process-region` discards the output and returns `nil` immediately, without waiting for the subprocess to finish (this only works if asynchronous subprocesses are supported; i.e., not on MS-DOS).

    The remaining arguments, `args`, are strings that specify command line arguments for the program.

    The return value of `call-process-region` is just like that of `call-process`: `nil` if you told it to return without waiting; otherwise, a number or string which indicates how the subprocess terminated.

    In the following example, we use `call-process-region` to run the `cat` utility, with standard input being the first five characters in buffer ‘`foo`’ (the word ‘`input`’). `cat` copies its standard input into its standard output. Since the argument `destination` is `t`, this output is inserted in the current buffer.

    ```lisp
    ---------- Buffer: foo ----------
    input∗
    ---------- Buffer: foo ----------
    ```

    ```lisp
    ```

    ```lisp
    (call-process-region 1 6 "cat" nil t)
         ⇒ 0

    ---------- Buffer: foo ----------
    inputinput∗
    ---------- Buffer: foo ----------
    ```

    For example, the `shell-command-on-region` command uses `call-shell-region` in a manner similar to this:

    ```lisp
    (call-shell-region
     start end
     command              ; shell command
     nil                  ; do not delete region
     buffer)              ; send output to buffer
    ```

<!---->

*   Function: **call-process-shell-command** *command \&optional infile destination display*

    This function executes the shell command `command` synchronously. The other arguments are handled as in `call-process`. An old calling convention allowed passing any number of additional arguments after `display`, which were concatenated to `command`; this is still supported, but strongly discouraged.

<!---->

*   Function: **process-file-shell-command** *command \&optional infile destination display*

    This function is like `call-process-shell-command`, but uses `process-file` internally. Depending on `default-directory`, `command` can be executed also on remote hosts. An old calling convention allowed passing any number of additional arguments after `display`, which were concatenated to `command`; this is still supported, but strongly discouraged.

<!---->

*   Function: **call-shell-region** *start end command \&optional delete destination*

    This function sends the text from `start` to `end` as standard input to an inferior shell running `command`. This function is similar than `call-process-region`, with process being a shell. The arguments `delete`, `destination` and the return value are like in `call-process-region`. Note that this function doesn’t accept additional arguments.

<!---->

*   Function: **shell-command-to-string** *command*

    This function executes `command` (a string) as a shell command, then returns the command’s output as a string.

<!---->

*   Function: **process-lines** *program \&rest args*

    This function runs `program`, waits for it to finish, and returns its output as a list of strings. Each string in the list holds a single line of text output by the program; the end-of-line characters are stripped from each line. The arguments beyond `program`, `args`, are strings that specify command-line arguments with which to run the program.

    If `program` exits with a non-zero exit status, this function signals an error.

    This function works by calling `call-process`, so program output is decoded in the same way as for `call-process`.

***

#### Footnotes

##### [(21)](#DOCF21)

On other systems, Emacs uses a Lisp emulation of `ls`; see [Contents of Directories](Contents-of-Directories.html).

Next: [Asynchronous Processes](Asynchronous-Processes.html), Previous: [Shell Arguments](Shell-Arguments.html), Up: [Processes](Processes.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
