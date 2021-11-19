

Next: [System Processes](System-Processes.html), Previous: [Sentinels](Sentinels.html), Up: [Processes](Processes.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 38.11 Querying Before Exit

When Emacs exits, it terminates all its subprocesses. For subprocesses that run a program, it sends them the `SIGHUP` signal; connections are simply closed. Because subprocesses may be doing valuable work, Emacs normally asks the user to confirm that it is ok to terminate them. Each process has a query flag, which, if non-`nil`, says that Emacs should ask for confirmation before exiting and thus killing that process. The default for the query flag is `t`, meaning *do* query.

*   Function: **process-query-on-exit-flag** *process*

    This returns the query flag of `process`.

<!---->

*   Function: **set-process-query-on-exit-flag** *process flag*

    This function sets the query flag of `process` to `flag`. It returns `flag`.

    Here is an example of using `set-process-query-on-exit-flag` on a shell process to avoid querying:

    ```lisp
    (set-process-query-on-exit-flag (get-process "shell") nil)
         ⇒ nil
    ```

<!---->

*   User Option: **confirm-kill-processes**

    If this user option is set to `t` (the default), then Emacs asks for confirmation before killing processes on exit. If it is `nil`, Emacs kills processes without confirmation, i.e., the query flag of all processes is ignored.
