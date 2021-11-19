

Next: [Input to Processes](Input-to-Processes.html), Previous: [Deleting Processes](Deleting-Processes.html), Up: [Processes](Processes.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 38.6 Process Information

Several functions return information about processes.

*   Command: **list-processes** *\&optional query-only buffer*

    This command displays a listing of all living processes. In addition, it finally deletes any process whose status was ‘`Exited`’ or ‘`Signaled`’. It returns `nil`.

    The processes are shown in a buffer named `*Process List*` (unless you specify otherwise using the optional argument `buffer`), whose major mode is Process Menu mode.

    If `query-only` is non-`nil`, it only lists processes whose query flag is non-`nil`. See [Query Before Exit](Query-Before-Exit.html).

<!---->

*   Function: **process-list**

    This function returns a list of all processes that have not been deleted.

    ```lisp
    (process-list)
         ⇒ (#<process display-time> #<process shell>)
    ```

<!---->

*   Function: **get-process** *name*

    This function returns the process named `name` (a string), or `nil` if there is none. The argument `name` can also be a process object, in which case it is returned.

    ```lisp
    (get-process "shell")
         ⇒ #<process shell>
    ```

<!---->

*   Function: **process-command** *process*

    This function returns the command that was executed to start `process`. This is a list of strings, the first string being the program executed and the rest of the strings being the arguments that were given to the program. For a network, serial, or pipe connection, this is either `nil`, which means the process is running or `t` (process is stopped).

    ```lisp
    (process-command (get-process "shell"))
         ⇒ ("bash" "-i")
    ```

<!---->

*   Function: **process-contact** *process \&optional key no-block*

    This function returns information about how a network, a serial, or a pipe connection was set up. When `key` is `nil`, it returns `(hostname service)` for a network connection, `(port speed)` for a serial connection, and `t` for a pipe connection. For an ordinary child process, this function always returns `t` when called with a `nil` `key`.

    If `key` is `t`, the value is the complete status information for the connection, server, serial port, or pipe; that is, the list of keywords and values specified in `make-network-process`, `make-serial-process`, or `make-pipe-process`, except that some of the values represent the current status instead of what you specified.

    For a network process, the values include (see `make-network-process` for a complete list):

    *   `:buffer`

        The associated value is the process buffer.

    *   `:filter`

        The associated value is the process filter function. See [Filter Functions](Filter-Functions.html).

    *   `:sentinel`

        The associated value is the process sentinel function. See [Sentinels](Sentinels.html).

    *   `:remote`

        In a connection, the address in internal format of the remote peer.

    *   `:local`

        The local address, in internal format.

    *   `:service`

        In a server, if you specified `t` for `service`, this value is the actual port number.

    `:local` and `:remote` are included even if they were not specified explicitly in `make-network-process`.

    For a serial connection, see `make-serial-process` and `serial-process-configure` for the list of keys. For a pipe connection, see `make-pipe-process` for the list of keys.

    If `key` is a keyword, the function returns the value corresponding to that keyword.

    If `process` is a non-blocking network stream that hasn’t been fully set up yet, then this function will block until that has happened. If given the optional `no-block` parameter, this function will return `nil` instead of blocking.

<!---->

*   Function: **process-id** *process*

    This function returns the PID of `process`. This is an integral number that distinguishes the process `process` from all other processes running on the same computer at the current time. The PID of a process is chosen by the operating system kernel when the process is started and remains constant as long as the process exists. For network, serial, and pipe connections, this function returns `nil`.

<!---->

*   Function: **process-name** *process*

    This function returns the name of `process`, as a string.

<!---->

*   Function: **process-status** *process-name*

    This function returns the status of `process-name` as a symbol. The argument `process-name` must be a process, a buffer, or a process name (a string).

    The possible values for an actual subprocess are:

    *   `run`

        for a process that is running.

    *   `stop`

        for a process that is stopped but continuable.

    *   `exit`

        for a process that has exited.

    *   `signal`

        for a process that has received a fatal signal.

    *   `open`

        for a network, serial, or pipe connection that is open.

    *   `closed`

        for a network, serial, or pipe connection that is closed. Once a connection is closed, you cannot reopen it, though you might be able to open a new connection to the same place.

    *   `connect`

        for a non-blocking connection that is waiting to complete.

    *   `failed`

        for a non-blocking connection that has failed to complete.

    *   `listen`

        for a network server that is listening.

    *   `nil`

        if `process-name` is not the name of an existing process.

    ```lisp
    (process-status (get-buffer "*shell*"))
         ⇒ run
    ```

    For a network, serial, or pipe connection, `process-status` returns one of the symbols `open`, `stop`, or `closed`. The latter means that the other side closed the connection, or Emacs did `delete-process`. The value `stop` means that `stop-process` was called on the connection.

<!---->

*   Function: **process-live-p** *process*

    This function returns non-`nil` if `process` is alive. A process is considered alive if its status is `run`, `open`, `listen`, `connect` or `stop`.

<!---->

*   Function: **process-type** *process*

    This function returns the symbol `network` for a network connection or server, `serial` for a serial port connection, `pipe` for a pipe connection, or `real` for a subprocess created for running a program.

<!---->

*   Function: **process-exit-status** *process*

    This function returns the exit status of `process` or the signal number that killed it. (Use the result of `process-status` to determine which of those it is.) If `process` has not yet terminated, the value is 0. For network, serial, and pipe connections that are already closed, the value is either 0 or 256, depending on whether the connection was closed normally or abnormally.

<!---->

*   Function: **process-tty-name** *process*

    This function returns the terminal name that `process` is using for its communication with Emacs—or `nil` if it is using pipes instead of a pty (see `process-connection-type` in [Asynchronous Processes](Asynchronous-Processes.html)). If `process` represents a program running on a remote host, the terminal name used by that program on the remote host is provided as process property `remote-tty`. If `process` represents a network, serial, or pipe connection, the value is `nil`.

<!---->

*   Function: **process-coding-system** *process*

    This function returns a cons cell `(decode . encode)`, describing the coding systems in use for decoding output from, and encoding input to, `process` (see [Coding Systems](Coding-Systems.html)).

<!---->

*   Function: **set-process-coding-system** *process \&optional decoding-system encoding-system*

    This function specifies the coding systems to use for subsequent output from and input to `process`. It will use `decoding-system` to decode subprocess output, and `encoding-system` to encode subprocess input.

Every process also has a property list that you can use to store miscellaneous values associated with the process.

*   Function: **process-get** *process propname*

    This function returns the value of the `propname` property of `process`.

<!---->

*   Function: **process-put** *process propname value*

    This function sets the value of the `propname` property of `process` to `value`.

<!---->

*   Function: **process-plist** *process*

    This function returns the process plist of `process`.

<!---->

*   Function: **set-process-plist** *process plist*

    This function sets the process plist of `process` to `plist`.

Next: [Input to Processes](Input-to-Processes.html), Previous: [Deleting Processes](Deleting-Processes.html), Up: [Processes](Processes.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
