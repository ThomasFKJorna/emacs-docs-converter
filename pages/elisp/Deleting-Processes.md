

### 38.5 Deleting Processes

*Deleting a process* disconnects Emacs immediately from the subprocess. Processes are deleted automatically after they terminate, but not necessarily right away. You can delete a process explicitly at any time. If you explicitly delete a terminated process before it is deleted automatically, no harm results. Deleting a running process sends a signal to terminate it (and its child processes, if any), and calls the process sentinel. See [Sentinels](Sentinels.html).

When a process is deleted, the process object itself continues to exist as long as other Lisp objects point to it. All the Lisp primitives that work on process objects accept deleted processes, but those that do I/O or send signals will report an error. The process mark continues to point to the same place as before, usually into a buffer where output from the process was being inserted.

### User Option: **delete-exited-processes**

This variable controls automatic deletion of processes that have terminated (due to calling `exit` or to a signal). If it is `nil`, then they continue to exist until the user runs `list-processes`. Otherwise, they are deleted immediately after they exit.

### Function: **delete-process** *process*

This function deletes a process, killing it with a `SIGKILL` signal if the process was running a program. The argument may be a process, the name of a process, a buffer, or the name of a buffer. (A buffer or buffer-name stands for the process that `get-buffer-process` returns.) Calling `delete-process` on a running process terminates it, updates the process status, and runs the sentinel immediately. If the process has already terminated, calling `delete-process` has no effect on its status, or on the running of its sentinel (which will happen sooner or later).

If the process object represents a network, serial, or pipe connection, its status changes to `closed`; otherwise, it changes to `signal`, unless the process already exited. See [process-status](Process-Information.html).

```lisp
(delete-process "*shell*")
     ⇒ nil
```
