

Next: [Deleting Processes](Deleting-Processes.html), Previous: [Synchronous Processes](Synchronous-Processes.html), Up: [Processes](Processes.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 38.4 Creating an Asynchronous Process

In this section, we describe how to create an *asynchronous process*. After an asynchronous process is created, it runs in parallel with Emacs, and Emacs can communicate with it using the functions described in the following sections (see [Input to Processes](Input-to-Processes.html), and see [Output from Processes](Output-from-Processes.html)). Note that process communication is only partially asynchronous: Emacs sends and receives data to and from a process only when those functions are called.

An asynchronous process is controlled either via a *pty* (pseudo-terminal) or a *pipe*. The choice of pty or pipe is made when creating the process, by default based on the value of the variable `process-connection-type` (see below). If available, ptys are usually preferable for processes visible to the user, as in Shell mode, because they allow for job control (`C-c`, `C-z`, etc.) between the process and its children, and because interactive programs treat ptys as terminal devices, whereas pipes don’t support these features. However, for subprocesses used by Lisp programs for internal purposes (i.e., no user interaction with the subprocess is required), where significant amounts of data need to be exchanged between the subprocess and the Lisp program, it is often better to use a pipe, because pipes are more efficient. Also, the total number of ptys is limited on many systems, and it is good not to waste them unnecessarily.

*   Function: **make-process** *\&rest args*

    This function is the basic low-level primitive for starting asynchronous subprocesses. It returns a process object representing the subprocess. Compared to the more high-level `start-process`, described below, it takes keyword arguments, is more flexible, and allows to specify process filters and sentinels in a single call.

    The arguments `args` are a list of keyword/argument pairs. Omitting a keyword is always equivalent to specifying it with value `nil`. Here are the meaningful keywords:

    *   :name `name`

        Use the string `name` as the process name; if a process with this name already exists, then `name` is modified (by appending ‘`<1>`’, etc.) to be unique.

    *   :buffer `buffer`

        Use `buffer` as the process buffer. If the value is `nil`, the subprocess is not associated with any buffer.

    *   :command `command`

        Use `command` as the command line of the process. The value should be a list starting with the program’s executable file name, followed by strings to give to the program as its arguments. If the first element of the list is `nil`, Emacs opens a new pseudoterminal (pty) and associates its input and output with `buffer`, without actually running any program; the rest of the list elements are ignored in that case.

    *   :coding `coding`

        If `coding` is a symbol, it specifies the coding system to be used for both reading and writing of data from and to the connection. If `coding` is a cons cell `(decoding . encoding)`, then `decoding` will be used for reading and `encoding` for writing. The coding system used for encoding the data written to the program is also used for encoding the command-line arguments (but not the program itself, whose file name is encoded as any other file name; see [file-name-coding-system](Encoding-and-I_002fO.html)).

        If `coding` is `nil`, the default rules for finding the coding system will apply. See [Default Coding Systems](Default-Coding-Systems.html).

    *   :connection-type `type`

        Initialize the type of device used to communicate with the subprocess. Possible values are `pty` to use a pty, `pipe` to use a pipe, or `nil` to use the default derived from the value of the `process-connection-type` variable. This parameter and the value of `process-connection-type` are ignored if a non-`nil` value is specified for the `:stderr` parameter; in that case, the type will always be `pipe`. On systems where ptys are not available (MS-Windows), this parameter is likewise ignored, and pipes are used unconditionally.

    *   :noquery `query-flag`

        Initialize the process query flag to `query-flag`. See [Query Before Exit](Query-Before-Exit.html).

    *   :stop `stopped`

        If provided, `stopped` must be `nil`; it is an error to use any non-`nil` value. The `:stop` key is ignored otherwise and is retained for compatibility with other process types such as pipe processes. Asynchronous subprocesses never start in the stopped state.

    *   :filter `filter`

        Initialize the process filter to `filter`. If not specified, a default filter will be provided, which can be overridden later. See [Filter Functions](Filter-Functions.html).

    *   :sentinel `sentinel`

        Initialize the process sentinel to `sentinel`. If not specified, a default sentinel will be used, which can be overridden later. See [Sentinels](Sentinels.html).

    *   :stderr `stderr`

        Associate `stderr` with the standard error of the process. A non-`nil` value should be either a buffer or a pipe process created with `make-pipe-process`, described below. If `stderr` is `nil`, standard error is mixed with standard output, and both are sent to `buffer` or `filter`.

        If `stderr` is a buffer, Emacs will create a pipe process, the *standard error process*. This process will have the default filter (see [Filter Functions](Filter-Functions.html)), sentinel (see [Sentinels](Sentinels.html)), and coding systems (see [Default Coding Systems](Default-Coding-Systems.html)). On the other hand, it will use `query-flag` as its query-on-exit flag (see [Query Before Exit](Query-Before-Exit.html)). It will be associated with the `stderr` buffer (see [Process Buffers](Process-Buffers.html)) and send its output (which is the standard error of the main process) there.

        If `stderr` is a pipe process, Emacs will use it as standard error process for the new process.

    *   :file-handler `file-handler`

        If `file-handler` is non-`nil`, then look for a file name handler for the current buffer’s `default-directory`, and invoke that file name handler to make the process. If there is no such handler, proceed as if `file-handler` were `nil`.

    The original argument list, modified with the actual connection information, is available via the `process-contact` function.

    The current working directory of the subprocess is set to the current buffer’s value of `default-directory` if that is local (as determined by `unhandled-file-name-directory`), or `~` otherwise. If you want to run a process in a remote directory, pass `:file-handler t` to `make-process`. In that case, the current working directory is the local name component of `default-directory` (as determined by `file-local-name`).

    Depending on the implementation of the file name handler, it might not be possible to apply `filter` or `sentinel` to the resulting process object. The `:stderr` argument cannot be a pipe process, file name handlers do not support pipe processes for this. A buffer as `:stderr` argument is accepted, its contents is shown without the use of pipe processes. See [Filter Functions](Filter-Functions.html), [Sentinels](Sentinels.html), and [Accepting Output](Accepting-Output.html).

    Some file name handlers may not support `make-process`. In such cases, this function does nothing and returns `nil`.

<!---->

*   Function: **make-pipe-process** *\&rest args*

    This function creates a bidirectional pipe which can be attached to a child process. This is useful with the `:stderr` keyword of `make-process`. The function returns a process object.

    The arguments `args` are a list of keyword/argument pairs. Omitting a keyword is always equivalent to specifying it with value `nil`.

    Here are the meaningful keywords:

    *   :name `name`

        Use the string `name` as the process name. As with `make-process`, it is modified if necessary to make it unique.

    *   :buffer `buffer`

        Use `buffer` as the process buffer.

    *   :coding `coding`

        If `coding` is a symbol, it specifies the coding system to be used for both reading and writing of data from and to the connection. If `coding` is a cons cell `(decoding . encoding)`, then `decoding` will be used for reading and `encoding` for writing.

        If `coding` is `nil`, the default rules for finding the coding system will apply. See [Default Coding Systems](Default-Coding-Systems.html).

    *   :noquery `query-flag`

        Initialize the process query flag to `query-flag`. See [Query Before Exit](Query-Before-Exit.html).

    *   :stop `stopped`

        If `stopped` is non-`nil`, start the process in the stopped state. In the stopped state, a pipe process does not accept incoming data, but you can send outgoing data. The stopped state is set by `stop-process` and cleared by `continue-process` (see [Signals to Processes](Signals-to-Processes.html)).

    *   :filter `filter`

        Initialize the process filter to `filter`. If not specified, a default filter will be provided, which can be changed later. See [Filter Functions](Filter-Functions.html).

    *   :sentinel `sentinel`

        Initialize the process sentinel to `sentinel`. If not specified, a default sentinel will be used, which can be changed later. See [Sentinels](Sentinels.html).

    The original argument list, modified with the actual connection information, is available via the `process-contact` function.

<!---->

*   Function: **start-process** *name buffer-or-name program \&rest args*

    This function is a higher-level wrapper around `make-process`, exposing an interface that is similar to `call-process`. It creates a new asynchronous subprocess and starts the specified `program` running in it. It returns a process object that stands for the new subprocess in Lisp. The argument `name` specifies the name for the process object; as with `make-process`, it is modified if necessary to make it unique. The buffer `buffer-or-name` is the buffer to associate with the process.

    If `program` is `nil`, Emacs opens a new pseudoterminal (pty) and associates its input and output with `buffer-or-name`, without creating a subprocess. In that case, the remaining arguments `args` are ignored.

    The rest of `args` are strings that specify command line arguments for the subprocess.

    In the example below, the first process is started and runs (rather, sleeps) for 100 seconds (the output buffer ‘`foo`’ is created immediately). Meanwhile, the second process is started, and given the name ‘`my-process<1>`’ for the sake of uniqueness. It inserts the directory listing at the end of the buffer ‘`foo`’, before the first process finishes. Then it finishes, and a message to that effect is inserted in the buffer. Much later, the first process finishes, and another message is inserted in the buffer for it.

    ```lisp
    (start-process "my-process" "foo" "sleep" "100")
         ⇒ #<process my-process>
    ```

    ```lisp
    ```

    ```lisp
    (start-process "my-process" "foo" "ls" "-l" "/bin")
         ⇒ #<process my-process<1>>

    ---------- Buffer: foo ----------
    total 8336
    -rwxr-xr-x 1 root root 971384 Mar 30 10:14 bash
    -rwxr-xr-x 1 root root 146920 Jul  5  2011 bsd-csh
    …
    -rwxr-xr-x 1 root root 696880 Feb 28 15:55 zsh4

    Process my-process<1> finished

    Process my-process finished
    ---------- Buffer: foo ----------
    ```

<!---->

*   Function: **start-file-process** *name buffer-or-name program \&rest args*

    Like `start-process`, this function starts a new asynchronous subprocess running `program` in it, and returns its process object.

    The difference from `start-process` is that this function may invoke a file name handler based on the value of `default-directory`. This handler ought to run `program`, perhaps on the local host, perhaps on a remote host that corresponds to `default-directory`. In the latter case, the local part of `default-directory` becomes the working directory of the process.

    This function does not try to invoke file name handlers for `program` or for the rest of `args`. For that reason, if `program` or any of `args` use the remote-file syntax (see [Magic File Names](Magic-File-Names.html)), they must be converted either to file names relative to `default-directory`, or to names that identify the files locally on the remote host, by running them through `file-local-name`.

    Depending on the implementation of the file name handler, it might not be possible to apply `process-filter` or `process-sentinel` to the resulting process object. See [Filter Functions](Filter-Functions.html), and [Sentinels](Sentinels.html).

    Some file name handlers may not support `start-file-process` (for example the function `ange-ftp-hook-function`). In such cases, this function does nothing and returns `nil`.

<!---->

*   Function: **start-process-shell-command** *name buffer-or-name command*

    This function is like `start-process`, except that it uses a shell to execute the specified `command`. The argument `command` is a shell command string. The variable `shell-file-name` specifies which shell to use.

    The point of running a program through the shell, rather than directly with `make-process` or `start-process`, is so that you can employ shell features such as wildcards in the arguments. It follows that if you include any arbitrary user-specified arguments in the command, you should quote them with `shell-quote-argument` first, so that any special shell characters do *not* have their special shell meanings. See [Shell Arguments](Shell-Arguments.html). Of course, when executing commands based on user input you should also consider the security implications.

<!---->

*   Function: **start-file-process-shell-command** *name buffer-or-name command*

    This function is like `start-process-shell-command`, but uses `start-file-process` internally. Because of this, `command` can also be executed on remote hosts, depending on `default-directory`.

<!---->

*   Variable: **process-connection-type**

    This variable controls the type of device used to communicate with asynchronous subprocesses. If it is non-`nil`, then ptys are used, when available. Otherwise, pipes are used.

    The value of `process-connection-type` takes effect when `make-process` or `start-process` is called. So you can specify how to communicate with one subprocess by binding the variable around the call to these functions.

    Note that the value of this variable is ignored when `make-process` is called with a non-`nil` value of the `:stderr` parameter; in that case, Emacs will communicate with the process using pipes. It is also ignored if ptys are unavailable (MS-Windows).

    ```lisp
    (let ((process-connection-type nil))  ; use a pipe
      (start-process …))
    ```

    To determine whether a given subprocess actually got a pipe or a pty, use the function `process-tty-name` (see [Process Information](Process-Information.html)).

Next: [Deleting Processes](Deleting-Processes.html), Previous: [Synchronous Processes](Synchronous-Processes.html), Up: [Processes](Processes.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
