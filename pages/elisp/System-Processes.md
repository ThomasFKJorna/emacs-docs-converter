

### 38.12 Accessing Other Processes

In addition to accessing and manipulating processes that are subprocesses of the current Emacs session, Emacs Lisp programs can also access other processes running on the same machine. We call these *system processes*, to distinguish them from Emacs subprocesses.

Emacs provides several primitives for accessing system processes. Not all platforms support these primitives; on those which don’t, these primitives return `nil`.

### Function: **list-system-processes**

This function returns a list of all the processes running on the system. Each process is identified by its PID, a numerical process ID that is assigned by the OS and distinguishes the process from all the other processes running on the same machine at the same time.

### Function: **process-attributes** *pid*

This function returns an alist of attributes for the process specified by its process ID `pid`. Each association in the alist is of the form `(key . value)`, where `key` designates the attribute and `value` is the value of that attribute. The various attribute `key`s that this function can return are listed below. Not all platforms support all of these attributes; if an attribute is not supported, its association will not appear in the returned alist.

*   `euid`

    The effective user ID of the user who invoked the process. The corresponding `value` is a number. If the process was invoked by the same user who runs the current Emacs session, the value is identical to what `user-uid` returns (see [User Identification](User-Identification.html)).

*   `user`

    User name corresponding to the process’s effective user ID, a string.

*   `egid`

    The group ID of the effective user ID, a number.

*   `group`

    Group name corresponding to the effective user’s group ID, a string.

*   `comm`

    The name of the command that runs in the process. This is a string that usually specifies the name of the executable file of the process, without the leading directories. However, some special system processes can report strings that do not correspond to an executable file of a program.

*   `state`

    The state code of the process. This is a short string that encodes the scheduling state of the process. Here’s a list of the most frequently seen codes:

    *   `"D"`

        uninterruptible sleep (usually I/O)

    *   `"R"`

        running

    *   `"S"`

        interruptible sleep (waiting for some event)

    *   `"T"`

        stopped, e.g., by a job control signal

    *   `"Z"`

        zombie: a process that terminated, but was not reaped by its parent

    For the full list of the possible states, see the manual page of the `ps` command.

*   `ppid`

    The process ID of the parent process, a number.

*   `pgrp`

    The process group ID of the process, a number.

*   `sess`

    The session ID of the process. This is a number that is the process ID of the process’s *session leader*.

*   `ttname`

    A string that is the name of the process’s controlling terminal. On Unix and GNU systems, this is normally the file name of the corresponding terminal device, such as `/dev/pts65`.

*   `tpgid`

    The numerical process group ID of the foreground process group that uses the process’s terminal.

*   `minflt`

    The number of minor page faults caused by the process since its beginning. (Minor page faults are those that don’t involve reading from disk.)

*   `majflt`

    The number of major page faults caused by the process since its beginning. (Major page faults require a disk to be read, and are thus more expensive than minor page faults.)

*   *   `cminflt`
    *   `cmajflt`

    Like `minflt` and `majflt`, but include the number of page faults for all the child processes of the given process.

*   `utime`

    Time spent by the process in the user context, for running the application’s code. The corresponding `value` is a Lisp timestamp (see [Time of Day](Time-of-Day.html)).

*   `stime`

    Time spent by the process in the system (kernel) context, for processing system calls. The corresponding `value` is a Lisp timestamp.

*   `time`

    The sum of `utime` and `stime`. The corresponding `value` is a Lisp timestamp.

*   *   `cutime`
    *   `cstime`
    *   `ctime`

    Like `utime`, `stime`, and `time`, but include the times of all the child processes of the given process.

*   `pri`

    The numerical priority of the process.

*   `nice`

    The *nice value* of the process, a number. (Processes with smaller nice values get scheduled more favorably.)

*   `thcount`

    The number of threads in the process.

*   `start`

    The time when the process was started, as a Lisp timestamp.

*   `etime`

    The time elapsed since the process started, as a Lisp timestamp.

*   `vsize`

    The virtual memory size of the process, measured in kilobytes.

*   `rss`

    The size of the process’s *resident set*, the number of kilobytes occupied by the process in the machine’s physical memory.

*   `pcpu`

    The percentage of the CPU time used by the process since it started. The corresponding `value` is a floating-point number between 0 and 100.

*   `pmem`

    The percentage of the total physical memory installed on the machine used by the process’s resident set. The value is a floating-point number between 0 and 100.

*   `args`

    The command-line with which the process was invoked. This is a string in which individual command-line arguments are separated by blanks; whitespace characters that are embedded in the arguments are quoted as appropriate for the system’s shell: escaped by backslash characters on GNU and Unix, and enclosed in double quote characters on Windows. Thus, this command-line string can be directly used in primitives such as `shell-command`.
